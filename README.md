# Azure Data Engineer Take-Home — Blue Owls Solutions

## 📌 Overview

This project implements an end-to-end data pipeline using a medallion architecture (Bronze → Silver → Gold) to ingest, process, and analyze e-commerce data from the Blue Owls API.

The pipeline is designed to handle real-world challenges such as API failures, token expiration, and inconsistent data quality.

---

## 🏗️ Architecture

API → Bronze → Silver → Gold → SQL Analytics

---

## ⚙️ Tech Stack

* Python (requests, pandas)
* PySpark
* Spark SQL
* Jupyter Notebooks
* Docker

---

## 🚀 Data Ingestion & Resilience

### Authentication

* Implemented Bearer token authentication
* Automatic token refresh on 401 errors

### Retry Strategy

* Handles API failures (500, 429)
* Exponential backoff implemented

### Pagination

* Supports full paginated extraction

### Fault Handling

* Page-level retry ensures pipeline does not fail

---

## 📂 Bronze Layer

* Raw API data stored as CSV
* Metadata columns added:

  * `_ingested_at`
  * `_source_endpoint`

### Idempotency Note

Current implementation overwrites Bronze for simplicity.
In production, a manifest-based approach would prevent duplicate ingestion.

---

## 🧹 Silver Layer

* Data cleaned and type-cast
* Deduplicated using natural keys
* `_is_valid` flag added for data quality

---

## ⭐ Gold Layer (Star Schema)

### Fact Table: `fact_order_items`

* Grain: one row per order item
* Includes pricing, freight, payment, and delivery metrics

### Dimensions:

* `dim_customers`
* `dim_products`
* `dim_sellers`

### Surrogate Keys

* Deterministic hashing using `sha2`

---

## 📊 SQL Analysis

### Query 1

Revenue trend with ranking, growth %, and rolling average.

### Query 2

Seller performance scorecard using composite ranking.

---

## ⚠️ Assumptions & Trade-offs

* Payment distribution simplified
* Bronze layer not fully idempotent (documented)
* Some delivery metrics depend on available fields

---

## 🛠️ Production Improvements

* Azure Data Factory / Fabric for orchestration
* Delta Lake for storage
* Monitoring & alerting
* CI/CD pipelines
* Data quality frameworks

---

## 📁 Project Structure

notebooks/

* 1_api_ingestion.ipynb
* 2_bronze_to_silver.ipynb
* 3_silver_to_gold.ipynb
* 4_sql_analysis.ipynb

submission/output/

* bronze/
* silver/
* gold/

---

## ✅ Conclusion

This pipeline demonstrates:

* Resilient API ingestion
* Scalable transformation
* Star schema modeling
* Analytical querying

Designed with real-world data engineering principles.
