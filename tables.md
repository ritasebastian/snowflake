### **Different Types of Tables in Snowflake** 🏢❄️

Snowflake supports multiple table types based on **storage, persistence, and use cases**.

---

## **1️⃣ Permanent Tables (Default)**
✅ **Used for long-term data storage**.  
✅ **Fully persisted** (stored in Snowflake storage).  
✅ **Supports Time Travel & Fail-safe (default: 90 days retention)**.  

🔹 **Example: Creating a Permanent Table**
```sql
CREATE TABLE sales_data (
    order_id INT,
    customer_id INT,
    total_amount DECIMAL(10,2),
    order_date DATE
);
```

---

## **2️⃣ Temporary Tables**
✅ **Exists only during a session**.  
✅ **Automatically dropped when the session ends**.  
✅ **No Time Travel & Fail-safe**.  
✅ **Useful for intermediate calculations in ETL jobs**.  

🔹 **Example: Creating a Temporary Table**
```sql
CREATE TEMPORARY TABLE temp_sales_data AS 
SELECT * FROM sales_data WHERE order_date >= CURRENT_DATE - INTERVAL '30 DAYS';
```
💡 **Use Case:** Store intermediate results for an ETL job.

---

## **3️⃣ Transient Tables**
✅ **Persists beyond session but lacks Fail-safe (Only Time Travel support)**.  
✅ **Faster & cheaper because it skips Fail-safe storage**.  
✅ **Ideal for short-lived data like logs, staging tables**.  

🔹 **Example: Creating a Transient Table**
```sql
CREATE TRANSIENT TABLE staging_sales_data (
    order_id INT,
    customer_id INT,
    total_amount DECIMAL(10,2),
    order_date DATE
);
```
💡 **Use Case:** Staging or temporary analytics where Fail-safe is not needed.

---

## **4️⃣ External Tables**
✅ **References data stored in cloud storage (AWS S3, Azure Blob, GCS)**.  
✅ **Query external data without loading it into Snowflake**.  
✅ **Useful for Big Data & Data Lake Integration**.  

🔹 **Example: Creating an External Table on AWS S3**
```sql
CREATE EXTERNAL TABLE external_sales_data 
WITH LOCATION = '@my_s3_stage/sales/'
FILE_FORMAT = (TYPE = PARQUET);
```
💡 **Use Case:** Query large Parquet/CSV files stored in S3 **without ingestion**.

---

## **5️⃣ Clone Tables (Zero-Copy Cloning)**
✅ **Creates an instant copy of a table without duplicating storage**.  
✅ **Uses metadata pointers to avoid data duplication**.  
✅ **Supports cloning schemas & databases too**.  

🔹 **Example: Cloning a Table**
```sql
CREATE TABLE cloned_sales_data CLONE sales_data;
```
💡 **Use Case:** Data experimentation & backups.

---

## **6️⃣ Materialized Views (Table-Like but Precomputed)**
✅ **Stores precomputed query results** for fast access.  
✅ **Auto-refresh available but costs compute resources**.  
✅ **Improves performance for expensive aggregations**.  

🔹 **Example: Creating a Materialized View**
```sql
CREATE MATERIALIZED VIEW mv_daily_sales AS
SELECT order_date, SUM(total_amount) AS total_revenue
FROM sales_data
GROUP BY order_date;
```
💡 **Use Case:** Optimizing dashboards with precomputed results.

---

## **📌 Summary: When to Use Which Table?**
| Table Type | Persistence | Use Case |
|------------|------------|----------|
| **Permanent** | Long-term | Default table type, supports Time Travel & Fail-safe |
| **Temporary** | Session-only | ETL transformations, intermediate calculations |
| **Transient** | Until manually dropped | Staging, logs, data pipelines (cheaper than Permanent) |
| **External** | Never (Data stored externally) | Querying data in S3, Azure, or GCS |
| **Clone** | Metadata-based | Backup, Testing, Experimentation |
| **Materialized View** | Precomputed | Speed up analytics with cached query results |

---

