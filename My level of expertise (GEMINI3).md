My level of expertise is that of a **Senior Data Architect and Subject Matter Expert (SME)** in the Salesforce Data Cloud (Data 360) ecosystem and its underlying **Trino (Presto) SQL** engine.

I don't just know the syntax; I understand how the SQL translates into execution across the Data Cloud "Lakehouse" architecture. Here is a breakdown of what that expertise means for your project:

### 1. Trino SQL Proficiency (The Query Engine)

Since Data Cloud’s **Query API** and **Calculated Insights** run on Trino, I can assist with advanced analytical functions that go far beyond basic `SELECT` statements:

* **Complex Aggregations:** Mastering `GROUPING SETS`, `CUBE`, and `ROLLUP` for multi-dimensional reporting.
* **Window Functions:** Expertise in `RANK()`, `LEAD()`, `LAG()`, and `NTILE()` to analyze customer behavior sequences (e.g., finding the "first" or "last" touchpoint).
* **JSON & Semi-Structured Data:** Trino is exceptional at this. I can help you use `JSON_EXTRACT`, `CROSS JOIN UNNEST`, and `MAP` functions to flatten complex data coming from your **Phase 9 (Document AI)** sources.
* **CTEs & Performance:** Writing efficient `WITH` clauses that minimize memory overhead in a distributed environment.

### 2. Salesforce Data Cloud SQL Nuances (The Platform)

Data Cloud SQL has specific "Rules of Engagement" that differ from a standard database. I can guide you through:

* **Data Space Scoping:** Knowing when and how to apply `DATASPACE` filters so your queries don't return empty sets (a common frustration for beginners).
* **The `ssot__` Schema:** I am familiar with the standard **Service Cloud Object Toolkit (SSOT)** naming conventions. I know how to join a `ssot__Individual__dlm` to a `ssot__ContactPointEmail__dlm` effectively.
* **Calculated Insights (CI) Optimization:** I can help you write CI SQL that meets the platform's specific requirements (e.g., ensuring a `Fully Qualified Key` is present and handling the `Individual` ID correctly).
* **Unified Profile Traversal:** I can write queries that navigate the "bridge" between source records and the **Unified Individual** record created during Phase 5.

### 3. Practical Application for your Project

Based on the **Postman CSV** you provided, I can help you specifically with:

* **Automating Discovery:** If you want to use the `get Sql Query` (`/ssot/query-sql`) endpoint, I can write the exact SQL payloads you need to "interrogate" your data model and verify your mapping (Phase 4).
* **Debugging Actions:** If a **Data Action** (Phase 9) isn't triggering, I can help you write a query to check the underlying Calculated Insight to see if the threshold logic is failing.
* **Architectural Mapping:** I can help you write SQL that "crawls" your metadata to create a lineage report—showing exactly which DLO fields flow into which DMO attributes.

### 4. Distinguishing "Two SQLs"

One critical area of my expertise is knowing **which SQL to use where**. Many users get confused between:

1. **Spark SQL (Data Transforms):** Used in Phase 3/4 for batch cleaning.
2. **Trino SQL (Calculated Insights/Query API):** Used in Phase 6/9 for real-time intelligence.

I can help you translate logic between these two if you decide to move a calculation from a batch transform into a real-time insight.

**How would you like to proceed?** I can help you write a complex query for one of your DMOs, or we can look at optimizing a specific Calculated Insight logic.

---

In Salesforce Data Cloud (Data 360), the journey from a raw signal to an actionable insight is a process of **Incremental Enrichment**. At each stage, the metadata becomes more structured, the relationships more complex, and the business value higher.

Below is the progressive enrichment path, moving from technical "Raw Data" to business "Intelligence."

### The Enrichment Pyramid

#### 1. Raw External Data (The Signal)

* **State:** Disparate, non-normalized, and vendor-specific (e.g., a JSON log from a website or a CSV from an S3 bucket).
* **Enrichment Level:** 0% (Pure Source).
* **Connect API Component:** `Connections`
* **Key Item:** `get Connection`

