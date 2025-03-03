# **SQL Window Functions in Snowflake**
Snowflake supports **Window Functions (also called Analytical Functions)**, which allow you to **perform calculations across a defined set of rows** without grouping them. These functions help with **ranking, running totals, moving averages, and more**.

---

## **🔹 1. Basic Syntax of a Window Function**
```sql
<function_name>(column) OVER (
    PARTITION BY column_1 -- (Optional: Divides data into groups)
    ORDER BY column_2 -- (Optional: Defines order within partitions)
    ROWS BETWEEN X PRECEDING AND Y FOLLOWING -- (Optional: Defines window frame)
)
```
---

# **📌 Common Window Functions in Snowflake**
| **Function** | **Description** |
|-------------|---------------|
| **RANK()** | Assigns a **unique rank** to each row within a partition. |
| **DENSE_RANK()** | Similar to `RANK()`, but **no gaps in ranking**. |
| **ROW_NUMBER()** | Assigns a unique **sequential** row number within a partition. |
| **LAG()** | Gets **previous row value**. |
| **LEAD()** | Gets **next row value**. |
| **SUM() OVER()** | Running total of a column. |
| **AVG() OVER()** | Moving average of a column. |
| **NTILE(N)** | Splits data into **N buckets** (quartiles, percentiles). |

---

## **🔹 2. Example Data**
Let’s create a sample table:

```sql
CREATE OR REPLACE TABLE sales (
    sales_id INT,
    region STRING,
    sales_amount INT,
    sales_date DATE
);

INSERT INTO sales VALUES 
(1, 'North', 500, '2024-02-20'),
(2, 'North', 700, '2024-02-21'),
(3, 'South', 400, '2024-02-20'),
(4, 'South', 300, '2024-02-21'),
(5, 'East', 900, '2024-02-20'),
(6, 'East', 600, '2024-02-21');
```

---

## **🔹 3. Ranking Functions**
### **1️⃣ RANK() – Assigns a Unique Rank (Skips Ranks for Ties)**
```sql
SELECT sales_id, region, sales_amount,
       RANK() OVER (PARTITION BY region ORDER BY sales_amount DESC) AS rank
FROM sales;
```
**🔹 Output:**
```
+----------+--------+--------------+------+
| sales_id | region | sales_amount | rank |
+----------+--------+--------------+------+
|    2     | North  |     700      |  1   |
|    1     | North  |     500      |  2   |
|    5     | East   |     900      |  1   |
|    6     | East   |     600      |  2   |
|    3     | South  |     400      |  1   |
|    4     | South  |     300      |  2   |
+----------+--------+--------------+------+
```

---
### **2️⃣ DENSE_RANK() – No Rank Gaps for Ties**
```sql
SELECT sales_id, region, sales_amount,
       DENSE_RANK() OVER (PARTITION BY region ORDER BY sales_amount DESC) AS dense_rank
FROM sales;
```
🚀 **Difference:** If two sales have the same `sales_amount`, they **get the same rank**, and the next rank is not skipped.

---
### **3️⃣ ROW_NUMBER() – Assigns a Unique Row Number (No Gaps)**
```sql
SELECT sales_id, region, sales_amount,
       ROW_NUMBER() OVER (PARTITION BY region ORDER BY sales_amount DESC) AS row_num
FROM sales;
```
🚀 **Best for pagination:** It provides a unique row number for each row.

---

## **🔹 4. Running Totals & Moving Averages**
### **4️⃣ SUM() OVER() – Running Total**
```sql
SELECT sales_id, region, sales_amount,
       SUM(sales_amount) OVER (PARTITION BY region ORDER BY sales_date) AS running_total
FROM sales;
```
**🔹 Output:**
```
+----------+--------+--------------+---------------+
| sales_id | region | sales_amount | running_total|
+----------+--------+--------------+---------------+
|    1     | North  |     500      |      500     |
|    2     | North  |     700      |     1200     |
|    5     | East   |     900      |      900     |
|    6     | East   |     600      |     1500     |
+----------+--------+--------------+---------------+
```
🚀 **Use Case:** Cumulative sales over time.

---
### **5️⃣ AVG() OVER() – Moving Average**
```sql
SELECT sales_id, region, sales_amount,
       AVG(sales_amount) OVER (PARTITION BY region ORDER BY sales_date ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS moving_avg
FROM sales;
```
🚀 **Use Case:** Finding the **average of the last N rows**.

---

## **🔹 5. LAG() & LEAD() – Access Previous/Next Rows**
### **6️⃣ LAG() – Get Previous Row Value**
```sql
SELECT sales_id, region, sales_amount,
       LAG(sales_amount, 1) OVER (PARTITION BY region ORDER BY sales_date) AS prev_day_sales
FROM sales;
```
🚀 **Use Case:** Compare **current row with the previous row**.

---
### **7️⃣ LEAD() – Get Next Row Value**
```sql
SELECT sales_id, region, sales_amount,
       LEAD(sales_amount, 1) OVER (PARTITION BY region ORDER BY sales_date) AS next_day_sales
FROM sales;
```
🚀 **Use Case:** Predict **future sales trends**.

---

## **🔹 6. NTILE() – Split Data into Equal Buckets**
### **8️⃣ NTILE(2) – Divide Data into Two Groups**
```sql
SELECT sales_id, region, sales_amount,
       NTILE(2) OVER (PARTITION BY region ORDER BY sales_amount DESC) AS sales_bucket
FROM sales;
```
🚀 **Use Case:** Assign customers into **percentile groups (Top 25%, Top 50%)**.

---

## **🚀 Summary of Snowflake Window Functions**
| **Function** | **Use Case** |
|-------------|-------------|
| **RANK()** | Unique rank, gaps in ranking |
| **DENSE_RANK()** | No gaps in ranking |
| **ROW_NUMBER()** | Assigns a unique row number (Best for Pagination) |
| **SUM() OVER()** | Running total (Cumulative sum) |
| **AVG() OVER()** | Moving average |
| **LAG()** | Previous row value |
| **LEAD()** | Next row value |
| **NTILE(N)** | Distributes rows into N equal parts |

---
