### **Lesson 2: SQL Syntax – DML vs. DDL & Creating a Database**  

Now that you understand SQL syntax, let's **break it down into two major categories**:  
✅ **DDL (Data Definition Language)** – Defines and manages database structure.  
✅ **DML (Data Manipulation Language)** – Performs CRUD operations (Create, Read, Update, Delete).  

---

## **🔹 Step 1: Understanding DDL vs. DML**
### **1️⃣ Data Definition Language (DDL) – Deals with Structure**
DDL commands are used to **define** and **modify** database objects like tables, schemas, and indexes.

| **DDL Command** | **Purpose** |
|---------------|------------|
| `CREATE`   | Creates databases and tables |
| `ALTER`    | Modifies existing tables |
| `DROP`     | Deletes databases or tables |
| `TRUNCATE` | Deletes all rows in a table without logging |

👉 **Example:** Creating a new table  
```sql
CREATE TABLE Employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);
```

---

### **2️⃣ Data Manipulation Language (DML) – Deals with Data**
DML commands are used to **insert, update, retrieve, and delete data** inside tables.

| **DML Command** | **Purpose** |
|---------------|------------|
| `INSERT`   | Adds new records |
| `SELECT`   | Retrieves records |
| `UPDATE`   | Modifies existing records |
| `DELETE`   | Removes records |

👉 **Example:** Insert data into a table  
```sql
INSERT INTO Employees (name, department, salary)
VALUES ('Alice Johnson', 'HR', 55000.00);
```

---

## **🔹 Step 2: Create a Database**
Before using DDL and DML commands, **let’s create a database**.

### **🛠️ Create a New Database in PostgreSQL**
Run the following command in **psql**:

```sql
CREATE DATABASE companyDB;
```

🔹 **Verify the Database Exists**  
```sql
\l
```

🔹 **Switch to the New Database**  
```sql
\c companyDB
```

---

## **🔹 Step 3: Create a Table (DDL - `CREATE TABLE`)**
Inside `companyDB`, create an `Employees` table:

```sql
CREATE TABLE Employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);
```

🔹 **Verify the Table Exists**  
```sql
\d Employees
```

---

## **🔹 Step 4: Perform CRUD Operations (DML)**
Once the database and table are created, let’s perform **CRUD (Create, Read, Update, Delete) operations**.

### **✅ Create Data (INSERT)**
```sql
INSERT INTO Employees (name, department, salary)
VALUES 
('Alice Johnson', 'HR', 55000.00),
('Bob Smith', 'IT', 70000.00),
('Charlie Davis', 'Finance', 65000.00);
```

🔹 **Verify Data**  
```sql
SELECT * FROM Employees;
```

---

### **✅ Read Data (SELECT)**
1️⃣ **Retrieve All Employees**
```sql
SELECT * FROM Employees;
```
2️⃣ **Retrieve Employees from IT Department**
```sql
SELECT * FROM Employees WHERE department = 'IT';
```
3️⃣ **Sort Employees by Salary (Descending)**
```sql
SELECT * FROM Employees ORDER BY salary DESC;
```

---

### **✅ Update Data (UPDATE)**
1️⃣ **Increase Bob Smith’s Salary**
```sql
UPDATE Employees 
SET salary = 75000.00
WHERE name = 'Bob Smith';
```

2️⃣ **Change Alice Johnson’s Department**
```sql
UPDATE Employees 
SET department = 'Admin'
WHERE name = 'Alice Johnson';
```

🔹 **Verify Changes**
```sql
SELECT * FROM Employees;
```

---

### **✅ Delete Data (DELETE)**
1️⃣ **Remove an Employee**
```sql
DELETE FROM Employees 
WHERE name = 'Charlie Davis';
```

2️⃣ **Delete All Employees in IT Department**
```sql
DELETE FROM Employees 
WHERE department = 'IT';
```

🔹 **Verify Changes**
```sql
SELECT * FROM Employees;
```

---

### **🔹 Final Steps**
- **DDL Commands Recap**: `CREATE DATABASE`, `CREATE TABLE`, `ALTER`, `DROP`
- **DML Commands Recap**: `INSERT`, `SELECT`, `UPDATE`, `DELETE`
- You now have a **fully functional database** with CRUD operations.

---
