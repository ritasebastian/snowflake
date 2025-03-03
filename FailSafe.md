### **Is Fail-safe Built-in for Permanent Tables in Snowflake?** ❄️✅

Yes! **Fail-safe is automatically built-in for Permanent Tables in Snowflake**, and it provides an **additional 7-day retention period** for disaster recovery. However, there are some key points to understand.

---

## **1️⃣ Key Facts About Fail-safe**
✅ **Enabled by default for Permanent Tables** (Cannot be disabled).  
✅ **Fixed 7-day retention period** (Cannot be changed).  
✅ **Only for Permanent Tables** (**Not available for Transient or Temporary Tables**).  
✅ **Data is stored internally and cannot be accessed via SQL queries**.  
✅ **Requires Snowflake Support to restore** (Users cannot access Fail-safe data directly).  
✅ **Fail-safe is NOT an alternative to Time Travel** (It's a last-resort recovery option).

---

## **2️⃣ Which Tables Have Fail-safe?**
| **Table Type** | **Fail-safe Supported?** | **Retention Modifiable?** |
|--------------|----------------|----------------|
| **Permanent Table** | ✅ Yes (7 Days) | ❌ No |
| **Transient Table** | ❌ No | ❌ No |
| **Temporary Table** | ❌ No | ❌ No |
| **External Table** | ❌ No | ❌ No |

---

## **3️⃣ How to Recover Data from Fail-safe?**
Since **Fail-safe is NOT user-accessible**, follow these steps to request recovery:

### **🔹 Step 1: Verify Time Travel Expiration**
Check if the table can still be recovered using **Time Travel** (before resorting to Fail-safe).
```sql
SELECT * FROM your_table AT (TIMESTAMP => '2025-02-28 12:00:00');
```
If Time Travel is expired, you must contact Snowflake Support.

---

### **🔹 Step 2: Contact Snowflake Support for Fail-safe Recovery**
Go to **Snowflake Support**:  
🔗 [https://support.snowflake.com](https://support.snowflake.com)

Provide these details:
- **Snowflake Account & Region** (e.g., `my_account.us-east-1`)
- **Database & Schema Name** where the table was located.
- **Table Name** that needs to be restored.
- **Approximate Deletion Time**.
- **Reason for Recovery Request**.

🔹 **Snowflake engineers will manually recover the data**.

---

## **4️⃣ How to Prevent Data Loss?**
To avoid relying on Fail-safe, follow these best practices:

### ✅ **Increase Time Travel Retention**
For critical tables, extend Time Travel to **90 days**:
```sql
ALTER TABLE your_table SET DATA_RETENTION_TIME_IN_DAYS = 90;
```

### ✅ **Use Zero-Copy Cloning for Backups**
```sql
CREATE TABLE your_table_backup CLONE your_table;
```

### ✅ **Automate Daily Backups to Cloud Storage**
Use Snowflake **COPY INTO** to export data:
```sql
COPY INTO @my_s3_stage/backup/ FROM your_table FILE_FORMAT = (TYPE = CSV);
```

---

## **5️⃣ Summary: Time Travel vs. Fail-safe**
| Feature | **Time Travel** | **Fail-safe** |
|---------|---------------|-------------|
| **Purpose** | Restore recent changes | Last-resort recovery |
| **Retention Period** | Up to 90 days | Fixed 7 days |
| **Who Can Restore?** | User (via SQL) | Only Snowflake Support |
| **Works On?** | Permanent & Transient Tables | Only Permanent Tables |
| **Data Access** | Immediate via SQL | No user access, manual restore required |

---

