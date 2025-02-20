### **📌 Section 5: SQL Best Practices for Efficiency and Security**  
#### **🔹 Lesson 2: Preventing SQL Injection & Optimizing Query Performance**  

✅ **Topic 1: Preventing SQL Injection**  
✅ **Topic 2: Optimizing Query Performance**  

---

## **🔹 1. Preventing SQL Injection**
SQL Injection is a **serious security risk** where attackers manipulate SQL queries by injecting malicious inputs.  
✅ **If an application does not properly validate user input, an attacker can execute arbitrary SQL commands.**  

---

### **📍 Example of SQL Injection**
Imagine a **login system** where the query is built using **string concatenation**:

```sql
SELECT * FROM Users WHERE username = 'admin' AND password = 'password123';
```

An attacker **injects a malicious input**:  
```sql
' OR 1=1 --
```
The query becomes:  
```sql
SELECT * FROM Users WHERE username = '' OR 1=1 --' AND password = '';
```
✅ Since **`1=1` is always TRUE**, the attacker **logs in without credentials**. 😨  

---

### **📍 How to Prevent SQL Injection**
✅ **Use Prepared Statements & Parameterized Queries**  
Prepared statements **bind user input as parameters**, preventing injection.

#### **🔹 PostgreSQL Example (Using Parameterized Query)**
```sql
PREPARE login_query (TEXT, TEXT) AS
SELECT * FROM Users WHERE username = $1 AND password = $2;

EXECUTE login_query('admin', 'password123');
```
✅ **No direct string concatenation** → prevents malicious input.  

#### **🔹 In Python (Using psycopg2 for PostgreSQL)**
```python
import psycopg2

conn = psycopg2.connect(database="testdb", user="postgres", password="securepass")
cursor = conn.cursor()

username = input("Enter username: ")
password = input("Enter password: ")

query = "SELECT * FROM Users WHERE username = %s AND password = %s;"
cursor.execute(query, (username, password))  # Secure way to pass parameters

result = cursor.fetchall()
print(result)
```
✅ **User input is sanitized** before execution.  

---

## **🔹 2. Optimizing Query Performance**
Slow queries impact **application speed & database efficiency**.  
✅ Here are best practices for **optimizing SQL queries**.

---

### **📍 1️⃣ Use Indexes to Speed Up Queries**
Indexes **improve query speed** by allowing faster lookups.  
**❌ Without an Index:**
```sql
SELECT * FROM Employees WHERE name = 'Alice Johnson';
```
💡 **PostgreSQL performs a Full Table Scan (Slow).**  

**✅ Create an Index to Optimize:**
```sql
CREATE INDEX idx_employee_name ON Employees (name);
```
🔹 **Now, the query uses an Index Scan (Fast).**  

---

### **📍 2️⃣ Avoid SELECT \*** (Only Query Needed Columns)
**❌ Bad Practice:**
```sql
SELECT * FROM Employees;
```
💡 **Retrieves unnecessary data, slowing queries.**  

**✅ Optimized Query:**
```sql
SELECT name, salary FROM Employees WHERE department_id = 2;
```
🔹 **Only fetches required data**, reducing memory usage.

---

### **📍 3️⃣ Use `EXPLAIN ANALYZE` to Debug Slow Queries**
Before optimizing, **analyze the query execution plan**:
```sql
EXPLAIN ANALYZE
SELECT * FROM Employees WHERE department_id = 3;
```
✅ **Reveals if the query is using Index Scan or Sequential Scan.**  

---

### **📍 4️⃣ Optimize Joins by Indexing Foreign Keys**
If you **JOIN tables frequently**, index the **foreign keys**.

**❌ Slow JOIN Without Index:**
```sql
SELECT e.name, d.department_name
FROM Employees e
JOIN Departments d ON e.department_id = d.department_id;
```
💡 **If `department_id` is not indexed, it scans all rows.**  

**✅ Solution:**
```sql
CREATE INDEX idx_department_id ON Employees (department_id);
```
🔹 **Speeds up JOINs significantly.**  

---

### **📍 5️⃣ Use Proper Data Types for Faster Queries**
✅ **Use `INT` instead of `VARCHAR(20)` for IDs**  
✅ **Use `TIMESTAMP` instead of `VARCHAR(20)` for dates**  

**❌ Bad Example:**
```sql
CREATE TABLE Orders (
    order_id VARCHAR(20) PRIMARY KEY,  -- Slower Lookups
    order_date VARCHAR(20)
);
```
**✅ Optimized Example:**
```sql
CREATE TABLE Orders (
    order_id SERIAL PRIMARY KEY,  -- Faster Lookups
    order_date TIMESTAMP
);
```

---

### **📍 6️⃣ Partition Large Tables to Improve Query Speed**
**If a table has millions of rows**, **partitioning** improves query performance.  

#### **🔹 Example: Partition Employees Table by Department**
```sql
CREATE TABLE Employees_2024 PARTITION OF Employees
FOR VALUES IN (2024);
```
✅ **Queries targeting a specific year run faster.**  

---

### **📍 7️⃣ Use Caching for Repeated Queries**
Instead of running **heavy queries multiple times**, **cache results** using:  
✅ **Materialized Views**  
✅ **Redis or Memcached for web apps**  

---

## **🔹 Summary Table**
| **Best Practice** | **Why It's Important?** |
|------------------|------------------------|
| **Use Prepared Statements** | Prevents SQL Injection. |
| **Use Indexes** | Speeds up searches. |
| **Avoid `SELECT *`** | Reduces memory usage. |
| **Use `EXPLAIN ANALYZE`** | Debugs slow queries. |
| **Optimize Data Types** | Saves storage and speeds up lookups. |
| **Partition Large Tables** | Improves query speed for large datasets. |
| **Use Caching** | Reduces database load for repeated queries. |

---
