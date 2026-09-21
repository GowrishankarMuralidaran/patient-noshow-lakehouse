# Enterprise Patient Attendance Lakehouse: Data, Analytics & AI Pipeline

An end-to-end, production-grade Lakehouse solution built on **Databricks**, **Delta Lake**, **dbt Cloud**, and **MLflow**. This project spans the full lifecycle of modern data stack delivery—from raw ingestion to predictive machine learning and natural language business intelligence.

---

## System Architecture

* **Medallion Architecture**: Bronze (Staging) → Silver (Cleaned/Enriched) → Gold (Business Aggregates)
* **Data Transformation & Quality**: dbt Cloud running automated unit tests on Databricks SQL Warehouse
* **Data Governance & Drift**: Databricks Lakehouse Monitoring & Unity Catalog metadata persistence
* **Predictive AI**: XGBoost classifier logged to MLflow with full signature tracking in Unity Catalog
* **Conversational AI/BI**: Databricks Genie Space configured for self-service natural language querying

---

## Tech Stack & Key Components

| Domain | Key Tools & Frameworks | Implementation |
| :--- | :--- | :--- |
| **Data Engineering** | Databricks, Delta Lake, Databricks SDK | Medallion architecture, automated dbt Cloud orchestration, Lakehouse Snapshot Quality Monitors. |
| **Analytics Engineering** | dbt Cloud, Unity Catalog, SQL | Modular staging/mart models, schema constraints (`unique`, `not_null`), persistent catalog docs. |
| **AI & MLOps** | PySpark, XGBoost, Scikit-Learn, MLflow | Binary classification predicting no-shows, hyperparameter tracking, registered model signatures. |
| **Generative AI & BI** | Databricks AI/BI Genie | Natural language interface querying `fct_neighbourhood_performance`. |

---

## Analytics Engineering & Semantic Layer (dbt)

The pipeline transforms raw booking logs into structured Silver and Gold layers:
* `stg_patient_appointments`: Standardizes schema types, calculates wait times, and flags binary targets.
* `fct_neighbourhood_performance`: Aggregates neighborhood-level no-show percentages, average waiting days, and total SMS reminders sent.

---

## Predictive Model & MLOps (MLflow)

Trained an XGBoost classification model to predict patient appointment attendance (`is_noshow`).
* **Input Features**: `age`, `has_scholarship`, `has_hypertension`, `has_diabetes`, `has_alcoholism`, `disability_level`, `sms_reminder_sent`, `waiting_days`.
* **Model Registry**: Registered under `workspace.default.patient_noshow_model` using explicit schema signatures (`infer_signature`) for Unity Catalog governance.

---

## Natural Language Self-Service (Databricks Genie)

Configured a **Databricks AI/BI Genie Space** on the Gold layer (`fct_neighbourhood_performance`) to allow non-technical business stakeholders to ask natural language questions (e.g., *"Which neighborhood has the highest no-show rate?"*).

---

## How to Run

1. **dbt Pipeline**: Execute `dbt build` inside `dbt_project/` to run SQL transformations and schema tests.
2. **ML Pipeline**: Execute `python ml_ops/01_predictive_noshow_model.py` on Databricks Serverless Compute to log hyperparameters and register the model.
3. **Data Monitoring**: Execute `python data_ops/02_lakehouse_monitoring.py` to trigger snapshot metrics and track dataset drift over time.
