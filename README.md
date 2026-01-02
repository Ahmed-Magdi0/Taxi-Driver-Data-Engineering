# NYC Taxi Data: End-to-End Data Engineering Pipeline

A fully automated **ELT (Extract, Load, Transform)** pipeline designed to ingest raw NYC Green Taxi trip data and transform it into an analytics-ready **Star Schema**. This project leverages the modern data stack to handle data inconsistencies and automate the entire lifecycle from local ingestion to BI visualization.

---

## Table of Contents
1. [Project Description](#project-description)
2. [Technologies Used](#technologies-used)
3. [Pipeline Workflow](#pipeline-workflow)
4. [Data Schema](#data-schema)
5. [Setup & Installation](#setup--installation)
6. [Monitoring & Logging](#monitoring--logging)

---

## Project Description

This project implements a complete automated ELT data pipeline for NYC LPEP (Green Taxi) trip records. Raw CSV files are ingested, stored, and modeled into a performant star schema inside **Snowflake** using **dbt**.

**Key Challenges Addressed:**
* **Data Quality:** Handled messy, inconsistent raw taxi data through automated cleaning.
* **Automation:** Eliminated manual ingestion and transformation steps.
* **Optimization:** Transformed flat files into a Star Schema for high-performance BI querying.

---

## Technologies Used

| Layer | Technology |
| :--- | :--- |
| **Orchestration** | Apache Airflow |
| **Data Warehouse** | Snowflake |
| **Transformation** | dbt (Data Build Tool) |
| **Language** | Python |
| **Data Source** | NYC TLC Green Taxi (CSV) |

---

## Pipeline Workflow

This project utilizes a modern ELT batch architecture, orchestrated entirely by **Apache Airflow**.

1. **Extract:** Raw `.csv` files are landed in a local directory designated for ingestion.
2. **Load:** Airflow automates the transfer of files into a Snowflake **Internal Stage** and utilizes the `COPY INTO` command to load data into raw staging tables.
3. **Transform:** Airflow triggers **dbt** to:
    * Apply business logic and data cleaning.
    * Materialize Fact and Dimension tables.
    * Execute data quality tests to ensure schema integrity.
4. **Consumption:** The final schema is available in Snowflake for querying and analysis.

---

## Data Schema

The final analytical model is a **Star Schema** designed for optimal performance in BI tools.

### Fact Table
* `Fact_Taxi_Trips`: Contains quantitative measures such as fare amounts, tips, tolls, and trip distances.

### Dimension Tables
* `Dim_Vendor`
* `Dim_Date`
* `Dim_Location`
* `Dim_Payment_Type`
* `Dim_Rate_Code`
* `Dim_Trip_Distance_Band`
* `Dim_Passenger_Count`
* `Dim_Trip_Type`

---

## Setup & Installation

### 1. Prerequisites
* Snowflake Account
* Apache Airflow (Docker recommended)
* Python 3.8+
* dbt Core (`dbt-snowflake`)

### 2. Snowflake Setup
Initialize your environment by running:
```sql
CREATE WAREHOUSE TAXI_WH;
CREATE DATABASE TAXI_DB;
CREATE SCHEMA TAXI_PROD;

-- Create stage for ingestion
CREATE STAGE taxi_stage;
```

### 3. Airflow Configuration
* Create a Snowflake connection in the Airflow UI (`conn_id: 'snowflake'`).
* Update the following variables in your DAG or environment configuration:
  * `LOCAL_DATA_PATH`: Directory containing your raw CSV files.
  * `DBT_PROJECT_DIR`: Path to your dbt project.
  * `DBT_VENV`: Path to your Python virtual environment.

---

## Monitoring & Logging
* **Airflow UI:** Used for real-time monitoring of DAG runs, task success/failure, and execution logs.
* **dbt Logs:** Transformation logs and test results are stored in the `LOG_DIR`, including:
  * `dimensions.log`
  * `taxi_fact.log`
  * `dbt_test.log`

---

*Maintained by: Ahmed Magdi*
