# Analytics Engineering Pipeline for an EdTech Bootcamp Startup (Databricks + Delta Lake) 

## 1. Project Overview

### Problem Statement

EdTech startups need reliable analytics to understand:

* learner sign-ups vs paid conversions
* cohort performance
* session attendance
* revenue trends over time

This project simulates a **realistic analytics engineering pipeline** for an EdTech bootcamp platform using **Databricks Free Edition**, focusing on **batch processing**, **clean data modeling**, and **business-ready metrics**.

### Objective

Build an end-to-end analytics pipeline that:

* ingests raw operational data
* transforms it into clean, modeled datasets
* produces analytics tables suitable for dashboards and decision-making

---

## 2. Architecture Overview

### High-level Architecture (Medallion Pattern)

```
Raw Files (CSV / JSON)
        ↓
Bronze Delta Tables (raw ingestion)
        ↓
Silver Delta Tables (cleaned & modeled)
        ↓
Gold Tables (analytics & metrics)
```

> Even though **Unity Catalog is not available** in Databricks Free Edition, the **Medallion Architecture** is implemented using Delta tables in the workspace metastore.

**Reference (Databricks official):**
[https://docs.databricks.com/lakehouse/medallion.html](https://docs.databricks.com/lakehouse/medallion.html)

---

## 3. Environment & Constraints

### Databricks Environment

* Databricks **Free Edition**
* Workspace metastore (no Unity Catalog)
* Batch processing only
* Delta Lake tables

**What is intentionally not used**

* Unity Catalog (not available in Free Edition)
* Streaming (kept simple by design)
* Production orchestration tools
  
---

## 4. Tech Stack

| Layer         | Technology              |
| ------------- | ----------------------- |
| Compute       | Databricks Free Edition |
| Storage       | Delta Lake              |
| Processing    | Apache Spark            |
| Languages     | Python (PySpark), SQL   |
| Versioning    | GitHub                  |
| Documentation | Markdown (README)       |

### Why Delta Lake?

Delta Lake provides:

* ACID transactions
* Schema enforcement
* Time travel
* Reliable batch analytics

**Official docs:**
[https://docs.databricks.com/delta/index.html](https://docs.databricks.com/delta/index.html)

---

## 5. Data Sources (Simulated but Realistic)

The dataset represents a typical EdTech bootcamp workflow.

### Raw Input Tables

* `users` — learner signups
* `courses` — available bootcamps
* `cohorts` — course batches
* `enrollments` — user → cohort mapping
* `payments` — payment transactions
* `sessions_attended` — attendance logs

> Data is manually uploaded as CSV/JSON to simulate real operational exports.

**Databricks ingestion reference:**
[https://docs.databricks.com/ingestion/file-upload.html](https://docs.databricks.com/ingestion/file-upload.html)

---

## 6. Data Modeling Strategy

### Bronze Layer

**Purpose:** Raw ingestion with minimal transformation

Characteristics:

* Original schema preserved
* Ingestion timestamp added
* Stored as Delta tables
* No joins

Best practice reference:
[https://docs.databricks.com/ingestion/bronze-layer.html](https://docs.databricks.com/ingestion/bronze-layer.html)

---

### Silver Layer

**Purpose:** Clean, analytics-ready data

Transformations:

* Type casting
* Null handling
* Deduplication
* Business-key normalization
* Fact & dimension modeling

Tables:

* `dim_users`
* `dim_courses`
* `fact_enrollments`
* `fact_payments`
* `fact_attendance`

Reference (Data modeling on Databricks):
[https://docs.databricks.com/sql/language-manual/sql-ref-datamodeling.html](https://docs.databricks.com/sql/language-manual/sql-ref-datamodeling.html)

---

### Gold Layer

**Purpose:** Business metrics & KPIs

Examples:

* Enrollment → payment conversion rate
* Cohort completion rate
* Monthly revenue
* Attendance vs payment correlation

Gold tables are created primarily using **SQL**, reflecting how analytics teams consume data.

---

## 7. Data Quality & Reliability

Basic data quality checks include:

* Non-null primary keys
* Valid date ranges
* Deduplication logic
* Row count sanity checks between layers

> Advanced data quality frameworks (e.g., expectations frameworks) are **out of scope** for this project due to Free Edition limitations.

Databricks data quality concepts:
[https://docs.databricks.com/data-governance/data-quality.html](https://docs.databricks.com/data-governance/data-quality.html)

---

## 8. Repository Structure

```
/
├── notebooks/
│   ├── bronze_ingestion/
│   ├── silver_transformations/
│   └── gold_metrics/
├── data/
│   └── raw_files/
├── docs/
│   └── architecture.md
├── README.md
```

> Notebooks are kept thin and readable, focusing on transformation logic rather than experimentation.

---

## 9. How to Run (High Level)

1. Upload raw data files to Databricks workspace
2. Run Bronze ingestion notebooks
3. Execute Silver transformation notebooks
4. Query Gold tables using SQL