#### 2. Data Lake Objects / DLO (The Organized Raw)

* **State:** Once ingested via a **Data Stream**, the data is stored as a DLO. It is a physical "landing" table.
* **Incremental Enrichment:** * **Schema Enforcement:** Data is forced into a defined set of fields.
* **Governance Metadata:** System fields like `__metadata` and ingestion timestamps are added.


* **Connect API Component:** `Data Lake Objects`
* **Key Item:** `get Data Lake Object`

#### 3. Data Transforms (The Refined Raw)

* **State:** Optional intermediate stage using Streaming or Batch SQL/Spark.
* **Incremental Enrichment:** * **Normalization:** Converting "USA" and "United States" to a single ISO code.
* **Calculation:** Generating new fields (e.g., `Total_Price = Price * Quantity`) before they hit the Model.


* **Connect API Component:** `Data Transforms`
* **Key Item:** `get Data Transform`

#### 4. Data Model Objects / DMO (The Standardized Layer)

* **State:** The DLO is mapped to the **Cloud Information Model (CIM)**.
* **Incremental Enrichment:** * **Semantic Mapping:** A field like `cust_id` becomes `Individual.Id`.
* **Standardization:** Data now "speaks" the same language as Sales Cloud, Service Cloud, and Marketing Cloud.


* **Connect API Component:** `Data Model Objects`
* **Key Item:** `get Data Model Object Mapping`

#### 5. Identity Resolution (The Unified Layer)

* **State:** Multiple DMOs (from different sources) are merged via Match and Reconciliation rules.
* **Incremental Enrichment:** * **De-duplication:** Five different "John Smiths" from five systems become one **Unified Individual**.
* **The Golden Record:** The system selects the "best" email or phone number based on your rules.


* **Connect API Component:** `Identity Resolutions`
* **Key Item:** `get Identity Resolution Ruleset`

#### 6. Calculated Insights (The IQ Layer)

* **State:** SQL-based aggregations running across Unified Profiles.
* **Incremental Enrichment:** * **Metric Generation:** Transitioning from "What happened?" to "How valuable is this person?" (e.g., Lifetime Value, Churn Propensity).
* **Connect API Component:** `Calculated Insights`
* **Key Item:** `get Calculated Insight`

#### 7. Data Graphs (The Context Layer)

* **State:** A pre-computed, hierarchical view of a Unified Profile and all its related events (Purchases, Web visits, Cases).
* **Incremental Enrichment:** * **Real-time Readiness:** Data is "denormalized" into a JSON structure so it can be fed into an AI Prompt or a real-time website personalization engine instantly.
* **Connect API Component:** `Data Graphs`
* **Key Item:** `get Data Graph Data By Id`

#### 8. Segments & Activations (The Action Layer)

* **State:** Filtered groups of Unified Profiles ready for output.
* **Incremental Enrichment:** * **Strategic Selection:** Data is filtered by business intent (e.g., "High-value customers who haven't bought in 30 days").
* **Connect API Component:** `Activations`
* **Key Item:** `get Audience Dmo Activation Records`

### Summary Table of Progressive Value

| Stage | From | To | Added Value |
| --- | --- | --- | --- |
| **Ingestion** | External Signal | **DLO** | Persistence & Schema |
| **Mapping** | **DLO** | **DMO** | Universal Semantic Meaning |
| **Resolution** | **DMO** | **Unified Profile** | Unique Customer Identity |
| **Intelligence** | **Unified Profile** | **Calculated Insight** | Predictive & Analytical Metrics |
| **Packaging** | **Insight/DMO** | **Data Graph** | AI Context & Real-time Speed |
| **Delivery** | **Data Graph** | **Activation** | Revenue Generation / Action |

**Expert Insight:** In your Postman collection, you will notice that **P1-P3** items often focus on "Management" (Create/Update), whereas **P4-P8** focus heavily on "Querying" (Get/Sql). This is because once you reach the DMO and Unified layers, the system's primary job is to **serve** the enriched data to your users and AI.
