# Indexing & Query Optimization

Indexing is a fundamental technique for **speeding up query performance**, especially in OLTP systems and analytical databases. It allows the database engine to **avoid full table scans** and instead jump directly to relevant records.

---

## 🔍 What is an Index?

An index is a **data structure** that maps column values to their physical locations in storage, enabling faster lookups.

Think of it like an index in a book: instead of scanning every page, you jump directly to the content.

---

## 🧱 Common Index Types

| Index Type     | Description                   | Best For                           |
| -------------- | ----------------------------- | ---------------------------------- |
| **B-Tree**     | Balanced tree index           | Range queries, sorted lookups      |
| **Hash**       | Key-value hash table          | Equality lookups (`WHERE id = 42`) |
| **Bitmap**     | Bit vectors for column values | Low-cardinality fields             |
| **Inverted**   | Token-based indexing for text | Full-text search (Elasticsearch)   |
| **Skip Index** | Min/max per data block        | ClickHouse, columnar formats       |
| **Zone Maps**  | Min/max summary per block     | Parquet, ORC, BigQuery             |

---

## ⚡ Indexing in Different Systems

### ✅ **OLTP Databases (Postgres, MySQL)**

* Use **B-Tree indexes** by default
* Composite indexes can speed up multi-column filters
* Use `EXPLAIN` to verify query plan and index usage

### ✅ **OLAP Warehouses (ClickHouse, BigQuery, Redshift)**

* Rely on **column pruning**, **zone maps**, and **data skipping**
* Indexing is more implicit or done via clustering keys or projections

### ✅ **Search Engines (Elasticsearch, Solr)**

* Use **inverted indexes** to search tokens across documents

---

## 🧠 Query Optimization Techniques

### 🔍 General Strategies

* Select only needed columns (`SELECT id, name` vs `SELECT *`)
* Use indexed columns in `WHERE`, `JOIN`, `GROUP BY`
* Filter early (pushdown filters to scan layer)
* Avoid functions on indexed columns (`WHERE LOWER(name) = ...`)

### 🧰 System-Specific Tips

* **PostgreSQL**: Create `btree` or `GIN` indexes, analyze queries with `EXPLAIN ANALYZE`
* **ClickHouse**: Use **primary key**, **skip indexes**, and **TTL settings**
* **BigQuery**: Partition + Cluster smartly, use approximate aggregations if possible

---

## ⚠️ Tradeoffs

| Benefit           | Cost                                       |
| ----------------- | ------------------------------------------ |
| Fast queries      | Slower inserts/updates (index maintenance) |
| Reduced scan size | Extra storage footprint                    |
| Optimized plans   | Need for regular tuning as data grows      |

---

## 🧠 Pro Tips

* Avoid over-indexing; every index has a write cost
* Monitor with query plans and slow query logs
* Periodically review and drop unused indexes

---

## 🚀 Real-World Scenarios

* ClickHouse: indexing on `user_id` for fast retrieval in dashboards
* Postgres: composite index on `(user_id, event_time)` for session queries
* BigQuery: clustering by `event_name` improves filter performance dramatically
