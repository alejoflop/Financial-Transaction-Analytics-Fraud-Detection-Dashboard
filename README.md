# 💳 Financial Transaction Analytics & Fraud Detection Dashboard

[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![SQL Server](https://img.shields.io/badge/SQL_Server-SSMS_22-CC292B?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)](https://www.microsoft.com/sql-server)
[![Power BI](https://img.shields.io/badge/Power_BI-DAX_%26_Power_Query-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Project Status](https://img.shields.io/badge/Status-In_Progress_(Phase_4_Complete)-00A86B?style=for-the-badge)](#-project-roadmap--phase-status)

An end-to-end financial data engineering and business intelligence solution designed to ingest semi-structured transaction data, construct a robust SQL relational star schema, and deliver an interactive executive dashboard for fraud detection and risk analytics.

---

## 📌 Executive Summary

Financial institutions handle millions of transactional events daily, where early detection of high-risk anomalies directly mitigates monetary loss and regulatory risk. This project establishes a repeatable data pipeline and interactive analytics environment that combines transactional data, credit card metadata, user demographics, Merchant Category Codes (MCC), and confirmed fraud ground-truth labels.

### Key Objectives
* **Data Unification:** Ingest and clean disparate structured (`.csv`) and semi-structured (`.json`) datasets into a unified analytical environment.
* **Dimensional Modeling:** Architect an optimized Star Schema in SQL Server (SSMS) utilizing dedicated staging tables and production views.
* **Risk Intelligence:** Isolate key fraud drivers across merchant categories (MCCs), geographical anomalies, spending tiers, and card types.
* **Executive Decision Support:** Build a multi-page Power BI dashboard with automated DAX metrics for real-time risk tracking.

---

## 🛠️ Tech Stack & Tooling

| Domain | Tool / Technology | Usage Description |
| :--- | :--- | :--- |
| **ETL & Data Wrangling** | `Python` (`Pandas`, `NumPy`, `json`) | Ingestion, JSON flattening, type casting, missing value treatment, and clean CSV export. |
| **Database & Modeling** | `SQL Server` (`SSMS 22`, `pyodbc`) | Staging table creation, relational keys, index optimization, and Star Schema analytical views. |
| **Analytics & BI** | `Power BI` (`DAX`, `Power Query`) | Import Mode connection, data modeling ($1:*$), measure calculations, and executive dashboards. |
| **Version Control** | `Git` / `GitHub` | Repository tracking, code versioning, and documentation. |

---

## 🗺️ System Architecture & Workflow

```text
[ Raw Files ] ──(Python / Pandas)──> [ Clean CSVs ] ✅ ──(Bulk Insert / pyodbc)──> [ SSMS: Staging ] ✅
(CSV / JSON)         COMPLETED                                                        Clean Raw Tables
                                                                                                 │
                                                                                      (SQL DDL & Views)
                                                                                                 ▼
[ Power BI Dashboard ] <──(Import Mode)── [ SSMS: Star Schema ] ✅ <────────────── [ SSMS: Data Mart ] ✅
  Visualization & DAX                       Fact & Dimensions                      Views / Transformation
```

---

## 🚦 Project Roadmap & Phase Status

### ① Phase 1: Data Preparation & Cleaning in Python (Pandas)
**STATUS:** `COMPLETED` ✅

* **JSON Flattening:** Parsed and normalized semi-structured `mcc_codes.json` and `train_fraud_labels.json` into tabular DataFrames.
* **Data Sanitization:** Standardized transaction amounts (`amount`), enforced strict `datetime` formatting, and handled null/duplicate entries.
* **Data Export:** Exported standardized CSV files to the `/data/cleaned/` directory using relative, cross-platform paths.

---

### ② Phase 2: Ingestion & Structuring in SQL Server (SSMS)
**STATUS:** `COMPLETED` ✅

* **Database DDL:** Provisioned core database `FraudAnalyticsDB` in Microsoft SQL Server Management Studio (SSMS 22).
* **Staging Area (`stg_`):** Bulk loaded 13.3M+ records into target staging tables:
  * `stg_transactions` (13,305,915 rows)
  * `stg_fraud_labels` (8,914,963 rows)
  * `stg_cards` (6,146 rows)
  * `stg_users` (2,000 rows)
  * `stg_mcc` (109 rows)
* **Performance Tuning:** Defined Primary Keys (PKs) and created non-clustered indexes on `client_id`, `card_id`, `mcc`, and `date` to optimize analytical joins.

---

### ③ Phase 3: Transformation & Dimensional Modeling (Star Schema)
**STATUS:** `COMPLETED` ✅

* **Fraud Target Integration:** Joined transaction staging records with ground-truth labels (`is_fraud`), confirming **13,332 fraudulent transactions** ($1,469,648.78 in losses).
* **Analytical Views (Data Mart):**
  * `vw_Fact_Transactions`: Core Fact Table containing transaction metrics and flags.
  * `vw_Dim_Users`: User demographics, income brackets, age groups, and credit score tiers.
  * `vw_Dim_Cards`: Card types, credit limits, issuance details, and dark web flags.
  * `vw_Dim_MCC`: Merchant Category Codes and human-readable industry names.
  * `vw_Dim_Calendar`: High-performance time intelligence dimension generated via SQL Tally generator.

---

### ④ Phase 4: Connection & DAX Data Modeling in Power BI
**STATUS:** `COMPLETED` ✅

* **Import Mode Connection:** Connected Power BI to SQL Server views (`vw_`).
* **Star Schema Relationship Validation:** Enforced single-direction 1-to-Many ($1:*$) relationships across all dimensions (`vw_Dim_Users`, `vw_Dim_Cards`, `vw_Dim_MCC`, `vw_Dim_Calendar`) to `vw_Fact_Transactions`.
* **Centralized DAX Measures (`_Measures`):**
  * **Total Transactions:** `COUNTROWS(vw_Fact_Transactions)`
  * **Total Processed Volume ($):** `SUM(vw_Fact_Transactions[Amount])`
  * **Avg Transaction Amount ($):** `AVERAGE(vw_Fact_Transactions[Amount])`
  * **Total Fraud Transactions:** `CALCULATE(COUNTROWS(...), Is_Fraud = 1)`
  * **Total Fraud Losses ($):** `CALCULATE(SUM(...), Is_Fraud = 1)`
  * **% Fraud Rate (Volume):** `DIVIDE([Total Fraud Losses ($)], [Total Processed Volume ($)], 0)`
  * **% Fraud Rate (Transactions):** `DIVIDE([Total Fraud Transactions], [Total Transactions], 0)`

---

### ⑤ Phase 5: Interactive Visualization & Executive Dashboard
**STATUS:** `PENDING ⏳ (Next Step)`

* **Page 1 — Executive Overview:** Top-level financial KPIs, processing volume trends, and macro fraud rates.
* **Page 2 — Risk & Anomaly Analysis:** Day-of-week vs. Hour-of-day risk heatmaps, high-risk MCC categories, and transaction distribution spikes.
* **Page 3 — Customer Demographics & Behavior:** User segmentation by income tier, age cohort, credit utilization rates, and spending patterns.

---

## 📂 Repository Structure

```text
financial-fraud-analytics/
│
├── data/
│   ├── raw/                 # Raw CSVs and JSON files (git-ignored)
│   └── cleaned/             # Processed CSVs generated by Phase 1
│
├── notebooks/
│   └── 01_data_preparation_and_cleaning.ipynb   # Data cleaning & JSON flattening notebook
│
├── scripts/
│   ├── python/
│   │   └── 02_ingest_to_ssms.py                 # Automated SQL Server ingestion script
│   └── sql/
│       ├── 01_create_database_and_staging.sql   # SSMS Table DDL & staging definitions
│       ├── 02_bulk_insert_staging.sql          # Bulk data loading script
│       ├── 03_indexing_and_verification.sql    # Non-clustered indexing script
│       └── 04_create_star_schema_views.sql     # Relational Star Schema views
│
├── powerbi/
│   └── Fraud_Analytics_Report.pbix              # Interactive Power BI Report
│
├── docs/
│   └── architecture_diagram.png                 # Workflow diagram asset
│
├── .gitignore
├── README.md
└── requirements.txt
```

---

## 👨‍💻 Author & Contact

* **Project Owner:** Alejandro Florez
* **LinkedIn:** [Alejandro Florez](https://www.linkedin.com/in/alejandroflorezl/?locale=en_US)
