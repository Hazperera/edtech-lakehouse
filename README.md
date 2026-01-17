# Analytics Engineering Pipeline for an EdTech Startup (Databricks + Delta Lake)

## Project Overview

EdTech startups need reliable analytics to understand:

- learner sign-ups vs paid conversions  
- cohort performance  
- session attendance  
- revenue trends over time  

This project simulates a **realistic analytics engineering pipeline** for an EdTech bootcamp platform using **Databricks Free Edition**, with a focus on:

- batch processing  
- clean data modeling  
- business-ready analytics tables  

### Objective

Build an end-to-end analytics pipeline that:

- ingests raw operational data  
- transforms it into clean, modeled datasets  
- produces analytics tables suitable for dashboards and decision-making  

---

## High-Level Architecture (Medallion Pattern)

Raw Files (CSV / JSON)
↓
Bronze Delta Tables (raw ingestion)
↓
Silver Delta Tables (cleaned & modeled)
↓
Gold Tables (analytics & metrics)


Even though **Unity Catalog is not available** in Databricks Free Edition, the **Medallion Architecture** is implemented using Delta tables in the **workspace metastore**.

---

## Environment & Constraints

### Databricks Environment

- Databricks **Free Edition**
- Workspace metastore (no Unity Catalog)
- Batch processing only
- Delta Lake tables

### What Is Intentionally Not Used

- Unity Catalog (not available in Free Edition)
- Streaming pipelines
- Production orchestration tools

These constraints are documented intentionally and discussed as trade-offs rather than limitations.

---

## Tech Stack

| Area            | Choice                  |
|-----------------|-------------------------|
| Platform        | Databricks Free Edition |
| Storage         | Delta Lake              |
| Processing      | Apache Spark            |
| Languages       | PySpark, SQL            |
| Version Control | GitHub                  |
| Documentation   | Markdown                |

Delta Lake is used for reliability, schema enforcement, and data versioning.

---

## Dataset – EdTech Bootcamp Scenario

The dataset represents a simplified but realistic EdTech workflow:

- users sign up  
- users enroll in cohorts  
- some users pay  
- some users attend sessions  
- some users drop off  

### Raw Entities

- `users`  
- `courses`  
- `cohorts`  
- `enrollments`  
- `payments`  
- `sessions_attended`  

The dataset is **synthetic, but structurally representative of real operational data**, designed to surface common analytics challenges such as joins, deduplication, and funnel analysis.

---

## Layer Responsibilities

### Bronze Layer – Raw Ingestion

**Purpose**
- Preserve source data as close to raw form as possible  
- Enable reprocessing if downstream logic changes  

**Characteristics**
- Explicit schema definitions  
- Minimal transformations  
- Ingestion timestamp added  
- No joins or business logic  

---

### Silver Layer – Clean & Modeled Data

**Purpose**
- Create reliable, analytics-ready datasets  
- Serve as the single source of truth for analytics  

**Transformations**
- Type casting and normalization  
- Deduplication using business keys  
- Referential integrity across entities  
- Separation of fact and dimension tables  

---

### Gold Layer – Business Metrics

**Purpose**
- Provide business-facing metrics and KPIs  
- Optimize data for analytical consumption  

**Characteristics**
- Pre-aggregated metrics  
- SQL-first design  
- No complex transformation logic  

---

## Data Modeling Approach

The project follows a **dimensional modeling** approach commonly used in analytics engineering.

- **Dimensions** describe entities such as users, courses, and cohorts  
- **Fact tables** capture events and transactions such as enrollments, payments, and attendance  

Each fact table has a clearly defined **grain**, documented in the transformation logic and code comments.

---

## Code and Data Storage Decisions

### Code

All code is version-controlled in GitHub, including:

- notebooks  
- PySpark transformation logic  
- SQL analytics queries  
- documentation  

Databricks Repos is used to sync the workspace with GitHub, ensuring changes are tracked and reviewable.

---

### Data

Datasets are deliberately **not stored in GitHub**.

- raw files are uploaded to DBFS  
- processed data is stored as Delta tables  

This avoids Git LFS issues and follows standard data platform practices, where data versioning is handled by the storage layer rather than source control.

---

## How Raw Data Is Loaded

Raw CSV / JSON files are uploaded manually into DBFS using the Databricks UI.


### Example Directory Structure

```
dbfs:/FileStore/data/
├── users.csv
├── courses.csv
├── cohorts.csv
├── enrollments.csv
├── payments.csv
└── sessions_attended.csv
```

---

## Repository Structure

```/
├── notebooks/
│ ├── bronze_ingestion/
│ ├── silver_transformations/
│ └── gold_metrics/
├── data/
│ └── sample/ # tiny example files only
├── docs/
│ └── architecture.md
├── README.md
```
---

## What This Project Demonstrates

- Designing analytics pipelines under real-world constraints  
- Applying Medallion Architecture without Unity Catalog  
- Using Delta Lake for reliable batch analytics  
- Separating transformation logic from analytics consumption  
- Communicating trade-offs clearly and transparently  

---

## How to Run (High Level)

1. Upload raw data files to DBFS  
2. Run Bronze ingestion notebooks  
3. Run Silver transformation notebooks  
4. Query Gold tables using SQL  

---

## Future Improvements

If moved to a paid Databricks environment:

- enable Unity Catalog for governance  
- introduce streaming ingestion  
- add automated workflows  
- integrate dbt for transformations  
- implement formal data quality checks

---  

## References:
- https://docs.databricks.com/lakehouse/medallion.html
- https://docs.databricks.com/delta/index.html
- https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/
- https://docs.databricks.com/repos/index.html
- https://docs.databricks.com/ingestion/file-upload.html
- https://docs.databricks.com/dbfs/index.html  
- https://docs.databricks.com/delta/history.html  
- https://docs.databricks.com/data-governance/unity-catalog/index.html


