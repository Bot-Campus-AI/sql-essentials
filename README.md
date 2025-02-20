### **📌 Section 4: Subqueries and Common Table Expressions (CTEs)**  
Subqueries and CTEs allow you to **break down complex queries** into smaller, more manageable parts.  

---

## **🔹 1. Subqueries (Nested Queries)**
A **subquery** is a query **inside another query**.  
✅ Useful for filtering, calculations, and dynamic conditions.

### **📍 Example: Find Employees Earning Above Average Salary**
```sql
SELECT name, salary 
FROM Employees
WHERE salary > (SELECT AVG(salary) FROM Employees);
```
✅ **The subquery (`SELECT AVG(salary) FROM Employees`) calculates the average salary.**  
✅ **The outer query retrieves employees earning above this value.**  

---

### **📌 Types of Subqueries**
#### **1️⃣ Scalar Subquery (Returns a Single Value)**
💡 Used where **a single value is expected**, like in a `WHERE` clause.  

🔹 **Example: Find the Employee with the Lowest Salary**
```sql
SELECT name, salary 
FROM Employees
WHERE salary = (SELECT MIN(salary) FROM Employees);
```

#### **2️⃣ Multi-Row Subquery (Returns Multiple Values)**
💡 Used with `IN`, `ANY`, `ALL`.  

🔹 **Example: Get Employees Working in Departments with More Than 3 Employees**
```sql
SELECT name, department_id
FROM Employees
WHERE department_id IN (
    SELECT department_id 
    FROM Employees 
    GROUP BY department_id 
    HAVING COUNT(*) > 3
);
```

#### **3️⃣ Correlated Subquery (Depends on the Outer Query)**
💡 Runs **once per row in the outer query**.

🔹 **Example: Find Employees Earning More Than Their Department’s Average Salary**
```sql
SELECT name, department_id, salary 
FROM Employees e1
WHERE salary > (
    SELECT AVG(salary) 
    FROM Employees e2 
    WHERE e1.department_id = e2.department_id
);
```
✅ **Each row’s `department_id` is passed to the subquery dynamically.**  

---

## **🔹 2. Common Table Expressions (CTEs)**
A **CTE** is a temporary result set **used within a query**.  
✅ Improves **readability**, **performance**, and **modularity**.

### **📍 Syntax:**
```sql
WITH cte_name AS (
    SELECT column1, column2 FROM table_name WHERE condition
)
SELECT * FROM cte_name;
```

---

### **📌 Example: Get Departments with High Average Salaries**
```sql
WITH HighSalaryDepartments AS (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM Employees
    GROUP BY department_id
    HAVING AVG(salary) > 60000
)
SELECT d.department_name, hsd.avg_salary
FROM HighSalaryDepartments hsd
JOIN Departments d ON hsd.department_id = d.department_id;
```
✅ **CTE calculates departments with high salaries.**  
✅ **The main query joins this with the `Departments` table.**  

---

### **📌 Recursive CTE (For Hierarchies)**
A **recursive CTE** is useful for **hierarchical data** (e.g., employees & managers).  

🔹 **Example: Get Employee Reporting Structure**
```sql
WITH RECURSIVE EmployeeHierarchy AS (
    SELECT employee_id, name, manager_id, 1 AS level
    FROM Employees
    WHERE manager_id IS NULL  -- Start with top-level managers
    UNION ALL
    SELECT e.employee_id, e.name, e.manager_id, eh.level + 1
    FROM Employees e
    JOIN EmployeeHierarchy eh ON e.manager_id = eh.employee_id
)
SELECT * FROM EmployeeHierarchy ORDER BY level;
```
✅ **Recursive CTE finds employees & their managers recursively.**  

---

### **📝 Summary**
| Feature  | Purpose |
|----------|---------|
| **Subquery** | Query inside another query. |
| **Scalar Subquery** | Returns a single value (e.g., `AVG(salary)`). |
| **Multi-Row Subquery** | Returns multiple values (e.g., `IN`, `ANY`). |
| **Correlated Subquery** | Uses outer query data (e.g., per-department salary check). |
| **CTE** | Temporary result set for cleaner queries. |
| **Recursive CTE** | Used for hierarchical data (e.g., managers & employees). |

---
