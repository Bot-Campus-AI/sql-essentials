### **📌 Section 4: Advanced Database Objects**  
#### **🔹 Lesson 1: Views, Materialized Views & Stored Procedures**  
✅ **Topic 1: Views**  
✅ **Topic 2: Materialized Views**  
✅ **Topic 3: Stored Procedures**  

---

## **🔹 1. Views (Virtual Tables)**
A **view** is a **virtual table** based on a `SELECT` query.  
✅ **Views simplify complex queries** by storing reusable query logic.  
✅ They **do not store data**, just the query definition.  

### **📍 Creating a View**
```sql
CREATE VIEW EmployeeSalaries AS 
SELECT name, department_id, salary 
FROM Employees 
WHERE salary > 60000;
```
✅ Now, instead of running the full query, use:  
```sql
SELECT * FROM EmployeeSalaries;
```
💡 **The view updates automatically** when underlying data changes.

---

### **📍 Updating Data Through Views**
If a view is based on a **single table**, you can update data through it.

```sql
UPDATE EmployeeSalaries
SET salary = 70000
WHERE name = 'Alice Johnson';
```
✅ This **updates the underlying `Employees` table**.

💡 **Limitations:**  
- Views **cannot update multiple tables**.  
- If a view has **aggregations (`SUM()`, `AVG()`)**, it **cannot be updated**.  

---

## **🔹 2. Materialized Views (Stored Results of Queries)**
A **Materialized View** is like a regular **View**, but it **stores query results** for better performance.  
✅ **Faster for large datasets** (since data is precomputed).  
✅ **Must be refreshed manually** to reflect new data.

### **📍 Creating a Materialized View**
```sql
CREATE MATERIALIZED VIEW HighPaidEmployees AS
SELECT name, department_id, salary 
FROM Employees 
WHERE salary > 60000;
```
✅ The data is **stored** at creation, improving performance.

---

### **📍 Refreshing a Materialized View**
Since data **does not update automatically**, refresh it when needed.

```sql
REFRESH MATERIALIZED VIEW HighPaidEmployees;
```
💡 **Use Case:**  
If the `Employees` table changes **frequently**, but **analytics queries don’t need real-time updates**, a **Materialized View** saves computation time.

---

### **📍 Dropping Views**
1️⃣ **Drop a Normal View**
```sql
DROP VIEW EmployeeSalaries;
```
2️⃣ **Drop a Materialized View**
```sql
DROP MATERIALIZED VIEW HighPaidEmployees;
```

---

## **🔹 3. Stored Procedures (Reusable SQL Code)**
A **Stored Procedure** is a **precompiled SQL block** that **executes multiple SQL commands**.  
✅ **Increases efficiency** by reducing redundant queries.  
✅ **Allows procedural logic (IF, LOOP, etc.).**  

---

### **📍 Creating a Stored Procedure**
```sql
CREATE PROCEDURE UpdateSalary(employee_id INT, new_salary DECIMAL)
LANGUAGE SQL
AS $$
    UPDATE Employees 
    SET salary = new_salary
    WHERE employee_id = employee_id;
$$;
```
✅ This **updates an employee’s salary** using a stored procedure.

---

### **📍 Executing a Stored Procedure**
```sql
CALL UpdateSalary(3, 75000);
```
✅ **Pass parameters dynamically**, making salary updates reusable.

---

### **📍 Stored Procedure with Conditional Logic**
```sql
CREATE PROCEDURE GiveBonus(department_id INT, bonus_amount DECIMAL)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE Employees
    SET salary = salary + bonus_amount
    WHERE department_id = department_id;
END;
$$;
```
✅ **Adds a salary bonus** to all employees in a department.

```sql
CALL GiveBonus(2, 5000);
```
💡 **Now, all IT employees get a ₹5,000 bonus**.

---

## **🔹 Summary**
| Feature | Purpose |
|---------|---------|
| **View** | Simplifies complex queries (virtual table). |
| **Materialized View** | Stores query results for better performance. |
| **Stored Procedure** | Runs precompiled SQL logic with parameters. |

---
