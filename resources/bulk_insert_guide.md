# Bulk Insertion: Loading Millions of Rows Efficiently

**CIS 444/544 - ETL Optimization Reference**

When your ETL pipeline needs to load thousands or millions of rows into a database, the
naive approach - issuing one `INSERT` statement per row - is almost always too slow for
production use. This guide explains *why* that is and demonstrates the correct techniques
for both SQL Server and MongoDB.

## Why Row-by-Row INSERT Is Slow

Every individual `INSERT` statement requires a round trip between your application and the
database server:

```
app -> [network] -> parse SQL -> plan query -> acquire lock -> write row -> commit -> [network] -> app
```

For a table with 1 million rows this means **1 million round trips**. Even on a fast
connection each round trip has overhead: TCP acknowledgment, query parsing, lock
acquisition, and a transaction commit (which flushes the write-ahead log to disk).

At just 1 ms of overhead per row, 1 million inserts takes **17 minutes**. In practice
it is often much worse - run times of hours are possible without optimization.


## SQL Server

### Option 1 - Batched `executemany` (baseline improvement)

The simplest improvement is to send many rows in a single call. Most Python database
drivers support `executemany`, which groups the rows into fewer network round trips:

```python
cursor.executemany(
    "INSERT INTO customers (fname, lname, email) VALUES (?, ?, ?)",
    list_of_tuples,   # [(fname, lname, email), ...]
)
```

**What changes:** instead of N round trips you get roughly N/batch_size round trips.
The database still parses and executes each row as a separate statement, but the network
cost drops significantly.

**Practical batch size:** 1,000–10,000 rows per call is a reasonable starting point.
Too small and you still have many round trips; too large and you risk memory pressure
or lock escalation.

```python
def chunked_executemany(cursor, query, rows, chunk_size=5000):
    for i in range(0, len(rows), chunk_size):
        cursor.executemany(query, rows[i:i + chunk_size])
        cursor.connection.commit()
```

Typical throughput: **10,000–50,000 rows/sec** depending on row width and server hardware.

### Option 2 - Bulk Copy (the fast path)

SQL Server has a dedicated high-throughput data loading protocol called **Bulk Copy**
(exposed via the `bcp` command-line utility, or the `SqlBulkCopy` class in .NET, or
`cursor.bulkcopy()` in the `mssql-python` driver).

Bulk copy bypasses the normal SQL execution pipeline. Instead of parsing and planning
each row as a statement, it streams rows directly into the storage engine using a
special binary protocol. The database:

- Minimally logs the operation (or not at all in simple/bulk-logged recovery mode)
- Acquires a table-level lock rather than row-level locks
- Skips query planning for each row
- Writes pages in large sequential batches

```python
result = cursor.bulkcopy(
    "dbo.customers",                        # destination table
    list_of_tuples,                         # rows: list of tuples or dicts
    column_mappings=["fname", "lname", "email"],  # column names in order
    keep_identity=False,   # let SQL Server assign IDENTITY values
    table_lock=True,       # acquire table lock for maximum throughput
    batch_size=100_000,    # commit every N rows
    timeout=3600,          # seconds; default is often only 30
)
print(result["rows_copied"], result["elapsed_time"], result["rows_per_second"])
```

Typical throughput: **100,000–500,000+ rows/sec** - a 10–50x improvement over batched `executemany`!

### Important caveats

| Concern | Detail |
|---|---|
| **IDENTITY columns** | Set `keep_identity=True` if your data already has ID values you want to preserve. Set it to `False` to let SQL Server auto-assign IDs. |
| **Foreign key constraints** | Bulk copy does not automatically check FK constraints during the load. Violations will surface later. Load parent tables first, or disable/re-enable constraints. |
| **Indexes** | You may get a small additional speedup by dropping non-clustered indexes before the bulk load and re-creating them afterward. The tradeoff is index rebuild time at the end. |
| **Timeout** | The default timeout in many drivers is 30 seconds, which is far too short for large tables. Always set an explicit timeout. |
| **Date types** | Bulk copy is strict about types. A Python `datetime` object going into a SQL `DATE` column will fail. Convert with `dt.date()` or `str(dt)[:10]` first. |

