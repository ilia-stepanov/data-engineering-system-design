# Scalability, Fault Tolerance, and Availability

These three concepts are foundational pillars in designing reliable and performant data systems. While related, each addresses a different dimension of **system robustness** and **performance under stress**.

---

## 📈 Scalability

### 🔍 Definition

Scalability is the ability of a system to **handle increasing load by adding resources** (hardware or software).

### 🧱 Types

| Type                   | Description                                | Example                                 |
| ---------------------- | ------------------------------------------ | --------------------------------------- |
| **Vertical Scaling**   | Add more power (CPU, RAM) to a single node | Upgrade VM from 8 GB to 64 GB RAM       |
| **Horizontal Scaling** | Add more nodes to handle load in parallel  | Add more Kafka brokers or Spark workers |

### ✅ Best Practices

* Stateless services + load balancer
* Distributed file systems (HDFS, S3)
* Sharding, partitioning, autoscaling

---

## 🛡️ Fault Tolerance

### 🔍 Definition

Fault tolerance is the ability of a system to **continue operating properly even if some components fail**.

### 🛠 Techniques

* Replication (data & services)
* Checkpointing and retries (e.g., Spark, Flink)
* Circuit breakers and fallback logic
* Leader election (e.g., Zookeeper, Raft)

### 🧠 Key Principle

Design for **graceful degradation**, not total failure.

---

## 🔁 Availability

### 🔍 Definition

Availability is the **percentage of time a system is operational and accessible**.

### 📊 Common Metrics

| SLA Level | Downtime per year |
| --------- | ----------------- |
| 99.0%     | \~3.65 days       |
| 99.9%     | \~8.7 hours       |
| 99.99%    | \~52 minutes      |
| 99.999%   | \~5 minutes       |

### 🔁 Techniques

* Active-active replication (multi-region)
* Load balancers + health checks
* Failover clusters
* Redundant hardware

---

## 🔄 How They Interact

* **Scalability** ensures your system handles growth.
* **Fault tolerance** ensures it survives internal issues.
* **Availability** ensures users can access it most of the time.

---

## 🧠 Tradeoffs

* More fault tolerance and availability usually means **higher cost and complexity**
* **CAP Theorem**: can only guarantee two out of Consistency, Availability, Partition tolerance

---

## 🚀 Real-World Examples

* Kafka: partitioned, replicated, leader-follower model
* ClickHouse: replicated + sharded cluster for HA
* Spark: resilient via DAG recovery, retries
* BigQuery: scales horizontally, hides fault-tolerance under the hood

