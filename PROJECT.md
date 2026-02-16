# CIS 444/544 - Semester Project

This document outlines the semester-long project for CIS 444/544 Data Analytics. This is a living document and changes, updates and expansions will be published throughout the semester. See the [Changes](#changes) section for the current list of updates over time.

## Contents

* [Scope](#scope)
* Sprints:
  * [Sprint 1: Discovery](#sprint-1-discovery)
  * [Sprint 2: MongoDB, ETL, and DW Schemas](#sprint-2-mongodb-nosql-and-star-schemas)

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
* Explore the `gift_shops` database which has been generated to match with your main Hotel database. Create an ERD for the database.
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
