# 💳 Financial Transaction Analytics & Fraud Detection Dashboard

[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![SQL Server](https://img.shields.io/badge/SQL_Server-SSMS-CC292B?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)](https://www.microsoft.com/sql-server)
[![Power BI](https://img.shields.io/badge/Power_BI-DAX_%26_Power_Query-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Project Status](https://img.shields.io/badge/Status-In_Progress_(Phase_1_Complete)-00A86B?style=for-the-badge)](#-project-roadmap--phase-status)

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
| **Database & Modeling** | `SQL Server` (`SSMS`, `pyodbc`) | Staging table creation, relational keys, index optimization, and Star Schema analytical views. |
| **Analytics & BI** | `Power BI` (`DAX`, `Power Query`) | Import Mode connection, data modeling ($1:*$), measure calculations, and executive dashboards. |
| **Version Control** | `Git` / `GitHub` | Repository tracking, code versioning, and documentation. |

---

## 🗺️ System Architecture & Workflow

```text
[ Raw Files ] ──(Python / Pandas)──> [ Clean CSVs ] ✅ ──(Import / pyodbc)──> [ SSMS: Staging ]
(CSV / JSON)         COMPLETED                                                  Clean Raw Tables
                                                                                       │
                                                                            (SQL DDL & Views)
                                                                                       ▼
[ Power BI Dashboard ] <──(Import Mode)── [ SSMS: Star Schema ] <────────── [ SSMS: Data Mart ]
  Visualization & DAX                       Fact & Dimensions                    Views / Transformation
```

---

## 🚦 Project Roadmap & Phase Status

### ① Phase 1: Data Preparation & Cleaning in Python (Pandas)
**STATUS:** `COMPLETED` ✅

* **JSON Flattening:** Parsed and normalized semi-structured `mcc_codes.json` and `train_fraud_labels.json` into tabular DataFrames.
* **Data Sanitization:** Standardized transaction amounts (`amount`), enforced strict `datetime` formatting, and handled null/duplicate entries.
* **Data Export:** Exported standardized CSV files to the `/cleaned/` directory ready for database staging.

---

### ② Phase 2: Ingestion & Structuring in SQL Server (SSMS)
**STATUS:** `PENDING ⏳ (Next Step)`

* **Database DDL:** Provision the core database `FraudAnalyticsDB` in Microsoft SQL Server.
* **Staging Area (`stg_`):** Bulk load cleaned datasets into target staging tables:
  * `stg_transactions`
  * `stg_cards`
  * `stg_users`
  * `stg_mcc`
  * `stg_fraud_labels`
* **Performance Tuning:** Define Primary Keys (PKs), Foreign Keys (FKs), and non-clustered indexes to accelerate joining operations.

---

### ③ Phase 3: Transformation & Dimensional Modeling (Star Schema)
**STATUS:** `PENDING ⏳`

* **Target Variable Integration:** Join transaction staging records with ground-truth labels (`is_fraud`).
* **Analytical Views (Data Mart):**
  * `vw_Fact_Transactions` (Core Fact Table containing transaction metrics and flags)
  * `vw_Dim_Users` (User demographics, income brackets, age groups)
  * `vw_Dim_Cards` (Card types, credit limits, issuance details)
  * `vw_Dim_MCC` (Merchant Category Codes and human-readable industry names)
  * `vw_Dim_Calendar` (Time intelligence dimension for historical trend analysis)

---

### ④ Phase 4: Connection & DAX Data Modeling in Power BI
**STATUS:** `PENDING ⏳`

* **Import Mode Connection:** Connect Power BI to SQL Server views (`vw_`).
* **Star Schema Relationship Validation:** Enforce single-direction $1:*$ relationships across all dimensions.
* **Core DAX KPI Metrics:**
  * **Total Volume ($):** `SUM(Fact[Amount])`
  * **Total Transactions:** `COUNTROWS(Fact)`
  * **Fraud Losses ($):** `CALCULATE(SUM(Fact[Amount]), Fact[is_fraud] = 1)`
  * **Fraud Rate (%):** `DIVIDE([Fraud Losses ($)], [Total Volume ($)], 0)`

---

### ⑤ Phase 5: Interactive Visualization & Executive Dashboard
**STATUS:** `PENDING ⏳`

* **Page 1 — Executive Overview:** Top-level financial KPIs, processing volume trends, and macro fraud rates.
* **Page 2 — Risk & Anomaly Analysis:** Day-of-week vs. Hour-of-day risk heatmaps, high-risk MCC categories, and transaction distribution spikes.
* **Page 3 — Customer Demographics & Behavior:** User segmentation by income tier, age cohort, utilization rates, and international spend patterns.

---

## 📂 Repository Structure

```text
financial-fraud-analytics/
│
├── data/
│   ├── raw/                 # Raw CSVs and JSON files (git-ignored if large)
│   └── cleaned/             # Processed CSVs generated by Phase 1 scripts
│
├── scripts/
│   ├── python/
│   │   └── 01_data_cleaning.py   # Data cleaning & JSON flattening pipeline
│   └── sql/
│       ├── 01_create_database_staging.sql   # SSMS Table DDL & staging loads
│       └── 02_create_star_schema_views.sql  # Fact & Dimension views
│
├── powerbi/
│   └── Fraud_Analytics_Report.pbix   # Interactive Power BI Report
│
├── docs/
│   └── architecture_diagram.png      # Workflow diagram asset
│
├── .gitignore
├── README.md
└── requirements.txt
```

---

## 👨‍💻 Author & Contact

* **Project Owner:** [Alejandro Florez]
* **LinkedIn:** [Alejandro Florez](https://www.linkedin.com/in/alejandroflorezl/?locale=en_US)  
* **Portfolio / Upwork:** [_]