#### IDENTITY_INSERT

When loading data that already has integer IDs (e.g., restoring from a backup or loading generated data where IDs must match foreign keys), you need to tell SQL Server to accept explicit values for the identity column:

```sql
SET IDENTITY_INSERT dbo.customers ON;
-- ... your inserts ...
SET IDENTITY_INSERT dbo.customers OFF;
```

From Python:

```python
cursor.execute("SET IDENTITY_INSERT dbo.customers ON")
# ... bulk copy or executemany ...
cursor.execute("SET IDENTITY_INSERT dbo.customers OFF")
```

Only one table can have `IDENTITY_INSERT ON` at a time per session.

#### Dropping Tables with Foreign Keys

When you need to drop and reload all tables, FK constraints prevent dropping parent tables while child rows still reference them. The safest approach is to drop in reverse dependency order, and drop views that depend on the tables first:

```python
# Order matters: children before parents
drop_order = ["transaction_lines", "transactions", "customers", "hotels"]

for table in drop_order:
    cursor.execute(f"""
        IF OBJECT_ID('{table}', 'U') IS NOT NULL
            DROP TABLE {table}
    """)
```

## MongoDB

### Option 1 - `insert_many` (baseline)

Never call `collection.insert_one()` in a loop. Always use `insert_many()` with a list:

```python
collection.insert_many(list_of_dicts)
```

`insert_many` sends all documents in a single network command (up to the 16 MB BSON
document limit per batch, automatically split by the driver).

### Option 2 - Unordered inserts

By default `insert_many` uses **ordered** mode: it inserts documents one at a time in
sequence and stops at the first error. This prevents the server from parallelizing the
work.

Setting `ordered=False` tells MongoDB it can insert documents in any order, enabling
the WiredTiger storage engine to process writes in parallel across its internal
worker threads:

```python
collection.insert_many(list_of_dicts, ordered=False)
```

**When to use:** any time you know there are no duplicate `_id` values in your data -
which is always the case for freshly generated data. If there could be duplicates and
you want to skip them rather than abort, `ordered=False` also causes MongoDB to skip
the bad documents and continue rather than stopping.

Typical throughput improvement: **2–3x** over ordered inserts for large batches.

### Option 3 - Chunked inserts for very large datasets

MongoDB's driver has an internal limit on how many documents it sends per batch
(typically 100,000 documents or 48 MB of BSON, whichever comes first). For very large
collections it can be useful to chunk explicitly so you can show progress:

```python
def chunked_insert(collection, docs, chunk_size=10_000, desc="Inserting"):
    for i in range(0, len(docs), chunk_size):
        collection.insert_many(docs[i:i + chunk_size], ordered=False)
```

### Embedded documents vs. separate collections (data modeling)

MongoDB is a document database, so you have a choice that doesn't exist in SQL:
you can **embed** related data inside a parent document rather than storing it in a
separate collection with a foreign key.

**Example - storing customer data with each transaction:**

```json
{
  "_id": "...",
  "store_id": 4,
  "total": 42.50,
  "customer": {
    "fname": "Alice",
    "lname": "Smith",
    "email": "alice@example.com"
  }
}
```

**Tradeoff:**

| | Separate collection (normalized) | Embedded (denormalized) |
|---|---|---|
| Storage | Less (data stored once) | More (customer repeated per transaction) |
| Query | Requires `$lookup` (like a JOIN) | Single document read, no join |
| Update | Update customer once, everywhere | Must update every transaction for that customer |
| Use when | Data changes frequently | Data is mostly read; writes are rare after initial load |

For an analytics workload - where queries read millions of rows and updates are
rare - **embedding is usually the right choice**. It trades storage space for
dramatically faster queries.
