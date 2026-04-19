Here is a comprehensive and detailed professional course syllabus for **Salesforce Data Cloud Fundamentals** (historically known within the ecosystem as Data 360 / Customer 360 Audiences). 

As an expert in the platform, I have structured your outline into a formal curriculum, complete with learning objectives, deep-dive topic expansions, and practical lab exercises to ensure participants move from theory to practical application.

---

# ☁️ Course Syllabus: Salesforce Data Cloud (Data 360) Fundamentals

**Target Audience:** Salesforce Administrators, Data Architects, Marketing Technology Professionals, and Implementation Consultants.
**Course Goal:** To provide a foundational to intermediate understanding of how to ingest, harmonize, unify, analyze, and activate customer data using Salesforce Data Cloud.

---

## Module 1: Introduction to Salesforce Data 360
*Understand the platform's architecture, business value, and administrative foundations.*

* **Define Salesforce Data 360:** Understanding the evolution of the platform (from CDP to Data Cloud) and its role as a hyperscale data engine built into the Einstein 1 platform.
* **Explore Core Capabilities and Benefits:** How the platform solves fragmented customer data issues, enables real-time personalization, and powers AI.
* **Navigate Key Features and Functionality:** A guided tour of the UI, including Data Streams, Data Spaces, Identity Resolution, and Segments.
* **Examine Data 360 Use Cases:** Real-world scenarios spanning Marketing (targeted campaigns), Service (unified agent consoles), and Sales (lead scoring).
* **Review Provisioning and Configuration Options:** Steps for initial org setup, enabling Data Cloud, and managing the core infrastructure.
* **Identify Roles and Permissions:** Best practices for assigning standard permission sets (e.g., Data Cloud Admin, Data Cloud Marketer, Data Awareness).
* **🛠️ Hands-on Lab:** *Navigating the Data Cloud Interface and Assigning User Permissions.*

## Module 2: Data Modeling Overview
*Grasp the fundamental concepts of relational database design before applying them to Salesforce.*

* **Define Key Data Modeling Terminology:** Entities, attributes, relationships, and schemas.
* **Identify Data Modeling Objects, Fields, and Relationships:** How tables connect (One-to-One, One-to-Many, Many-to-Many).
* **Compare Normalized and Denormalized Data Structures:** * *Normalized:* Highly structured, reduces redundancy (ideal for standard relational databases).
    * *Denormalized:* Flatter, wider tables optimized for fast querying and read-heavy analytics (common in Data Lakes).
* **Define Primary and Foreign Keys:** Understanding how unique identifiers connect disparate datasets.
* **🛠️ Hands-on Lab:** *Whiteboarding a basic Relational Data Model for a fictitious retail brand.*

## Module 3: Data Preparation
*Learn the critical steps required before bringing data into the platform.*

* **Review Data Usage Ethics:** Navigating compliance (GDPR, CCPA), consent management, and the ethical use of consumer data.
* **Identify Data Sources:** Evaluating the quality, format, and frequency of data from CRMs, ERPs, web analytics, and external databases.
* **Log Data Using a Data Dictionary:** How to document source fields, data types, and transformation requirements to ensure a smooth ingestion process.
* **🛠️ Hands-on Lab:** *Creating a Data Dictionary template to audit a sample legacy system's customer data.*

## Module 4: Data Modeling with Salesforce Data 360
*Translate generic data modeling concepts into Data Cloud's specific architecture.*

* **Examine the Salesforce Customer 360 Data Model:** Deep dive into the standard subject areas (Party, Engagement, Sales Order, Product).
* **Compare Data Model Types:** Understanding the flow from **Data Source Objects (DSO)** to **Data Lake Objects (DLO)** and finally to **Data Model Objects (DMO)**.
* **Define Data 360 Object Categorization:** * *Profile:* Who the customer is (e.g., Individual, Contact Point Email).
    * *Engagement:* What the customer did (e.g., Email Open, Web Purchase, point-in-time events).
    * *Other:* Reference data (e.g., Product Catalog).
* **Identify Functionality of Primary and Foreign Keys in Data 360:** How the platform uses Fully Qualified Keys (FQK) to prevent primary key collisions from multiple distinct data sources.
* **🛠️ Hands-on Lab:** *Categorizing sample datasets into Profile vs. Engagement DMOs.*

## Module 5: Data Ingestion
*Master the mechanics of bringing external and internal data into Data Cloud.*

