# Data Modeling

Data modeling is the process of **structuring your data** so that it is efficient to store, easy to query, and consistent across systems. It’s essential for building scalable, maintainable, and insight-driven data platforms.

---

## 🧱 What is a Data Model?

A data model defines how data is **organized, stored, and related** to other data. In analytics and warehousing, models are typically either:

* **Dimensional (Star/Snowflake)** — used in OLAP systems
* **Normalized** — used in OLTP and intermediate ETL stages

---

## ⭐ Star Schema (Dimensional Modeling)

### 📌 Structure

* Central **fact table** with keys to related **dimension tables**
* Denormalized for simplicity and performance

### ✅ Benefits

* Simple joins, fast aggregations
* Optimized for BI tools (Tableau, Looker, Power BI)

### 🧠 Example

```
FactSales
- order_id
- product_id
- customer_id
- revenue
- order_date

DimCustomer
- customer_id
- name
- segment

DimProduct
- product_id
- category
- brand
```

---

## ❄️ Snowflake Schema

### 📌 Structure

* Like star schema, but **dimension tables are normalized**

### ✅ Benefits

* Reduces storage redundancy
* Easier to maintain when dimension values are shared

### ⚠️ Tradeoffs

* More joins = potentially slower queries
* Higher complexity for analysts

---

## 🧩 Normalized Models (OLTP/ELT Stages)

### 🔍 What It Is

Highly normalized tables with strict referential integrity — typical in source systems or early-stage data lakes.

### ✅ Benefits

* Removes data duplication
* Easier for transactional consistency

### ⚠️ Tradeoffs

* Complex joins for analytics
* Requires downstream denormalization

---

## 🔄 Slowly Changing Dimensions (SCD)

### 📌 What It Is

A method to track changes in dimension tables over time.

| Type           | Description                       | Example                            |
| -------------- | --------------------------------- | ---------------------------------- |
| **SCD Type 1** | Overwrite old data                | Update `user_status` directly      |
| **SCD Type 2** | Add new row + versioning          | New row when `user_status` changes |
| **SCD Type 3** | Add new column for previous value | Keep `prev_user_status` column     |

---

## 🔐 Surrogate vs Natural Keys

| Type          | Description                  | Example                   |
| ------------- | ---------------------------- | ------------------------- |
| **Natural**   | Real-world unique value      | `email`, `SSN`            |
| **Surrogate** | Artificial unique identifier | `customer_id`, `user_key` |

Surrogate keys are preferred in warehouses to avoid join mismatches and handle slowly changing dimensions.

---

## 🧠 Best Practices

* Prefer **wide fact tables** with integer surrogate keys
* Use **date dimensions** for consistency in time analysis
* Avoid **many-to-many relationships** in BI models — use bridge tables if needed
* Include **null handling and default values** in your models
* Use **semantic layers** (e.g., LookML, dbt metrics) to abstract raw models

---

## 🚀 Example Use Case: Ecommerce Analytics Model

| Layer                 | Purpose                      | Example Tables                |
| --------------------- | ---------------------------- | ----------------------------- |
| **Staging**           | Raw ingested data            | `orders_raw`, `clicks_raw`    |
| **Base**              | Cleaned, typed, deduplicated | `orders_clean`, `users_clean` |
| **Dimensional**       | Star schema for analytics    | `fact_sales`, `dim_product`   |
| **Reporting/Metrics** | Pre-aggregated dashboards    | `daily_sales`, `top_skus`     |

---

## 🛠 Tools That Support Data Modeling

* **dbt** (data build tool)
* **LookML** (Looker modeling language)
* **Power BI semantic models**
* **Cube.js**, **AtScale**, **MetricFlow**

---

## 🧠 Summary

| Model Type       | Best For                       | Tools/Use Cases            |
| ---------------- | ------------------------------ | -------------------------- |
| Star Schema      | BI dashboards, fast queries    | Tableau, Power BI, Looker  |
| Snowflake Schema | Storage optimization, reuse    | Redshift, Snowflake        |
| Normalized       | Source ingestion, ELT layers   | Postgres, OLTP systems     |
| SCD Type 2       | History tracking in dimensions | Slowly evolving attributes |
