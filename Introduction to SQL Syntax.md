### **Lesson 2: Introduction to SQL Syntax**  

### **🔹 What is SQL?**
SQL (**Structured Query Language**) is a **language used to interact with relational databases**. It allows users to **retrieve, manipulate, and manage data** efficiently.  

💡 **Real-World Example:**  
- A **bank** uses SQL to check customer balances.
- A **shopping website** uses SQL to store product details and customer orders.

---

### **🔹 Basic SQL Syntax Structure**  
SQL commands generally follow this pattern:

```sql
COMMAND column_name(s)
FROM table_name
WHERE condition
ORDER BY column_name;
```

👉 **Example:** Retrieving all customers from a database:  
```sql
SELECT * FROM Customers;
```

---

### **🔹 Common SQL Commands**
SQL is divided into different categories:

1️⃣ **Data Query Language (DQL)** – Retrieve data  
   - `SELECT` – Fetch data from tables  
   - `WHERE` – Filter data  

2️⃣ **Data Manipulation Language (DML)** – Modify data  
   - `INSERT` – Add new data  
   - `UPDATE` – Modify existing data  
   - `DELETE` – Remove data  

3️⃣ **Data Definition Language (DDL)** – Define database structure  
   - `CREATE` – Make a new table  
   - `ALTER` – Modify an existing table  
   - `DROP` – Delete a table  

4️⃣ **Data Control Language (DCL)** – Control access  
   - `GRANT` – Give permissions  
   - `REVOKE` – Remove permissions  

---

### **🔹 Example: Creating & Querying a Table**
1️⃣ **Create a Table:**  
```sql
CREATE TABLE Employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);
```

2️⃣ **Insert Data:**  
```sql
INSERT INTO Employees (name, department, salary)
VALUES ('Alice Johnson', 'HR', 55000.00);
```

3️⃣ **Retrieve Data:**  
```sql
SELECT * FROM Employees;
```

4️⃣ **Filter Data:**  
```sql
SELECT * FROM Employees WHERE department = 'HR';
```

---

### **🔹 Key Takeaways**
✅ **SQL Syntax follows a structured format**  
✅ **SQL commands are categorized into DQL, DML, DDL, and DCL**  
✅ **Basic SQL operations include SELECT, INSERT, UPDATE, DELETE**  

---
