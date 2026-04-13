# CIS 444/544 - Semester Project

This document outlines the semester-long project for CIS 444/544 Data Analytics. This is a living document and changes, updates and expansions will be published throughout the semester. See the [Changes](#changes) section for the current list of updates over time.

## Contents

* [Scope](#scope)
* Sprints:
  * [Sprint 1: Discovery](#sprint-1-discovery)
  * [Sprint 2: MongoDB, ETL, and DW Schemas](#sprint-2-mongodb-nosql-and-star-schemas)
  * [Sprint 3: Data Warehousing - Populating your Star Schemas](#sprint-3-data-warehousing---populating-your-star-schemas)
  * [Sprint 4: ETL Optimization, Visualization and New Data Integration](#sprint-4-etl-optimization-visualization-and-new-data-integration)
  * [Sprint 5: Data Governance, ORM and Incremental Data Integration](#sprint-5-data-governance-orm-and-incremental-data-integration)
  * [Sprint 6: Dashboard, Data Product, Presentation, and Project Wrap-Up](#sprint-6-dashboard-data-product-presentation-and-project-wrap-up)
* **[Final Deliverable Package](#final-submission)**
  
## Scope

The goal for this project is to build a *comprehensive data analytics system* for a fictional organization. This project will consist of multiple components and skills:

* Database modeling, reverse engineering and diagramming
* Building advanced SQL queries including use of analytic functions and in-database programmability
* Working with data across multiple database servers and database types
* Producing analytic dashboards for non-technical users so they can understand and visualize complex data
* Agile project management methodology and documentation

## Methodology

You will work on this project in six **sprints**. Each sprint will focus on one or a few aspects of the project.

Note that you are not "restricted" to only working on certain tasks during each sprint:

* If you fall behind, you can make up slow work during the next sprint if necessary. I will not be checking your sprint logs or grading you based on specific individual task completions.
* If you wish, you may work ahead to a future sprint (if its documentation is available) to "get ahead!" 

During the first sprint you will formalize your group's Agile process:

- **Standups**: Hold a brief standup meeting at the beginning of each class session (except when conducting sprint planning or retrospectives). With 2-week sprints and 2 class meetings per week, expect approximately **3 standups per sprint**.

    Each standup should briefly cover:
    * What have you worked on since the last standup?
    * What will you work on next?
    * Are there any blockers or issues the team should know about?

        Designate a certain person, or take turns, to act as a "scribe" to capture standup notes -- these are an important portfolio artifact.

- **Sprint Planning and Retrospectives:** Conduct a **sprint planning meeting** at the start of each sprint and a **retrospective** at the end. These can be combined into a single session at the sprint boundary. Document the outcomes of these meetings.

- **Project Management:** Decide on and implement an electronic project management platform (e.g., Trello, Jira, GitHub Projects, Notion). Use this tool to track tasks, assignments, and progress throughout the semester.

### Sprint 1: Discovery

In Sprint 1, your primary task is to **get to know your database**.

All groups will be working with the **Hospitality Management Database** -- a transactional system for a hotel chain that tracks properties, room inventory, customers, events (such as check-ins and check-outs), and financial transactions. While every group shares the same database *schema*, each group's dataset is unique: the actual data has been generated with different patterns, trends, and characteristics.

> [!IMPORTANT]
> **Your dataset is unique.** As the semester progresses, your data will evolve -- new records will be added, and some data may contain quality issues that you'll need to identify and address. Part of your work as data analysts is discovering what patterns exist in *your* data and what questions *your* data can answer.

As part of this sprint, you will explore your database and produce an **entity-relationship diagram** (ERD) and a **data dictionary** documenting the table schemas you discover.

You will:

* Connect to your group's database on the cloud database server
* Explore the schema and data to understand the entities, their purposes, and their relationships
* Use reverse-engineering tools and techniques to document the database structure
* Examine the actual data to understand what information is captured and how

> [!TIP]
> **A note on the data model:** This database was designed for *transactional* (OLTP) purposes, not analytics. You may notice that some information you'd want for analysis isn't stored in the most convenient form. For example, hotel guest stays aren't stored as a single record with check-in and check-out dates -- instead, each event (check-in, check-out) is stored separately. Reconstructing a "stay" requires matching events by customer and property. This is intentional! Real-world analysts often work with data that wasn't designed with their needs in mind.

#### Query List 1: Discovery Queries

Write **at least three SQL queries** that help you understand your data and could provide value to business stakeholders. These queries should be answerable with the data you have and should produce insights that a non-technical business leader would find useful.

Your queries should explore different aspects of the business. Consider perspectives such as:

* **Revenue & Pricing** -- transaction patterns, pricing analysis, payment methods
* **Operations & Capacity** -- room utilization, property comparisons, event patterns
* **Customer Behavior** -- booking patterns, repeat customers, geographic distribution of guests
* **Temporal Patterns** -- seasonal trends, day-of-week patterns, business cycles

> [!NOTE]
> You don't need to overthink this at this stage. The goal is exploration and developing familiarity with the data. Your queries will evolve and improve throughout the semester as you learn more techniques and as additional data becomes available.

For each query, document:

1. **The business question** it answers (in plain English)
2. **The SQL query** itself
3. **A brief interpretation** of what the results tell you

Additionally, identify **at least two business questions** that you *cannot* fully answer with the current data. For each:

1. **State the question** you'd want to answer
2. **Explain what's missing** -- what additional data or data structure would you need?

These "unanswerable questions" will inform later sprints as the database evolves and you build out your data warehouse.

Use standard SQL for these queries. Don't use views, stored procedures, or other programmability features at this time.

#### Sprint 1 Deliverables

Add the following to your **final project portfolio**:

* **ERD diagram** -- a complete entity-relationship diagram for your database
* **Data dictionary** -- documentation of all tables, including column names, data types, constraints, and descriptions of what each element represents
* **Team documentation**, including:
    * Team members and any identified roles or strengths (e.g., documentation lead, SQL lead)
    * Your chosen project management platform and how you'll use it
    * Your plan for standups and sprint planning/retrospective meetings
* **Query List 1** -- your discovery queries with documentation as described above
* **Unanswerable questions** -- at least two business questions you can't yet answer, with explanation
* **Sprint documentation** -- standup notes, sprint planning notes, and retrospective notes

### Sprint 2: MongoDB, NoSQL and Star Schemas

In Sprint 2, you will learn about MongoDB and similar schemaless document databases, and you will form a process for copying your data from your SQL Server into MongoDB.

You will also begin designing and implementing your star schemas. Start with choosing **two** of your query list business scenarios, and devise **star schemas** that would specifically support those business questions. 

> [!TIP]
> Remember: Star schemas are generally **de-normalized** by design - you should plan to reshape your existing data in such a way to maximize *performance* at the expense of storage size optimization.

You will:

* ***Grad Students Lead:*** Use Python (or another programming language) to write a simple **ETL tool** that will automatically migrate *all* data for your primary database (all tables and rows) from SQL Server to MongoDB in a *new database*. (Your MongoDB instance already has a few databases - especially 'hotel' in MongoDB - that you will need for later steps.)
* Convert your Query List 1 queries into MongoDB aggregation pipelines. This will be Query List 2.
* Create appropriate indexes for your queries to ensure maximum performance
* Explore the `Hotel` database which has been generated to match with your main Hotel database. Create an ERD for the database.
  * See important notes on this below!
* Design and create ERDs for at least two star or snowflake schemas based on your Query List 1 business questions *and/or* new questions you develop with the new data you have available in MongoDB.
* Create a *separate* database in your Microsoft SQL Server instance for your data warehouse. *Don't* create tables within the `Hotel` database! In this database, create tables for your star/snowflake schemas. 

### MongoDB ERDs

Since MongoDB is not a relational database, you might ask "how can I list out entities? And what do I do about inconsitent fields?"

Answer: You can make assumptions about the dataset. There are only a couple of instances where a field is optional in our data. 

Additionally, *you need to essentially synthesize entities* for nested data objects in MongoDB collections. You should look at the data and *imagine* what you'd need to do if you wanted to make it into purely relational data, and then design your ERD accordingly.

Example: If a "user" table has a "comments" field that contains many objects, then "comment" would become a separate entity in your ERD, *even though it's not stored separately in the database*!

The challenge for you is to *understand the data* and *model it* in your head as if it were to be stored in a relational database. This knowledge will help you immensely when we move to Sprint 3 and you are moving data from MongoDB to SQL Server!

### Deliverables

Deliverables to include in your **final project portfolio** include:

* "ERD" diagrams for MongoDB `hotel` database in MongoDB
* Query List 2 (MongoDB) aggregation queries
* Code for your ETL tool for migrating from SQL Server to MongoDB
* A description of, and/or the code for, indexes you create on your collections for performance optimization
* ERD diagrams for star schemas
* SQL code for generating your data warehouse tables (you can use SSMS's script generation for this)

## Sprint 3: Data Warehousing - Populating Your Star Schemas

In Sprint 3, you will bring your data warehouse to life. The star schemas you designed in Sprint 2 are empty tables - now you'll build the ETL pipelines to populate them with real data from both of your source systems. You'll also begin thinking across data boundaries by formulating business questions that *require* data from both the hotel operations database (SQL Server) and the gift shop database (MongoDB).

> [!NOTE]
> This is a shorter sprint - approximately 10 calendar days. The scope is intentionally focused: get your data warehouse populated and working, and lay the groundwork for cross-source analytics in Sprint 4. Don't try to do everything at once. A working pipeline for one star schema is worth more than a half-finished pipeline for three.

You will:

* Build a **Python (or other language of your choice) ETL pipeline** that extracts data from your source systems (SQL Server and MongoDB), transforms it, and loads it into your SQL Server data warehouse star schemas
* **Populate all star schemas** you designed in Sprint 2, including appropriate dimension and fact tables
* Formulate **at least two new business questions** that require data from both sources
* Create **at least one new star schema** that integrates data from *both* the hotel operations database and the gift shop database which answers the business questions you selected in the previous point. 

  > [!NOTE]
  You need not create a star schema for each of your new business questions if you can answer both of them with one star schema. However, it's generally the case that each business question will use its own *fact table* - *dimensions* can be shared across fact tables however!

* Design and implement a **date dimension table** for your data warehouse based on one of your queries. If none of your queries involve dates, you can use dates in the new questions you developed in the previous points.
* Write **Query List 3** - queries against your *populated data warehouse* (*NOT* the source databases!) that answer your business questions

Below are details on the various aspects of this sprint.

### Date Dimension

Your data warehouse should include a **date dimension table** - a table with one row per day, hour, minute, etc. (per whatever granularity your analysis requires) that serves as a shared dimension across your fact tables.

Instead of storing raw dates or timestamps directly in your fact tables, your fact tables should reference the date dimension via a foreign key. This is standard practice in dimensional modeling and provides several advantages: consistent date handling across all facts, easy filtering by month/quarter/year/day-of-week, and the ability to add calendar attributes (e.g., holidays, fiscal periods) without modifying fact tables.

> [!TIP]
> Your date dimension should cover the full range of dates present in your data. Each row represents a single day and should include useful derived attributes - for example: year, month, day, day of week, and quarter. You can add more (week number, month name, is_weekend, etc.) as your analysis needs dictate.
>
> You can generate and populate this table as part of your ETL process - Python's `datetime` library makes it straightforward to generate a row for every day in a range.

### ETL Pipeline

Build a Python-based ETL tool that populates your data warehouse. Your pipeline must:

* **Extract** data from both source systems:
    * Hotel operations data from SQL Server (or from its MongoDB copy, if you prefer)
    * Gift shop data from MongoDB
  > [!NOTE]
  > You can use separate ETL tools for each business question if you prefer. The requirement is that you must write at least *one* ETL process that involves source data from both databases - this will naturally be the business questions you formed during this sprint for Query List 3. 
  >
  > In other words, it is *not* required that *all* ETL pipelines you design source data from both databases. Source data from the appropriate place given your business questions.
* **Transform** the data to fit your star schema design:
    * Denormalize related tables/documents into fact table records
    * Populate dimension tables with distinct values
    * Create surrogate keys where needed
    * Handle missing or null values appropriately
    * Map date/time values to your date dimension
* **Load** the transformed data into your SQL Server data warehouse tables

Your ETL should perform a **full refresh** - clear and reload the data warehouse from scratch. We'll explore incremental strategies in later sprints.

> [!IMPORTANT]
> **ETL Efficiency**
>
> As your ETL now spans two different database systems, think carefully about how your code interacts with each source. Your ETL should be *reasonably efficient* - meaning you should avoid obviously wasteful patterns.
>
> For example:
> * Don't execute a separate MongoDB query for every single row you read from SQL Server. Pull larger datasets into memory and work with them in Python.
> * Don't insert rows into SQL Server one at a time without batching - use transactions or bulk operations.
> * Don't query the same unchanged data repeatedly when you could cache it locally in a variable or data structure.
>
> You don't need to obsess over optimization, but your ETL documentation should include a brief explanation of your **data flow strategy** - how data moves through your pipeline and why you structured it the way you did.

#### Graduate Students: ETL Ownership

Graduate students should take the lead on writing and structuring the ETL code. All team members should contribute to the *logic and design* of transformations - deciding what gets denormalized, how keys are mapped, how nulls are handled - but graduate students are responsible for the implementation and code quality.

### Cross-Source Business Questions

Formulate **at least two new business questions** that *cannot* be answered using only one data source. These questions must require combining data from both the hotel operations database and the gift shop database.

For each question, document:

1. **The business question** in plain English
2. **What data is needed** from each source (hotel operations and gift shop)
3. **How the data connects** - what fields or logic link the two sources together (think about what's shared between them and what isn't!)

> [!TIP]
> Consider questions like:
> * Is there a relationship between length of stay and gift shop spending at the same property?
> * Do properties with higher average room rates also see higher gift shop revenue?
> * Are gift shop purchase patterns different for hotel guests vs. non-guest visitors?
>
> Remember: customers are *partially* duplicated across the two systems with no direct ID link. Part of your challenge - now and in Sprint 4 - is figuring out how to reason about data that doesn't neatly join together.

Design and implement **at least one new star schema** to support these cross-source questions. This schema should have fact table(s) that incorporate data from both the hotel and gift shop systems. You may share dimension tables (e.g., a property dimension, a date dimension) across your existing and new schemas.

### Query List 3: Data Warehouse Queries

Write queries against your **populated data warehouse** that demonstrate the value of your star schema design. Your query list should include:

* **At least one query per star schema** - showing that each schema is populated and functional
* **At least one query** that answers one of your new cross-source business questions
* For each query, include:
    * The business question it answers
    * The SQL query
    * A brief interpretation of the results

> [!NOTE]
> Compare the complexity of your data warehouse queries to the equivalent queries you'd need to write against the raw source data. One of the key benefits of a well-designed star schema is simpler, more readable analytical queries - your Query List 3 should demonstrate this.

### Deliverables

Add the following to your **final project portfolio**:

* **Date dimension table** - DDL and/or the ETL code that generates and populates it
* **Python ETL pipeline** source code, including:
    * Code for populating all star schemas (Sprint 2 schemas + new cross-source schema)
    * A brief written explanation of your data flow strategy (how data moves through the pipeline, what's batched or cached, and why)
* **Cross-source business questions** - at least two documented questions with the data requirements and linkage explanation described above
* **New star schema ERD** - for your cross-source schema(s)
* **Query List 3** - data warehouse queries as described above
* **Sprint documentation** - sprint planning/retrospective notes and standup notes

## Sprint 4: ETL Optimization, Visualization, and New Data Integration

Sprint 4 is a transitional sprint with two phases. In the first phase, you will improve your existing ETL pipelines and begin learning a visualization tool. In the second phase, new data will become available in your source database -- employee records, payroll, and property amenities, as well as additional data added to your existing transaction tables -- and you will integrate this data into your data warehouse using **incremental ETL** rather than rebuilding from scratch.

> [!IMPORTANT]
> **New data availability:** The employee and amenity data along with new data adding onto hotel and gift store transactions and customers will be added to your source databases during this sprint. Your instructor will announce the exact date when the new data is available. **Do not wait for this data** -- there is significant work to do in Phase 1 with your existing data. Plan your sprint accordingly.
>
> No new properties will be added during this new data load, however new *customers* may be created in both gift shop and hotel databases.

The two phases are:

**Phase 1 (Week 1):** ETL optimization and visualization tool introduction -- uses your *existing* data only

**Phase 2 (Week 2):** New data integration using incremental ETL techniques

---

### Phase 1: ETL Optimization and Visualization

#### ETL Optimization

Your Sprint 3 ETL pipeline works -- but how *well* does it work? In a production environment, ETL jobs that take too long can miss processing windows, block downstream reports, and frustrate stakeholders waiting for updated data. As a rule of thumb, most ETLs are expected to complete within short maintenance windows - often hours or even minutes. Does your current ETL tool run within an hour, or does it need to be left running overnight to even make any progress?

In this phase, you will audit and improve your existing ETL pipeline's performance.

You will:

* **Benchmark your current ETL** -- measure and record how long your full pipeline takes to run end-to-end. Break this down by stage if possible (extraction, transformation, loading).
* **Identify bottlenecks** -- examine your code for common performance issues:
    * Are you inserting rows one at a time instead of using bulk operations or batch inserts?
    * Are you running individual queries inside loops when you could pull larger datasets into memory?
    * Are you opening and closing database connections repeatedly instead of reusing them?
    * Are you not using transactions, which can cause each write of a row to auto-commit individually?
* **Implement improvements** -- refactor your ETL code to address the issues you find. Common strategies include:
    * Batching inserts using e.g. `executemany()` or equivalent bulk operations
    * Wrapping groups of writes in explicit transactions
    * Caching reference data (e.g., dimension lookups) in Python dictionaries rather than querying for each row
    * Pulling source data in bulk rather than row-by-row
* **Benchmark again** -- measure performance after your changes and document the improvement

> [!TIP]
> Even if your ETL is already reasonably fast, go through this exercise. You should be able to articulate *why* it's fast -- what design decisions you made that avoid common pitfalls. If you genuinely can't find anything to improve, document the strategies you're already using and explain why they're effective.

Document your optimization work, including before/after timings and a description of what you changed and why. This goes in your sprint journal.

#### Graduate Students: Optimization Leadership

Graduate students can lead the optimization effort, including profiling the code, identifying bottlenecks, and implementing the refactored pipeline. All team members should participate in benchmarking and documenting the results.

#### Getting Started with Visualization

A data warehouse full of well-structured data is only valuable if people can *see* and *understand* what it contains. In this phase, you will set up a visualization tool and create your first dashboard -- a small proof-of-concept to get your feet wet before building comprehensive dashboards in Sprints 5 and 6.

**Tool options:**

You may use one of the following:

* **Power BI Desktop** (recommended for most teams) -- widely used in industry, connects directly to SQL Server, and provides a drag-and-drop interface for building dashboards. Download Power BI Desktop (Report Server edition) and connect it to your data warehouse database.
    * *Mac users:* Power BI Desktop runs on Windows. You can use the ARM version of Windows or a cloud VM. Talk to me if you need help with access.
* **Python-based dashboards** (e.g., Streamlit, Plotly Dash) -- if your team is more comfortable working in Python, you may build your dashboards programmatically. This approach lets you reuse your existing database connection code and gives you full control over the presentation, though it requires more coding effort than a drag-and-drop tool. You still will want to implement the *dynamic* aspects of dashboards - the ability to "drill down" or filter the views with the dashboard's visualizations updating dynamically.

> [!NOTE]
> Both options -- PowerBI and Python tools -- have advantages and disadvantages. Power BI gives you experience with a tool you'll likely encounter in industry, while Python-based dashboards let you leverage skills you've already developed. Choose the tool that best fits your team's strengths, but commit to one -- you'll be using it through the final presentation.
>
> If you have very strong Python programmers on your team, you may enjoy the challenge of working with a Python-based dashboard tool. If not, PowerBI is recommended as its drag-and-drop interface and easy design tooling will allow you to intuitively develop your dashboard with minimal need for code.
>
> Other commercial BI tools (Tableau, Looker, etc.) should **not** be used for this project due to licensing and grading access constraints.

**For this sprint**, your visualization deliverable is intentionally small:

* Connect your chosen tool to your data warehouse
* Create **one dashboard** with **one or two visuals** (charts, graphs, tables -- whatever makes sense for the data)
* The visuals should answer or illustrate one of your existing business questions from a previous query list
* Take a screenshot for your portfolio

The goal is to prove you can get the tool working and produce *something* visual. Don't spend excessive time on aesthetics at this stage -- that comes in Sprints 5 and 6, where you will build the comprehensive dashboards you'll present to the class.

---

### Phase 2: New Data Integration and Incremental ETL

Once the new data is available (by the week of March 23rd), your source database will have been expanded with several new entities:

* **Employees** -- staff records for the hotel chain, including positions and department assignments
* **Payroll** -- biweekly paycheck records with basic tax withholding modeling (federal and state income tax)
* **Amenities** -- specific facilities at each property (e.g., swimming pool, fitness center, business center, restaurant). Note that gift shops, which you've already been working with via MongoDB, are also represented as amenities in this schema.
* Additional data for hotel and gift shop activity - your previous database ends at the end of year 2024; the addition will insert new data for part of year 2025. 

> [!NOTE]
> The new data extends the **time range** of your existing data. Your source database now contains records beyond the original date boundaries. This is important -- it means a full refresh of your data warehouse would need to process *all* the old data again plus the new data. That's exactly the problem incremental ETL solves.

#### Exploring the New Data

Before building anything, take time to understand what's been added:

* Explore the new tables and relationships
* Update your ERD to include the new entities
* Consider what business questions the employee and payroll data could answer -- think about perspectives like staffing levels relative to occupancy, labor cost analysis by property, or seasonal hiring patterns

#### Incremental ETL

In Sprint 3, your ETL performed a **full refresh** -- it cleared the data warehouse and reloaded everything from scratch. This worked fine when your dataset was manageable, but it doesn't scale. As data grows, full refreshes become slower, riskier, and wasteful when most of the data hasn't changed. (Imagine how long it would take if we had 10 years of data, and if our hotel chain was of the scale of hospitality giants like Wyndham Hotels or Marriott. These chains manage *thousands* of properties - yours only has around 250-300 properties - with *millions* of stays per day. Could your ETL system handle processing billions of records every single day?)

In this phase, you will modify your ETL pipeline to support **incremental loading** -- processing only *new or changed* data since the last ETL run.

To do this, you need a way to track what has already been loaded. Design and implement an **ETL metadata mechanism** -- a way for your pipeline to know what it loaded last time so it can determine what's new. A common approach is an ETL metadata table (sometimes called something like `__ETLMetadata` or `__ETLLog`) in your data warehouse that records information about past loads.

> [!IMPORTANT]
> **Be thoughtful about how you determine "what's new."** Not all tables have dates that can be used to find recent records. A hotel adding a new swimming pool, for instance, isn't a time-series event -- it's a new row in a reference table. Your incremental strategy needs to handle different types of changes, not just "records newer than the last load date."
>
> Document your approach in your sprint journal: what metadata you track, how you determine what's new for each table, and what assumptions or limitations your approach has.

Your incremental ETL should:

* Be able to determine what data is new or changed since the last successful load
* Process only that new/changed data and integrate it into your existing data warehouse
* Extend your date dimension to cover any new date ranges in the expanded data
* Handle the new employee, payroll, and amenity tables in addition to the existing hotel and gift shop data

> [!TIP]
> You don't need to solve every edge case perfectly. A reasonable first approach that works for the common cases -- new records appearing in your source systems -- is sufficient. Document known limitations honestly in your sprint journal.

#### Graduate Students: Incremental ETL Design

Graduate students can lead the design and implementation of the incremental ETL mechanism, including the metadata tracking approach and the logic for determining what's new. All team members should participate in deciding the strategy and documenting the design decisions.

#### New Business Questions

Formulate **at least two new business questions** that leverage the employee and/or payroll data, potentially in combination with your existing hotel operations and gift shop data. For each question, document:

1. **The business question** in plain English
2. **What data is needed** and from which source tables
3. **How this question could inform business decisions** -- why would an executive care about the answer?

> [!TIP]
> Think about questions that cross domain boundaries:
> * Is there a relationship between staffing levels and customer satisfaction or revenue?
> * How do labor costs compare across properties relative to their revenue?
> * Do properties with certain amenities generate more revenue per room than those without?
> * Are there seasonal patterns in staffing that align (or don't align) with occupancy patterns?
>
> Remember that you are adding *new* business questions - your final dashboard will give you a chance to incorporate *all* of the business questions you've come up with throughout the project!

#### New Star Schema

Design and implement **at least one new star schema** that incorporates the employee/payroll and/or amenity data. This schema should support your new business questions. As before, you may share dimension tables (e.g., property dimension, date dimension) across schemas.

Your incremental ETL pipeline should populate this new schema along with your existing ones.

### Deliverables

Add the following to your **final project portfolio**:

* **ETL optimization documentation** -- before/after benchmarks, description of what you changed, and explanation of why it improved performance (or, if already efficient, documentation of the strategies in place)
* **Visualization proof-of-concept** -- a screenshot of your first dashboard visual(s), including a note about which tool you chose and which business question the visual addresses
* **Updated ERD** -- your entity-relationship diagram updated to include the new employee, payroll, and amenity entities
* **Incremental ETL pipeline** source code, including:
    * Your metadata tracking mechanism (table DDL, code, or both)
    * A written explanation of how your pipeline determines what data is new or changed for different types of tables
* **New business questions** -- at least two documented questions leveraging the new data, with the documentation described above
* **New star schema ERD** -- for your employee/payroll/amenity schema(s)
* **Sprint documentation** -- sprint planning/retrospective notes and standup notes

## Sprint 5: Data Governance, ORM and Incremental Data Integration

Sprint 5 has two focus areas. First, you will build a **Data Governance Database (DGDB)** -- a small, separate database that tracks your ETL operations and data quality. Second, you will integrate **new source data** that will be added to your databases mid-sprint, using the incremental ETL techniques you developed in Sprint 4.

This is an intentionally lighter sprint. You should use extra time to catch up on past deliverables; you can also use the time to begin working on your dashboard if you are already caught up -- dashboard development is the primary focus of Sprint 6, but getting a head start now will pay off.

### Part 1: Data Governance Database (DGDB)

In a production analytics environment, it isn't enough for your ETL to simply run -- you need to know *when* it ran, *what* it did, and *whether the data it produced is trustworthy*. The Data Governance Database is where you track this information.

#### DGDB Setup

Create a **new database** on your SQL Server instance for your DGDB. This database is separate from both your source databases and your data warehouse.

Your DGDB should contain at minimum two tables:

**ETL_Runs** -- logs each execution of your ETL pipeline:

* Run ID (primary key)
* ETL job name or description (which pipeline or stage ran)
* Start date/time
* End date/time or duration
* Status (e.g., Success, Failure, Partial)
* Records processed
* Records rejected or skipped
* Notes or error messages (optional -- a text field for capturing log output or error details)

**Validation_Results** -- logs the outcomes of data quality checks:

* Result ID (primary key)
* Run ID (foreign key to ETL_Runs -- ties each validation to the ETL run that triggered it)
* Rule name or identifier (a short, human-readable name for the check)
* Rule description (plain English explanation of what the check verifies)
* Date/time the check was executed
* Records checked
* Records passed
* Records failed

> [!TIP]
> You're free to add additional columns or tables beyond what's listed here. For example, you might add a severity level to validation results, a separate table for detailed error records, or a lineage table that documents where each data warehouse table gets its data. The schema above is the minimum -- build what makes sense for your team.

#### Validation Rules

Implement **at least four validation rules** in your ETL code. Each rule should check a specific aspect of data quality, log its results to the `Validation_Results` table, and be associated with an ETL run.

Your validation rules don't need to be stored *in* the database -- implement them directly in your ETL code. What matters is that the *results* are logged to the DGDB so you have a record of what was checked and what passed or failed.

Your rules should cover at least one from each of the following categories:

1. **Referential integrity in the data warehouse** -- verify that foreign keys in your fact tables have matching records in the corresponding dimension tables. For example: does every `property_key` in a fact table have a matching row in the property dimension?

2. **Null/completeness checks** -- verify that critical business fields are populated. For example: no null customer names, no null transaction amounts, no null dates in fact records.

3. **Date range coverage** -- verify that your date dimension table covers the full span of dates present in your fact tables. Are there any fact records referencing dates that don't exist in your date dimension?

4. **Cross-source customer analysis** -- investigate the overlap between customers in your hotel operations database and your gift shop database. Since these two systems share *some* customers but use different IDs with no direct link, your task is to quantify the overlap: how many customers appear in both systems based on matching attributes like name and address? What percentage of gift shop customers are also hotel guests? Document your matching approach and its limitations.

> [!NOTE]
> The cross-source customer analysis is more of a **data quality investigation** than a pass/fail check. You're exploring the data to understand how well (or poorly) your two source systems align on customer identity. Log your findings to the DGDB -- for example, how many potential matches you found, what matching criteria you used, and what percentage of customers in each system had a match in the other.

For each validation rule, document:

1. **What it checks** (in plain English)
2. **Why it matters** (what could go wrong if this check fails?)
3. **How you implemented it** (brief description of the logic)

#### ETL Integration

Retrofit your existing ETL pipelines to log to the DGDB:

* At the start of each ETL run, insert a record into `ETL_Runs`
* As the ETL processes data, track record counts
* After loading, execute your validation rules and log results to `Validation_Results`
* At the end of the run, update the ETL run record with final status, duration, and counts

> [!TIP]
> You don't need to retrofit *every* ETL script you've written -- focus on your primary data warehouse ETL pipeline. If you have separate scripts for different star schemas, pick the one that's most complete and integrate DGDB logging there. You can extend to others as time permits.

**Graduate students** should lead this task and should use an **Object-Relational Mapper (ORM)** for all interactions with the DGDB. This includes creating the DGDB tables (if you choose to use the ORM's table creation features), inserting ETL run records, logging validation results, and querying the DGDB.

You may use any Python ORM:

* **SQLAlchemy** (most common in industry)
* **Peewee** (lightweight, good for smaller projects)
* **Django ORM** (if you're familiar with Django)

This is a practical application of ORMs: the DGDB tables are small, writes are infrequent, and the schema is simple -- exactly the use case where ORMs shine. Contrast this with your main ETL pipeline, where bulk loading millions of rows makes raw SQL and batch operations the better tool.

Undergraduate students may use whatever database interaction method they're comfortable with (pyodbc, raw SQL, etc.) for the DGDB.

### Part 2: New Data Integration

Once the new source data is available, your hotel and gift shop databases will contain **additional records extending into 2025**. New customers may also appear in both systems. This new data will model *specific* business conditions and anomalies. These will be one or more of the following:

* A month or similar time period of unusually high or low sales in one region or even one property
* Customers may appear that have no records of stays or gift purchases
* Evidence of an event that measurably changes the analytics (as an example, consider how COVID essentially shut down the hospitality industry overnight)
* Anomalous transactions (e.g. someone might buy an unreasonable or unusually strange amount of products at the gift store, someone might stay in a resort for three months...)
* Some properties may suddenly see increased or decreased popularity on certain weekdays
* A property or even a whole region suddenly goes "offline" for a sizable amount of time - no reservations, no gift shop purchases, etc. 
* Customer retention anomalies - frequent customers suddenly disappear, infrequent customers suddenly become very regular customers
* Potential fraud - including *but not limited to*: some customers might check in and out the same day (comp abuse), some customers might show multiple attempts to pay on multiple cards (potential "card testing")
* Financial anomalies or trends - including *but not limited to* a sudden bias in payment method usage at one property
* Duplication of customers - some customers appear twice with slightly modified but still obviously similar data (e.g. the same customer appears as two entries, with addresses ending in `Street` and `St.`)

Use your **incremental ETL pipeline** from Sprint 4 to integrate this new data into your existing data warehouse. Your incremental ETL should:

* Detect the new records based on your metadata tracking approach
* Process and load them into your existing star schemas
* Extend your date dimension to cover any new date ranges
* Log the incremental run to your DGDB (Part 1 of this sprint)

> [!NOTE]
> This is a practical test of the incremental ETL you designed in Sprint 4. If your incremental approach needs adjustments to handle the new data correctly, that's expected -- document what you changed and why in your sprint journal.

If your Sprint 4 incremental ETL isn't fully working yet, this is your opportunity to finish it. A working incremental pipeline that processes the new data and logs to the DGDB is a strong deliverable.

### Dashboard Development (Looking Ahead)

Dashboard creation is the focus of Sprint 6, but if you finish the DGDB and incremental load with time to spare, **start working on your dashboard now**. Connect your visualization tool (Power BI or Python-based) to your data warehouse and begin building visuals for your business questions.

You can also begin to consider the above noted potential anomalies or patterns in your new incoming data, and consider if you may want to add or change any of your business questions to accommodate those conditions. **You do NOT need to test or account for all possible anomalies** - each dataset will contain *many* (but not exactly the same) anomalies, so most business questions you have already come up with are likely to result in *measurable analytics outcomes* (this is one of the main "points" of analytics!).

### Deliverables

Add the following to your **final project portfolio**:

* **DGDB schema** -- DDL scripts for creating your DGDB database and tables (or, for graduate students using an ORM, the model definitions that generate the tables)
* **Validation rules documentation** -- for each rule: what it checks, why it matters, and how you implemented it. Include your cross-source customer analysis findings.
* **Updated ETL code** -- your ETL pipeline(s) modified to log runs and validation results to the DGDB. Keep your previous ETL code as a historical artifact -- store updated code separately (new file or directory).
* **DGDB evidence** -- screenshots or query output showing your DGDB tables populated with real ETL run and validation data
* **Incremental ETL results** -- evidence that your pipeline successfully processed the new source data (e.g., before/after record counts, DGDB log entries for the incremental run)
* **Sprint documentation** -- sprint planning/retrospective notes and standup notes

## Sprint 6: Dashboard, Data Product, Presentation, and Project Wrap-Up

Sprint 6 is the final sprint. Your focus is on three things: building the comprehensive analytics dashboard you'll present to the class, producing a curated data product from your data warehouse (a *data mart*), and preparing and delivering your final group presentation. You will also write individual retrospective reports and complete peer evaluations.

This sprint is where everything comes together. The data warehouse you've built, the ETL pipelines you've refined, the business questions you've developed -- all of it feeds into a dashboard that tells the story of what your data reveals. You are also producing a deliverable that demonstrates a key concept in modern data analytics: *data as a product*.

### Part 1: Analytics Dashboard

Build **one comprehensive dashboard** that draws from your data warehouse and addresses the business questions you've developed throughout the project. This dashboard is the centerpiece of your final presentation -- it demonstrates the business value of the entire analytics infrastructure you've built.

#### Dashboard Scope

Your dashboard should be comprehensive. It should incorporate business questions and insights from across your query lists and star schemas -- not just one narrow slice of the data. Think of this as the dashboard that the hotel chain's leadership team would open every morning to understand the state of the business.

You are welcome to revisit, revise, or add to your business questions at this stage. If you realize that your existing questions don't translate well into compelling visualizations, develop new ones. You may build additional star schemas or views in your data warehouse to support new questions if needed. You can design and run any additional ETL pipelines necessary, but please ensure to continue to use your DGDB database to log ETL runs and errors. Remember to provide *all* scripts you use in your final deliverables package.

#### Dashboard Requirements

Your dashboard must include:

* **At least 6 visualizations** -- each should answer a specific business question or illuminate a meaningful pattern in the data. Don't create charts for the sake of meeting a count; every visualization should earn its place on the dashboard.
* **At least 2 interactive visualizations** -- these must support parameterization that allows the viewer to update the visualization dynamically. Examples include drill-down capability (e.g., clicking a region to see property-level detail), date range selectors, slicers or filters that adjust what data is displayed, or dropdown parameters that change the metric being shown. Not every visualization needs interactivity -- some insights are best presented as a static view -- but at least two should respond to user input.
* **KPI cards or summary metrics** -- high-level numbers that give an immediate snapshot of business health (e.g., total revenue, occupancy rate, average transaction value, year-over-year growth).
* **Appropriate chart types** -- use the right visualization for the data. Time series data should use line or area charts. Categorical comparisons should use bar charts. Proportions should use pie or donut charts sparingly and only when there are few categories. Geographic data can use maps if your tool supports them. Avoid 3D charts.

#### Visualization Quality

Each visualization should:

* Answer a clear business question that you can articulate in one sentence
* Use an appropriate visualization to answer that business question
* Use proper titles, axis labels, and legends based on the exact type of visualization used
* Be interpretable at a glance by a non-technical viewer
* Use consistent formatting and color schemes across the dashboard

> [!TIP]
> Think about your dashboard as a *story*, not a collection of unrelated charts. Group related visualizations together. Use layout and visual hierarchy to guide the viewer's attention. Consider what the first thing someone sees when they open the dashboard -- that should be the most important information.

#### Computed Measures

At least one visualization in your dashboard must use a **computed or derived measure** -- a value that is calculated rather than pulled directly from a column in your data warehouse. Examples include:

* Year-over-year or month-over-month percentage change
* Rolling or moving averages
* Ratios such as revenue per room-night, labor cost as a percentage of revenue, or average gift shop spend per hotel guest
* Occupancy rates derived from room inventory and check-in/check-out events
* Customer retention or repeat-visit rates

These calculated values are also useful to present as KPI cards/summary metrics.

You may implement these computations as SQL views in your data warehouse, as calculated fields in your visualization tool (e.g., Power BI measures), or in Python if you're using a Python-based dashboard. Document what the computed measure is, how it's calculated, and which visualization uses it.

> [!NOTE]
> **Graduate students** should take the lead on designing and implementing computed measures. These are analytical calculations that go beyond simple aggregation -- they require thinking about what derived metrics would be most meaningful to business stakeholders and how to calculate them correctly.

#### Technical Reminders

* **Data connection:** Connect to your SQL Server data warehouse -- not directly to the source databases or MongoDB.
* **Tool:** Use the visualization tool you selected in Sprint 4 (Power BI Desktop or a Python-based tool such as Streamlit or Plotly Dash). If you haven't committed to a tool yet, now is the time -- Power BI Desktop (Report Server edition) is recommended for most teams.
* **Dashboard file:** Your dashboard file (`.pbix` for Power BI, or your Python source code for Python-based dashboards) is a graded deliverable and must be included in your final submission.

### Part 2: Data Product -- Gift Shop Open Dataset

Throughout this project, your data warehouse has served *your* analytical needs. But in practice, organizations also produce curated datasets intended for use by *others* -- business partners, researchers, or the general public. This is the concept of **data as a product**: a carefully prepared, documented dataset that has been extracted, cleaned, and shaped for a specific audience and purpose. This is what we refer to as a **data mart**.

> Remember that a *data mart* is simply a curated, sharable dataset. Typically, data marts are used for many purposes - sharing authoritative data snapshots between departments, used directly to feed data warehouses, and sharing with the general public. It's not important who the audience is - a data mart is simply the "product" that audience receives.

Think of sites like Kaggle, where you can find datasets on everything from Amazon product listings to airline delays. These datasets didn't appear out of thin air -- someone extracted them from production systems, removed sensitive information, selected the relevant subset of columns, and packaged them for public consumption. Decisions need to be made to ensure that published datasets serve their intended function but don't provide too much "insider knowledge" that competitors or malicious actors could abuse. For example, the Amazon product dataset on Kaggle includes product names, categories, prices, and ratings -- but it doesn't include who bought each product, how many units were sold, or any customer payment information. That's a deliberate editorial decision about what to include and what to exclude.

Your task is to produce a similar artifact: a **gift shop open dataset** suitable for public release.

#### Scenario

Your hotel chain has decided to publish an open dataset of gift shop transaction data. This dataset will be made publicly available (imagine publishing it on Kaggle or a similar platform). The goal is to share useful retail transaction data with researchers, students, and analysts -- while protecting customer privacy.

> **Note:** You should *not* actually publish your dataset online. Simply produce the dataset as described here and include it in your deliverables.

#### Requirements

Create a **new database** on your SQL Server instance (e.g., `GiftShopOpenDataset` or a name of your choosing) and use an ETL script to populate it. Your ETL should extract data from your existing data sources (your data warehouse, your MongoDB gift shop database, or both), transform it as described below, and load it into the new database.

The dataset must:

* **Include** gift shop transaction data: products, transaction details (dates, quantities, amounts), and product categories or similar organizational information
* **Exclude all customer PII** -- no customer names, addresses, email addresses, phone numbers, or any other personally identifiable information. No customer-specific transaction linkage (i.e., you should not be able to determine that "customer X bought products Y and Z"). The dataset should contain *what was sold*, not *who bought it*. You should *not* be including customer IDs of any kind in transactions (you *can* include metadata such as transaction date/time and you *should* include the *property* the transaction occurred at - see next point - but do *NOT* include any customer link.)
* **Include property-level context** -- transactions should be identifiable by property (or at minimum by region), so that analysts can compare gift shop performance across locations

> [!IMPORTANT]
> Think carefully about what "no PII" means in practice. Removing the customer name column isn't sufficient if you leave a customer ID that could be joined back to the source system. The dataset should stand alone -- someone downloading it should have no straightforward path back to individual customer identities, *even if they somehow gained access to the original full dataset*.

#### Export

Once your dataset database is populated, use SSMS to generate a **SQL script** of the database using the "Generate Scripts" wizard. Select the option to script both **schema and data**. This produces a self-contained `.sql` file that anyone could use to recreate your dataset on their own SQL Server instance -- exactly the kind of artifact you'd publish alongside a dataset on Kaggle or a data sharing platform. Don't worry about the size of the file - it is likely to be large - you may ZIP it for inclusion in your submission package. 

> [!TIP]
> In SSMS: right-click your database → Tasks → Generate Scripts. Walk through the wizard, selecting all tables. In the "Set Scripting Options" step, click "Advanced" and set "Types of data to script" to **Schema and Data**. Save to a single `.sql` file.

Include this `.sql` file in your final submission.

#### Data Card

Create a brief `README.md` file to accompany your dataset. This is your **data card** -- the documentation that a user of your dataset would read to understand what they're working with. It should include:

* A short description of the dataset (what it contains, where it came from, what time period it covers)
* A list of tables and their columns, including data types and brief descriptions
* Any notes on limitations, known issues, or caveats (e.g., "transaction amounts are in USD," "data covers January 2024 through June 2025")

This doesn't need to be long -- a page or two is sufficient. The goal is that someone encountering your dataset for the first time could read the README and understand what they have.

### Part 3: Final Presentation

Each group will deliver a **final presentation** during presentation week. You are presenting to a mixed audience of **executives and data analysts** -- your presentation should be accessible to non-technical viewers while also demonstrating the technical depth of your work.

#### Logistics

* **Time limit:** 15 minutes maximum (hard cutoff). Target **12 minutes** to leave time for brief questions and transition to the next group.
* **Format:** PowerPoint presentation with a live dashboard demonstration.
* **The PowerPoint file is a graded deliverable** and must be included in your final submission.

#### Required Elements

You should assume that you have a *combined audience* of business stakeholders and data science workers. Therefore, you *can* and *should* discuss some of the technical aspects of your implementation (you can discuss ETL, star schemas, etc.) but you should avoid unnecessary verbosity on specifics of the technical implementation (e.g. you should not show SQL queries and dive into their complexity, discuss specific programming techniques or workaround, etc.) The focus here is on *people who care about the data you're producing* - they don't necessarily care about the internals.

Your presentation must cover the following. You are free to structure and order these however you think is most effective -- there is no prescribed slide sequence.

* **Business context** -- briefly orient the audience: what organization are you analyzing, what does it do, and what questions did you set out to answer?
* **Data architecture overview** -- give the audience a high-level understanding of your data pipeline: where data comes from (source systems), how it flows through your warehouse (ETL), and how it reaches the dashboard. This doesn't need to be deeply technical -- a simple architecture diagram or data flow visual is effective here.
* **Dashboard demonstration** -- show your dashboard live. Walk through your key visualizations, demonstrate the interactive features, and highlight the insights your dashboard surfaces. This is the centerpiece of the presentation.
* **Key findings** -- what did you discover in the data? What patterns, anomalies, or insights did your analysis reveal? What would you recommend to the hotel chain's leadership based on what you found?
* **Data product** -- briefly describe the open dataset you produced: what's in it, what was excluded and why, and how it could be used by others.
* **Reflection** -- what went well, what was challenging, and what would you do differently if starting over?

> [!TIP]
> **Presenting a dashboard live** can be tricky -- small text and complex visuals don't always project well. Consider zooming in on specific visualizations as you discuss them, or including screenshots or a video of key visuals in your slides as a backup. Make sure your dashboard is connected and working *before* your presentation slot.

> [!NOTE]
> With 7 groups and 110 minutes of class time, the 15-minute cap is strict. Practice your presentation and time it. A well-rehearsed 12-minute presentation is far more effective than a rushed 15-minute one that gets cut off. You may want to designate a timekeeper in your group. During presentations I can give a signal at 10 minutes to let you know it's time to start wrapping up.

### Part 4: Individual Deliverables

Sprint 6 includes individual deliverables in addition to your group work. These are how individual contributions and learning are assessed separately from the group grade. There will be separate dropboxes on D2L for these individual submissions - **DO NOT** include any individual submissions listed here in your final deliverables package!!

#### Individual Written Reports

Each individual team member will submit **two short written reports** (1-2 pages each):

**Project Retrospective** -- reflect on the project as a whole from your individual perspective:

* What did you learn over the course of the project? What skills or concepts were new to you?
* What aspects of the project were most challenging? How did you work through those challenges?
* What would you do differently if you were starting the project over with what you know now?
* How has this project changed how you think about data analytics as a discipline?

**Group Retrospective** -- reflect on how your team worked together:

* How did your team divide work and make decisions?
* What worked well about your team's process? What didn't?
* How did your team handle disagreements, setbacks, or situations where someone fell behind?
* What would you change about how the team operated?

> [!IMPORTANT]
> These are *individual* reports -- each team member writes their own. They should reflect your personal experience and perspective, not be a copy of a shared document. Write honestly; these reports are not shared with your teammates. This is your chance to informally assess your teammates.

#### Peer Evaluation

You will complete an **anonymous peer evaluation** for each member of your group. This will be administered as a survey through D2L -- details and the link will be provided separately. The peer evaluation asks you to assess each teammate's contributions, reliability, and collaboration. Your responses are confidential and are used to inform individual grading adjustments. This will occur near the end of Sprint 6. Details will be announced in class.

## Final Submission

Your final project portfolio is due during finals week, at 11:59PM on the day that final presentations are given (May 4th). Submit either a **zip file** or provide **read access to a shared directory** (e.g., OneDrive) containing your complete project portfolio. 

> [!IMPORTANT]
> **YOU ARE RESPONSIBLE FOR ENSURING THAT I HAVE ACCESS TO YOUR SHARED FOLDER** should you decide to share a OneDrive or other shared cloud folder link. You MUST grant permission to `flint.million.2@mnsu.edu` to read the directory.
>
> As a courtesy, I will check group final submissions on Tuesday, May 5th and inform you if I have trouble accessing your submission. I will notify you *once* to resolve the issue with access and will give you an additional 24 hours to resolve the issue. **If I am still unable to access your final deliverables as of Thursday, May 8th, and you have not resolved the issue, YOUR GROUP WILL RECEIVE A SUBSTANTIAL PENALTY which will adversely affect the grade of ALL group members.**

Your submission should include all deliverables from across the entire project. Below is the complete list, organized by sprint. Use this as a checklist.

### Sprint 1: Discovery

* ERD diagram for the hotel operations database
* Data dictionary documenting all tables, columns, data types, and descriptions
* Team documentation (members, roles, project management platform, meeting plan)
* Query List 1 -- discovery queries with business questions, SQL, and interpretations
* Unanswerable questions -- at least two, with explanation of what's missing
* Sprint documentation (standup notes, sprint planning, retrospective)

### Sprint 2: MongoDB, ETL, and Star Schemas

* ERD for MongoDB `hotel` database
* Query List 2 -- MongoDB aggregation queries
* ETL tool source code for SQL Server to MongoDB migration
* Index descriptions and/or code for MongoDB performance optimization
* Star schema ERDs (at least two)
* SQL code for data warehouse table creation
* Sprint documentation

### Sprint 3: Data Warehousing

* Date dimension table DDL and/or ETL code
* Python ETL pipeline source code for populating star schemas
* Written explanation of data flow strategy
* Cross-source business questions (at least two, documented)
* New cross-source star schema ERD
* Query List 3 -- data warehouse queries
* Sprint documentation

### Sprint 4: ETL Optimization, Visualization, and New Data

* ETL optimization documentation (before/after benchmarks, changes made)
* Visualization proof-of-concept screenshot
* Updated ERD including employee, payroll, and amenity entities
* Incremental ETL pipeline source code with metadata tracking mechanism
* Written explanation of incremental ETL approach
* New business questions leveraging employee/payroll data (at least two)
* New star schema ERD for employee/payroll/amenity data
* Sprint documentation

### Sprint 5: Data Governance and Incremental Data Integration

* DGDB schema -- DDL scripts or ORM model definitions
* Validation rules documentation (what each checks, why it matters, how implemented)
* Updated ETL code with DGDB logging integration
* DGDB evidence -- screenshots or query output showing populated ETL run and validation data
* Incremental ETL results -- evidence of successful new data processing
* Sprint documentation

### Sprint 6: Dashboard, Data Product, and Presentation

* **Dashboard file** -- `.pbix` file (Power BI) or Python source code (Streamlit/Plotly Dash)
* **Dashboard documentation** -- for each visualization: the business question it answers and the data source it uses. For computed measures: what the measure is and how it's calculated.
* **Gift shop open dataset** -- the exported `.sql` script from SSMS (schema and data)
* **Data card** -- `README.md` documenting the open dataset
* **ETL code** for populating the open dataset database
* **PowerPoint file** -- the slide deck used for your final presentation
* **Individual project retrospective** (1-2 pages, submitted individually)
* **Individual group retrospective** (1-2 pages, submitted individually)
* Sprint documentation (sprint planning, standup notes, final retrospective)

You do not need to separate all documents by sprint. For example, a single Sprint Documentation file is fine if it includes all sprints.

> [!NOTE]
> The peer evaluation is completed separately through D2L and is **not** included in your portfolio submission.

> [!TIP]
> **Organization matters.** A well-organized submission is easier to grade and reflects well on your team's professionalism. Consider organizing your submission into directories by sprint or by artifact type. Include a top-level `README` or `INDEX` file that describes where to find each deliverable. If files have non-obvious names, rename them or add descriptions.

This concludes the final project for Data Analytics! I hope you've had a great semester and I wish you all the best of luck in your future endeavors. If you ever want to reach out to me for any reason, don't hesitate to E-mail me at `flint.million.2@mnsu.edu`. 