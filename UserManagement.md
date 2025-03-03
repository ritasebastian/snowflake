# **🚀 Snowflake User Management & Access Control (RBAC)**
Managing **users, roles, and privileges** in Snowflake is essential for security and access control. Snowflake follows **Role-Based Access Control (RBAC)**, where **roles control privileges, and users are assigned roles**.

---

## **🔹 1. Snowflake User Management Components**
| **Component** | **Description** |
|--------------|----------------|
| **User** | An individual account with login credentials. |
| **Role** | Defines privileges and access to objects (tables, databases, warehouses). |
| **Privileges** | Permissions assigned to roles (SELECT, INSERT, CREATE, etc.). |
| **Database & Schema** | Logical structures for storing tables and views. |
| **Warehouse** | Compute engine for executing queries. |

---

## **🔹 2. Creating & Managing Users in Snowflake**
### **1️⃣ Create a New User**
```sql
CREATE USER john_doe 
PASSWORD = 'Secure@123' 
DEFAULT_ROLE = analyst 
MUST_CHANGE_PASSWORD = TRUE;
```
✅ **Key Parameters**:
- `PASSWORD` → Initial password (User must change it on first login).
- `DEFAULT_ROLE` → The default role assigned upon login.
- `MUST_CHANGE_PASSWORD = TRUE` → Forces password reset.

---
### **2️⃣ Modify an Existing User**
```sql
ALTER USER john_doe SET COMMENT = 'Data Analyst';
ALTER USER john_doe SET DEFAULT_ROLE = data_engineer;
```
---
### **3️⃣ List All Users**
```sql
SHOW USERS;
```
---
### **4️⃣ Drop a User**
```sql
DROP USER john_doe;
```

---

## **🔹 3. Role-Based Access Control (RBAC) in Snowflake**
Snowflake **manages access using roles, not individual users**.  

### **1️⃣ Create a New Role**
```sql
CREATE ROLE data_engineer;
```
---
### **2️⃣ Assign Role to a User**
```sql
GRANT ROLE data_engineer TO USER john_doe;
```
---
### **3️⃣ Grant Privileges to a Role**
✅ **Allow role to access a specific database and schema**
```sql
GRANT USAGE ON DATABASE company_db TO ROLE data_engineer;
GRANT USAGE ON SCHEMA company_db.sales TO ROLE data_engineer;
```
✅ **Allow role to read and write a table**
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLE company_db.sales.transactions TO ROLE data_engineer;
```
---
### **4️⃣ Check Role Assignments**
```sql
SHOW GRANTS TO USER john_doe;
```
---
### **5️⃣ Remove a Role from a User**
```sql
REVOKE ROLE data_engineer FROM USER john_doe;
```
---
### **6️⃣ Drop a Role**
```sql
DROP ROLE data_engineer;
```
---
## **🔹 4. Snowflake Predefined Roles**
| **Role** | **Description** |
|---------|----------------|
| **ACCOUNTADMIN** | Full account control, including billing. |
| **SECURITYADMIN** | Manages users, roles, and privileges. |
| **SYSADMIN** | Manages objects like warehouses, databases, and schemas. |
| **PUBLIC** | Default role assigned to all users (minimal access). |
| **USERADMIN** | Manages user creation and role assignment. |

---
## **🔹 5. Warehouse Management & Resource Control**
### **1️⃣ Create a New Warehouse for Users**
```sql
CREATE WAREHOUSE data_warehouse
WITH 
    WAREHOUSE_SIZE = 'XSMALL'
    AUTO_SUSPEND = 60
    AUTO_RESUME = TRUE;
```
---
### **2️⃣ Grant Access to a Role**
```sql
GRANT USAGE ON WAREHOUSE data_warehouse TO ROLE data_engineer;
```
---
### **3️⃣ Monitor Warehouse Usage**
```sql
SHOW WAREHOUSES;
SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY;
```

---
## **🔹 6. Security Best Practices for User Management**
✅ **Use RBAC:** Assign privileges to roles, not individual users.  
✅ **Enforce Strong Passwords:** Require **complex passwords** and **multi-factor authentication (MFA)**.  
✅ **Enable Auto-Suspend for Warehouses:** Avoid unnecessary compute costs.  
✅ **Limit ACCOUNTADMIN Role Usage:** Only for super-admins.  
✅ **Regularly Audit Role Privileges:**  
```sql
SHOW GRANTS ON ROLE data_engineer;
```
✅ **Revoke Unused Accounts:**  
```sql
DROP USER old_user;
```

---

## **🚀 Summary: Snowflake User Management Cheatsheet**
| **Action** | **Command** |
|------------|------------|
| **Create User** | `CREATE USER john_doe PASSWORD = 'Secure@123' DEFAULT_ROLE = analyst;` |
| **Modify User** | `ALTER USER john_doe SET DEFAULT_ROLE = data_engineer;` |
| **Delete User** | `DROP USER john_doe;` |
| **Create Role** | `CREATE ROLE data_engineer;` |
| **Assign Role to User** | `GRANT ROLE data_engineer TO USER john_doe;` |
| **Grant Privileges to Role** | `GRANT SELECT ON TABLE company_db.sales TO ROLE data_engineer;` |
| **Revoke Role from User** | `REVOKE ROLE data_engineer FROM USER john_doe;` |
| **Grant Warehouse Access** | `GRANT USAGE ON WAREHOUSE data_warehouse TO ROLE data_engineer;` |

---

