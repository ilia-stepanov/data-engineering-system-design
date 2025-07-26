# Case Study: Quick Data Modeling Task (10 min)

### ⏱️ Time Limit: 10 minutes

### 🎯 Objective:

Design a data model to support a **city-level ride-sharing analytics dashboard**. The business wants to track KPIs such as:

* How many rides happen in each city per day?
* What’s the average ride duration by city?
* How much revenue does each driver generate?
* What’s the ride cancellation rate across cities and time periods?

You’ve been given raw data from upstream systems. Your job is to build a model that makes these questions easy to answer.

---

## 📥 Input Data Sources

You receive the following raw tables:

### `rides`

| Column      | Type      | Description                        |
| ----------- | --------- | ---------------------------------- |
| ride\_id    | STRING    | Unique ride identifier             |
| driver\_id  | STRING    | Driver who accepted the ride       |
| user\_id    | STRING    | User who requested the ride        |
| start\_time | TIMESTAMP | When the ride started              |
| end\_time   | TIMESTAMP | When the ride ended                |
| status      | STRING    | `completed`, `canceled`, `no_show` |
| price\_usd  | FLOAT     | Total price paid by user           |
| city        | STRING    | City where the ride took place     |

### `drivers`

| Column     | Type   | Description            |
| ---------- | ------ | ---------------------- |
| driver\_id | STRING | Unique driver ID       |
| name       | STRING | Driver name            |
| join\_date | DATE   | When the driver joined |
| rating     | FLOAT  | Avg. rating (1-5)      |
| city       | STRING | Home city              |

---

## 🧩 Your Task

You’ve been asked to design a model that:

* Supports consistent daily and weekly reporting for operations teams
* Allows slicing metrics by city, driver, and ride status
* Can be queried efficiently for dashboards and ad hoc exploration

---

## 🚨 Constraints

* Keep the design lean and fast to implement (10 min exercise)
* Don't overengineer — choose only the entities and relationships that support the business need
* You can assume you own the ETL and modeling layers


## Answer
| fact_rides |
| ----------- |
| ride_id (PK) |
| driver_id (FK) |
| city_id (FK) |
| date_id (FK) |
| price_usd |
| ride_duration_min |
| is_canceled |


| dim_driver |
| ----------- |
| driver_id (PK) |
| name |
| join_date |
| rating |
| home_city_id (FK) |

| dim_date |
| ----------- |
| date_id (PK) |
| full timestamp |
| day | 
| week |
| month |

| dim_city |
| ----------- |
| city_id (PK) |
| city_name |
| region |
