# Apache Spark ETL Pipeline: Brazilian Education Census

An end-to-end local Data Engineering pipeline that extracts massive public datasets, transforms the data into a relational Star Schema using Apache Spark, and loads it into a PostgreSQL data warehouse for analytics.

## 📌 Project Overview
This project processes the microdata from the Brazilian National Basic Education Census (INEP). The raw data consists of millions of rows in a flat CSV format. This pipeline automates the downloading, data cleaning, and architectural restructuring required to make this data usable for Business Intelligence (BI) tools.

### Tech Stack
* **Processing Engine:** Apache Spark (PySpark)
* **Data Warehouse:** PostgreSQL
* **Language:** Python 3.11
* **Environment:** Jupyter Notebook / Anaconda

## 🏗️ Architecture & Pipeline Steps

### 1. Extract (E)
* Utilizes the `requests` library to programmatically fetch the latest `.zip` microdata directly from the Brazilian Open Data Portal.
* Bypasses strict SSL configurations and automatically unzips the payload into a local `raw_data` directory.

### 2. Transform (T)
* **Ingestion:** Loads the multi-gigabyte CSV into a Spark DataFrame, explicitly handling Brazilian regional encoding (`iso-8859-1`) and custom delimiters.
* **Data Modeling:** Slices the denormalized flat file into a highly efficient **Star Schema**:
  * `dim_location`: Isolates region, state, and municipality data, removing duplicates and generating unique surrogate keys (`location_id`).
  * `dim_school`: Extracts descriptive school attributes (name, administrative dependency) and assigns unique `school_id` keys.
  * `fact_school`: The central fact table containing quantifiable metrics (e.g., total basic enrollments, internet access flags) mapped back to the dimension keys.

### 3. Load (L)
* Establishes a JDBC connection via Spark to a local PostgreSQL instance.
* Overwrites and pushes the fully transformed Star Schema (`dim_location`, `dim_school`, `fact_school`) into the public schema, ready for downstream SQL querying and dashboarding.

## 🚀 How to Run

1. **Prerequisites:** Ensure you have Apache Spark, Java (JVM), and PostgreSQL installed on your local machine.
2. **Environment:** Clone this repository and install the required dependencies:
   ```bash
   pip install pyspark requests jupyter