* **Define the Data Ingestion Process:** The lifecycle of a Data Stream.
* **Examine Data Space Use Cases:** Using Data Spaces to logically separate data, metadata, and processes by brand, region, or department for security and governance.
* **Identify Connectors and Data Sources:** Configuring out-of-the-box connectors (Salesforce CRM, Marketing Cloud, Amazon S3, Google Cloud Storage, B2C Commerce, Web/Mobile SDKs).
* **Explore Data Ingestion Features:** Handling schema changes, scheduling data refreshes, and managing load errors.
* **Define Batch vs. Streaming Data Transforms:** When to use scheduled daily loads versus real-time streaming ingestion, and how to use Data Transforms (visual or SQL-based) to clean data upon entry.
* **🛠️ Hands-on Lab:** *Setting up a Data Stream via an Amazon S3 connector and configuring a CRM data bundle.*

## Module 6: Data Harmonization
*Standardize disparate data sources into a single, cohesive vocabulary.*

* **Define Key Data Harmonization Concepts:** The process of mapping customized source data (DLOs) into standard canonical models (DMOs).
* **Understand the Value of Data Harmonization:** Why harmonization is the prerequisite for accurate identity resolution and segmentation.
* **Explore Data 360 Mapping Capabilities:** Using the visual data mapping canvas, utilizing formula fields during mapping to clean data (e.g., standardizing text casing), and managing complex relationships.
* **🛠️ Hands-on Lab:** *Mapping a legacy POS system's transaction DLO to the standard Sales Order DMO.*

## Module 7: Unified Profiles
*Solve the "fragmented identity" problem by stitching together customer records.*

* **Define Identity Resolution:** The engine that connects multiple records representing the same person.
* **Review the Profile Unification Process:** How source profiles become unified individuals.
* **Unified Profile vs. Golden Record:** Understanding why Data Cloud creates a "Unified Profile" (a dynamic, point-in-time view) rather than statically overwriting source data.
* **Configure Identity Resolution Rulesets:**
    * *Match Rules:* Determining *who* is the same person (e.g., Fuzzy Name + Exact Email, Device ID matching).
    * *Reconciliation Rules:* Determining *which data survives* when conflicts occur (e.g., Most Frequently Occurring, Last Updated, Source Priority).
* **Inspect Identity Resolution Results:** Using the Identity Resolution dashboard to analyze consolidation rates and validate rule effectiveness.
* **🛠️ Hands-on Lab:** *Building and running an Identity Resolution ruleset and querying the resulting Unified Individual DMO.*

## Module 8: Data-Driven Insights
*Generate actionable metrics and KPIs directly from your unified data.*

* **Define Calculated Insights:** Multi-dimensional, SQL-based metrics calculated in batches (e.g., Lifetime Value, RFM scores, Most Viewed Category).
* **Define Streaming Insights:** Metrics calculated continuously in near-real-time using windowing functions (e.g., "Has the user abandoned a cart in the last 15 minutes?").
* **Key Differences:** Batch processing vs. real-time processing; complex joins vs. single-stream analysis.
* **Review Options for Building Insights:** Using the Visual Builder versus writing custom SQL.
* **Examine Operator Use Cases:** Leveraging formulas within Insights to aggregate data (SUM, AVG, COUNT, MAX).
* **🛠️ Hands-on Lab:** *Using the Visual Builder to create a "Total Lifetime Value by Channel" Calculated Insight.*

## Module 9: Data Segmentation and Data Actions
*Put your data to work by targeting audiences and triggering downstream processes.*

* **Define Key Data Segmentation Terminology:** Populations, attribute libraries, and segment counts.
* **Create Data Segments in Data 360:** Using the drag-and-drop segmentation canvas to query unified attributes and related engagement data.
* **Implement Calculated Insights in Segmentation:** Filtering audiences based on the metrics built in Module 8 (e.g., "Target Unified Profiles where LTV > $1000").
* **Publish Data Segments Using Activations:** Routing audiences to specific Activation Targets (e.g., Marketing Cloud for email journeys, Meta/Google for advertising).
* **Trigger Targeted Automations Using Data Actions:** Setting up real-time triggers using Webhooks or Salesforce Platform Events when specific data conditions are met (e.g., alerting a sales rep immediately when a high-value lead exhibits buying signals).
* **🛠️ Hands-on Lab:** *Building a "High-Risk Churn" segment, attaching contact attributes, and publishing it to a Marketing Cloud Activation Target.*
