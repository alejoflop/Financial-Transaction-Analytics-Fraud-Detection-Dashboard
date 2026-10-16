# 💳 Financial Transaction Analytics & Fraud Detection Dashboard

An end-to-end financial data engineering and business intelligence solution designed to ingest semi-structured transaction data, construct a robust SQL relational star schema, and deliver an interactive executive dashboard for fraud detection and risk analytics.

---

## 📌 Executive Summary

Financial institutions handle millions of transactional events daily, where early detection of high-risk anomalies directly mitigates monetary loss and regulatory risk. This project establishes a repeatable data pipeline and interactive analytics environment combining transactional data, credit card metadata, user demographics, Merchant Category Codes (MCC), and confirmed fraud ground-truth labels.

### Key Objectives
* **Data Unification:** Ingest and clean disparate structured (`.csv`) and semi-structured (`.json`) datasets into a unified analytical environment.
* **Dimensional Modeling:** Architect an optimized Star Schema in SQL Server (SSMS) utilizing dedicated staging tables and production views.
* **Risk Intelligence:** Isolate key fraud drivers across merchant categories (MCCs), geographical anomalies, spending tiers, and card types.
* **Executive Decision Support:** Build a multi-page Power BI dashboard with automated DAX metrics and VertiPaq model optimizations for real-time risk tracking.

---

## 📊 Data Source

The raw datasets used in this project originate from Kaggle's public financial datasets:
* **Source:** [Financial Transactions Dataset: Analytics]([https://www.kaggle.com/](https://www.kaggle.com/datasets/computingvictor/transactions-fraud-datasets))
* **Contents:**
  * `transactions_data.csv` (13.3M+ records)
  * `cards_data.csv` (6.1K card metadata records)
  * `users_data.csv` (2.0K customer demographic records)
  * `mcc_codes.json` (Merchant Category Code lookup dictionary)
  * `train_fraud_labels.json` (Ground-truth fraud classification flags)

> *Note: Due to file size constraints, raw data files are not hosted in this repository. Download the dataset directly from Kaggle and place the raw files in a local `data/raw/` folder before running the Jupyter Notebook.*

---

## 🛠️ Tech Stack & Tooling

| Domain | Tool / Technology | Usage Description |
| :--- | :--- | :--- |
| **ETL & Data Wrangling** | `Python` (`Pandas`, `NumPy`, `json`) | Ingestion, JSON flattening, type casting, missing value treatment, and clean CSV export. |
| **Database & Modeling** | `SQL Server` (`SSMS 22`) | Staging table creation, relational keys, index optimization, and Star Schema analytical views. |
| **Analytics & BI** | `Power BI` (`DAX`, `Power Query`) | Import Mode connection, data modeling ($1:*$), VertiPaq engine optimization, and interactive executive reporting. |
| **Version Control & Sharing** | `Git` / `GitHub` | Repository tracking, code versioning, `.pbit` template distribution, and `.pdf` dashboard export. |

---

## 🗺️ System Architecture & Workflow

```text
[ Raw Kaggle Data ] ──(Python / Pandas)──> [ Clean CSVs ] ✅ ──(Bulk Insert)──> [ SSMS: Staging ] ✅
 (CSV / JSON)           COMPLETED                                                Clean Raw Tables
                                                                                         │
                                                                              (SQL DDL & Views)
                                                                                         ▼
[ Power BI Dashboard ] <──(Import Mode)── [ SSMS: Star Schema ] ✅ <────────── [ SSMS: Data Mart ] ✅
  Published (.pbit & .pdf) ✅               Fact & Dimensions                  Views / Transformation
  Dashboard Built ✅                         & Measures Created ✅
```

---

## 🚦 Project Roadmap & Phase Status

### ① Phase 1: Data Preparation & Cleaning in Python (Pandas)
**STATUS:** `COMPLETED` ✅

* **JSON Flattening:** Parsed and normalized semi-structured `mcc_codes.json` and `train_fraud_labels.json` into tabular DataFrames.
* **Data Sanitization:** Standardized transaction amounts (`amount`), enforced strict `datetime` formatting, and handled null/duplicate entries.
* **Data Export:** Exported standardized CSV files using relative, cross-platform paths.

---

### ② Phase 2: Ingestion & Structuring in SQL Server (SSMS)
**STATUS:** `COMPLETED` ✅

* **Database DDL:** Provisioned core database `FraudAnalyticsDB` in Microsoft SQL Server Management Studio (SSMS 22).
* **Staging Area (`stg_`):** Bulk loaded 13.3M+ records into target staging tables (`stg_transactions`, `stg_fraud_labels`, `stg_cards`, `stg_users`, `stg_mcc`).
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
* **Centralized DAX Measures (`_Measures`):** Created automated metrics for Volume ($), Transaction Counts, Fraud Losses ($), and Fraud Rate (%).

---

### ⑤ Phase 5: Interactive Visualization & Executive Dashboard
**STATUS:** `COMPLETED` ✅

* **VertiPaq Model Optimization:** Disabled Auto Date/Time, marked official Date Table, changed currency fields to `Fixed Decimal Number`, set geographic categories, and removed high-cardinality columns.
* **Multi-Page Executive Dashboard:**
  * **Page 1 — Executive Overview:** High-level KPIs, total processed volume trends vs. fraud losses, and macro fraud rate indicators.
  * **Page 2 — Risk & Anomaly Analysis:** Hour-of-day vs. Day-of-week heatmaps, high-risk Merchant Category Codes (MCCs), and error type correlations.
  * **Page 3 — Customer Demographics & Behavior:** Customer segmentation by income tier, age groups, credit score tiers, and dark web card flags.
* **Deliverable Sharing:** Exported lightweight template (`.pbit`) for repository deployment alongside a static executive report (`.pdf`).

---

## 📂 Repository Structure

```text
financial-fraud-analytics/
│
├── notebooks/
│   └── 01_data_preparation_and_cleaning.ipynb   # Data cleaning, JSON flattening & ETL notebook
│
├── powerbi/
│   ├── Fraud_Analytics_Report.pbit              # Power BI Template File
│   └── Fraud_Analytics_Report.pdf               # Executive Dashboard Export (PDF)
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
