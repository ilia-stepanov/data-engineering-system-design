# Change Data Capture (CDC) & Incremental Loads

Efficient data pipelines don’t reprocess everything — they only process **what changed**. That’s where **Change Data Capture (CDC)** and **incremental loading** come in.

---

## 🔁 What is Incremental Loading?

### 🔍 Definition

Incremental loading is the strategy of **loading only new or updated data** into your target system, rather than the full dataset.

### ✅ Benefits

* Faster pipelines
* Lower compute/storage cost
* Reduced load on source systems

### 📌 Example

Instead of reloading all 1M rows, load only the 10K rows where `updated_at > last_run_time`

### 🛠 Techniques

* Timestamps (`updated_at`, `created_at`)
* Sequence numbers (monotonic IDs)
* Appending logs or delta files (e.g., via S3 or Kafka)

---

## 🔄 What is Change Data Capture (CDC)?

### 🔍 Definition

CDC is a mechanism to **detect and propagate data changes** (INSERTs, UPDATEs, DELETEs) from a source system to downstream systems.

### ✅ Use Cases

* Real-time replication to a warehouse
* Event-driven processing (e.g., fraud alerts)
* Upserts/deletes in lakehouse tables

### 🛠 Implementation Types

| Method              | Description                                 | Examples                                      |
| ------------------- | ------------------------------------------- | --------------------------------------------- |
| **Log-based**       | Taps into DB logs (e.g., WAL, binlog)       | Debezium, StreamSets, Fivetran, Kafka Connect |
| **Trigger-based**   | Uses DB triggers to write to a change table | Custom logic (not scalable)                   |
| **Timestamp-based** | Queries rows where updated\_at > last sync  | Simpler, but less accurate                    |

---

## 🔃 Full Load vs Incremental Load

| Characteristic   | Full Load                  | Incremental Load                |
| ---------------- | -------------------------- | ------------------------------- |
| Data Volume      | High                       | Minimal (only changed/new rows) |
| Processing Time  | Long                       | Fast                            |
| Cost             | High compute + IO          | Low                             |
| Failure Recovery | Simpler                    | Requires state tracking         |
| Use Case         | Initial load, rare reboots | Daily syncs, near-real-time ETL |

---

## ⚠️ Challenges

* **Schema changes**: downstream must adapt
* **Deletes**: harder to detect if no soft delete or logs
* **Out-of-order events**: require windowing & deduplication
* **State tracking**: must remember last processed position

---

## 🧠 Pro Tips

* Always add `created_at`, `updated_at`, and `is_deleted` fields if you control the schema
* Use **idempotent** upserts (`MERGE` or `INSERT ON CONFLICT`) for safety
* For real-time, prefer **log-based CDC** (e.g., Debezium to Kafka)
