### **Time Travel vs Fail-safe in Snowflake** ❄️🔄

Snowflake provides **Time Travel** and **Fail-safe** as data recovery mechanisms, but they serve different purposes.  

| Feature | **Time Travel** | **Fail-safe** |
|---------|---------------|-------------|
| **Purpose** | Restore recent changes (deleted/updated data) | Recover lost data after Time Travel expires |
| **Retention Period** | **Up to 90 days** (depends on edition) | **Fixed 7 days** |
| **Modifiable?** | ✅ Yes (`DATA_RETENTION_TIME_IN_DAYS`) | ❌ No (Fixed at 7 days) |
| **Availability** | All Snowflake Editions (1-day default, more for Enterprise+) | Always enabled for Permanent tables |
| **Data Access** | Immediate (via SQL queries) | Snowflake Support must restore |
| **Storage Cost** | ✅ Yes (Extra storage costs based on retention) | ❌ No direct cost (handled internally) |
| **How It Works?** | Keeps historical versions of data (soft delete) | Stores a copy for disaster recovery (hard delete) |
| **How to Recover?** | `SELECT ... AT ...` | `UNDELETE` (through Snowflake Support) |
| **Works On?** | Permanent & Transient Tables | Only Permanent Tables |

---

## **1️⃣ What is Time Travel?**
✅ **Recovers deleted or modified data** for a configurable period (1 to 90 days).  
✅ **Works for Permanent & Transient tables** (not Temporary).  
✅ Can **query past versions** using `AT` or `BEFORE` keywords.

### **🔹 Example: Restoring a Deleted Table**
```sql
UNDROP TABLE sales_data;
```
✅ **Recovers `sales_data` if it's within the retention period**.

### **🔹 Example: Querying Historical Data**
```sql
SELECT * FROM sales_data AT (TIMESTAMP => '2025-02-25 10:00:00');
```
✅ **Retrieves data as it was at a specific time**.

---

## **2️⃣ What is Fail-safe?**
✅ **Backup mechanism for disaster recovery**.  
✅ **Fixed 7-day retention** (cannot be modified).  
✅ **Only available for Permanent Tables**.  
✅ **Accessible ONLY by Snowflake Support** (not through SQL).  

### **When to Use Fail-safe?**
🔹 **Time Travel expired** and you need to restore data.  
🔹 **Accidental deletion or corruption of important tables**.  
🔹 **Snowflake system failure** (rare but possible).  

---

## **3️⃣ Key Differences**
| Aspect | **Time Travel** | **Fail-safe** |
|--------|---------------|-------------|
| **Retention Period** | Up to 90 days (Enterprise+) | Fixed 7 days |
| **Who Can Restore?** | Users via SQL | Only Snowflake Support |
| **Recovery Speed** | Instant | Takes time (manual process) |
| **Costs** | Storage cost applies | No extra charge |
| **Works On?** | Permanent & Transient Tables | Only Permanent Tables |

---

### **🚀 Summary**
1️⃣ **Use Time Travel for quick recovery of recent changes**.  
2️⃣ **Fail-safe is a last-resort recovery handled by Snowflake**.  
3️⃣ **Time Travel is configurable, Fail-safe is not**.  
4️⃣ **Fail-safe is only for Permanent tables, Time Travel works for both Permanent & Transient tables**.  

