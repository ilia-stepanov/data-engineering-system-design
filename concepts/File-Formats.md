# File Formats for Data Engineering: Parquet, Avro, ORC, JSON, CSV

Choosing the right file format is crucial for building efficient and cost-effective data pipelines. File format impacts **storage size**, **query speed**, **compression**, and **compatibility** with tools.

---

## 🧾 Common File Formats

| Format      | Type               | Compression | Splittable | Schema Support | Best For                            |
| ----------- | ------------------ | ----------- | ---------- | -------------- | ----------------------------------- |
| **CSV**     | Row-based          | ❌ Low       | ✅ Yes      | ❌ No           | Simplicity, interoperability        |
| **JSON**    | Semi-structured    | ❌ Low       | ❌ No       | ❌ No           | Logs, APIs, nested structures       |
| **Avro**    | Row-based (binary) | ✅ Yes       | ✅ Yes      | ✅ Strong       | Kafka messages, row-level streaming |
| **Parquet** | Columnar           | ✅ Excellent | ✅ Yes      | ✅ Strong       | OLAP, analytical queries            |
| **ORC**     | Columnar           | ✅ Excellent | ✅ Yes      | ✅ Strong       | Hive, Spark SQL (on Hadoop)         |

---

## 📦 Row-Based vs Columnar Formats

### 🧱 Row-Based (CSV, JSON, Avro)

* Stores all fields of a row together
* Efficient for **write-heavy** and **row-level access**
* Common in **streaming systems** and **event logs**

### 📊 Columnar (Parquet, ORC)

* Stores values by column
* Great for **read-heavy workloads** (e.g., filtering, aggregations)
* Essential in **analytical engines** (e.g., BigQuery, ClickHouse)

---

## 🔍 Format Deep Dive

### ✅ **Parquet**

* **Columnar**, highly compressed
* Ideal for OLAP and large-scale querying
* Works well with Spark, Hive, Presto, Trino, BigQuery

### ✅ **Avro**

* Compact **binary row-based format**
* Includes schema and supports evolution (backward/forward compatible)
* Great for Kafka pipelines and streaming ingestion

### ✅ **ORC**

* Optimized **columnar format** built for Hadoop ecosystem
* Better compression and predicate pushdown than Parquet in Hive
* Used in Spark + Hive jobs on HDFS

### ❌ **CSV**

* Human-readable and portable
* No schema, no compression, inefficient at scale
* Good for small jobs or exporting data

### ❌ **JSON**

* Self-describing and flexible
* Poor compression and performance for large-scale processing
* Useful for ingesting semi-structured or API data

---

## 🧱 Table Formats: Delta Lake, Iceberg, Hudi

### ✅ **Delta Lake**

* Adds ACID transactions, time travel, schema enforcement to Parquet
* Supports batch + streaming workloads
* Works best with Databricks but also supported in Spark + other engines

### ✅ **Apache Iceberg**

* Table format for huge datasets with ACID support, schema evolution
* Enables time-travel, hidden partitioning, and snapshotting
* Supported by Trino, Flink, Spark, Dremio, Snowflake (partial)

### ✅ **Apache Hudi**

* Designed for fast upserts/deletes and incremental pulls
* Ideal for near-real-time ingestion
* Common in stream-based and CDC systems

These formats are not raw file types, but **transactional layers** over file formats like Parquet or ORC.

---

## 🔧 Choosing the Right Format

| Use Case                                | Recommended Format     |
| --------------------------------------- | ---------------------- |
| Streaming data ingestion (Kafka, Flink) | Avro                   |
| Batch analytics (Hive, Spark, Presto)   | Parquet / ORC          |
| Interchange between APIs                | JSON / CSV             |
| Machine learning feature stores         | Parquet                |
| Storing logs or semi-structured data    | JSON                   |
| ACID tables on data lake                | Delta / Iceberg / Hudi |

---

## ⚠️ Tradeoffs

* **Parquet/ORC**: High performance but needs upfront schema and slower writes.
* **Avro**: Best for write-heavy and schema evolution, but not good for analytics.
* **Delta/Iceberg/Hudi**: Add transactional guarantees, but add complexity and overhead.
* **JSON/CSV**: Easy to use but inefficient for scale.

---

## 🧠 Pro Tip

For most modern data lakes or warehouses, prefer **Parquet** unless:

* You're using Kafka (→ Avro)
* You need to ingest nested/semi-structured API data (→ JSON)
* You want versioned or transactional data (→ Delta/Iceberg/Hudi)
