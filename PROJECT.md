# CIS 444/544 - Semester Project

This document outlines the semester-long project for CIS 444/544 Data Analytics. This is a living document and changes, updates and expansions will be published throughout the semester. See the [Changes](#changes) section for the current list of updates over time.

## Contents

* [Scope](#scope)
* Sprints:
  * [Sprint 1: Discovery](#sprint-1-discovery)
  * [Sprint 2: MongoDB, ETL, and DW Schemas](#sprint-2-mongodb-nosql-and-star-schemas)
  * [Sprint 3: Data Warehousing - Populating your Star Schemas](#sprint-3-data-warehousing---populating-your-star-schemas)

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

* ***Grad Students Lead:*** Use Python (or another programming language) to write a simple **ETL tool** that will automatically migrate *all* data for your primary database (all tables and rows) from SQL Server to MongoDB in a *new database*. (Your MongoDB instance already has a few databases - especially 'gift_shops' - that you will need for later steps.)
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

* "ERD" diagrams for MongoDB `gift_shops` database
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
