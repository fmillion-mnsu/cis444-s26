# Assignment "0" &ndash; Analytics in the Wild: Business Questions and Analytics Design

## Overview / Purpose

In this course, you will design and implement a full analytics workflow: understanding and modeling data, building analytics-ready schemas, and answering business questions under realistic conditions using user-friendly dashboards accessible and understandable by non-technical professionals without database knowledge.

Unlike many “static dataset” courses, our course database will evolve over time. New data will arrive continuously and may introduce issues (schema drift, outliers, missingness, anomalies). These changes will require you to diagnose what happened and adapt your analytics pipeline -- just like happens often in real organizations.

This assignment prepares you for that long-term project by analyzing and thinking about the most important skill in analytics:

> **turning messy real-world activity into answerable business questions and designing a data system capable of answering them.**

---

## Task

Choose a **real system** you have interacted with as a user. Examples:

* course registration / LMS (MNSU E-services, D2L...)
* social media (TikTok, Instagram, Reddit, YouTube...)
* hotel or airline reservations
* food delivery / ridesharing (both customer and driver/provider)
* e-commerce and online sales (Amazon, eBay, etc.)
* banks / credit card firms
* streaming services (Netflix, Spotify, Plex, etc.)

You will propose five **business questions** for your system, and design an analytics architecture that can answer them *reliably*.

> [!TIP]
> Choose a system with a scope you can reasonably model in 2–4 pages. If the system is huge (e.g. YouTube), focus on one slice (e.g. creator monetization, comments moderation, shorts feed).

Since the actual makeup of the data infrastructures in your systems are unlikely to be known publicly, you will make reasonable assumptions about the data being stored *based on your understanding of the system* and your own intuition. As just an example, if we take the case of a social media site:

* We need user accounts with secure login
* We need a way to store *rich* posts that contain many different types of content
* We need to track likes/dislikes and comments on posts
* We need to manage *relationships among users* ("friends", "endorsements"...)
* We need to consider recommender systems that generate custom feeds for each user
* We need to track advertising revenue based on a large number of possible metrics
* We need strategies for identifying content that violates the terms of service (e.g. adult material, profoundly offensive slurs...)
* We may want to perform sentiment analysis on comments, aggregate general "feel" based on locale, and other "fuzzy analytics" tasks that often take advantage of machine learning and AI.
* We need to track user purchases (e.g. Facebook credits)
* Some social media networks allow for custom apps (e.g. Facebook games). We need a way to track *usage and engagement* of the apps.

This is not an exhaustive list - you can probably think of some other areas of concern that even a simple social media site might need to concern itself with that involve *recording and analyzing data*.

---

## Deliverable (Submit as a single Word document or PDF)

Length target: **2–4 pages**. This is a **soft target** - focus on conciseness and efficiency over length.

Include all of the following in your analysis:

* Answer the following questions:

    * What is the system?
    * Who are the stakeholders? (users, admins, business owners)
    * What outcomes does the organization care about? (retention, revenue, operational efficiency, safety, fairness, etc.)

* Think of **five (5) business questions** that:

    * support a decision
    * are measurable
    * would plausibly matter to the organization

* For **each** of your five questions, include in no more than a few sentences:

    1. Business value: What decision does the answer change?
    1. Data needed:

        * What entities are involved? (user, session, transaction, course, booking…)
        * What events must be logged? (clicks, searches, submissions, cancellations…)
        * What time granularity is required? (minute/hour/day/week)
        * What metric(s) answer the question?

    1. Analytics type. Pick one:

       * Descriptive (what happened?)
       * Diagnostic (why did it happen?)
       * Predictive (what will happen?)
       * Prescriptive (what should we do?)

    1. Limits / risks (1-3 bullet points or notes)

        Examples:

        * confounders
        * missing data
        * proxy measurements
        * selection bias
        * privacy constraints

* Describe the likely operational data model at a high level:

    * list about 8-15 tables/entities (conceptually - this is where you can make assumptions and decide how you think the data might be stored). (There is no upper limit, but you generally should not need to go beyond 15 tables - if so, your scenario is likely too complex and you should consider narrowing scope - see the tip section in the task above.)
    * indicate what data should represent primary keys
    * indicate which tables behave like event logs (typically write-only, time-focused) vs state tables (e.g. user accounts)

    You can do this as a bullet list or small diagram.

    > [!TIP]
    > You do not need to fully enumerate every entity; focus on the ones needed to support your five business questions.

* Design a high-level conceptual plan for a data warehouse that can answer your questions.

    Requirements:

    * Identify at least one **fact table** (preferably 2)
    * Identify at least two **dimensions** (one can be time, e.g. daily, weekly...)
    * Identify at least one **derived or computed metric** (e.g. conversion rate = purchases / sessions)
    * Clearly state grain for each fact table
  (e.g., *“one row per session per user”* or *“one row per transaction line item”*)

* Describe how new data might arrive:

    * batch vs stream
    * how often updates land
    * what happens when data is late or out-of-order

* Propose an **anomaly** that could happen that would break a naive analytics pipeline.

    * What would you observe? (symptoms)
    * How would you detect it? (monitoring or data validation)
    * How would you recover? (pipeline changes, backfills, schema change handling)

    Examples you can allow:

    * sudden spike due to bot traffic
    * new category values appear
    * duplicated event ingestion
    * timestamps wrong (timezone bug)
    * schema drift (a column changes semantics)
    * “data poisoning” (malicious or pathological inputs)

    > [!IMPORTANT]
    > Your anomaly must be a **data anomaly** or **pipeline/system** anomaly (not just a real-world event), and must describe how it breaks analytics.
    >
    > By "not a real-world event", I mean that "the weather was unusually cold" or "there was destructive weather" does **not** count as an anomaly, because while data collected in that context might *appear* anomalous, the data still reflects the real state of the system. Anomalies refer to *errors in the data itself*. 

---

## Submission details

* Submit: single Word or PDF document. Word preferred, formatting not critical, but PDF also OK.
* Professional writing expected, but academic research not required
* Diagrams may be hand drawn (photo embedded is fine)

---

# Rubric (50 points total)

### A) Business questions quality (15)

* 12–15: questions are decision-oriented, measurable, nontrivial
* 8–11: mixed quality, some vague questions
* 0–7: mostly “toy” questions, hard to operationalize

### B) Data requirements mapping (10)

* clear entities, events, grain, time component, metrics
* recognize confounders + proxy risk

### C) Warehouse conceptual plan (15)

* correct fact/dimension identification
* grain described

### D) Real-time + anomaly plan (10)

* realistic anomaly scenario
* detection + response described (not just “fix it”)
