Let's insert more employees into the **Employees** table so that the `GROUP BY`, `HAVING`, and aggregation queries produce meaningful results.

---

## **🔹 Insert More Employee Records**
```sql
INSERT INTO Employees (name, department, salary) VALUES
('Alice Johnson', 'HR', 55000),
('Bob Smith', 'IT', 75000),
('Charlie Davis', 'Finance', 60000),
('David Brown', 'Sales', 62000),
('Emily White', 'IT', 85000),
('Frank Black', 'HR', 58000),
('Grace Green', 'IT', 79000),
('Harry Adams', 'Finance', 72000),
('Isla Blue', 'Marketing', 50000),
('Jack Carter', 'Marketing', 52000),
('Karen Miller', 'Sales', 65000),
('Liam Scott', 'IT', 90000),
('Mia Roberts', 'Sales', 58000),
('Noah Wilson', 'IT', 87000),
('Olivia Harris', 'HR', 56000),
('Peter Evans', 'Finance', 67000),
('Quinn Lee', 'IT', 78000),
('Ryan Walker', 'Sales', 70000),
('Sophia Allen', 'Marketing', 54000),
('Tom Baker', 'IT', 81000);
```

---

## **🔹 Check Employee Data**
```sql
SELECT * FROM Employees;
```
✅ Now, our dataset contains **20 employees** across different departments.

---

### **📌 Section 2: Data Retrieval and Manipulation**  
#### **🔹 Lesson 2: Aggregating Data with `GROUP BY` and `HAVING`**  

Aggregation functions help summarize data, and the `GROUP BY` clause allows us to **group data based on specific columns**. The `HAVING` clause is used to **filter grouped data**.

---

## **🔹 1. The `GROUP BY` Clause (Grouping Data)**
The `GROUP BY` clause is used with **aggregate functions** like `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()` to group results **based on one or more columns**.

### **📍 Syntax:**
```sql
SELECT column_name, AGGREGATE_FUNCTION(column_name)
FROM table_name
GROUP BY column_name;
```

---

### **📌 Example 1: Count Employees in Each Department**
```sql
SELECT department, COUNT(*) AS total_employees
FROM Employees
GROUP BY department;
```
✅ **Groups employees by department and counts the number of employees in each.**

| Department | Total Employees |
|------------|----------------|
| IT         | 5              |
| HR         | 3              |
| Sales      | 7              |

---

### **📌 Example 2: Calculate the Average Salary Per Department**
```sql
SELECT department, AVG(salary) AS avg_salary
FROM Employees
GROUP BY department;
```
✅ **Groups employees by department and calculates the average salary for each department.**  

---

## **🔹 2. The `HAVING` Clause (Filtering Grouped Data)**
- `HAVING` is used **after** `GROUP BY` to **filter aggregated results**.
- Unlike `WHERE`, which filters **individual rows**, `HAVING` filters **grouped data**.

### **📍 Syntax:**
```sql
SELECT column_name, AGGREGATE_FUNCTION(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

---

### **📌 Example 3: Departments with More Than 3 Employees**
```sql
SELECT department, COUNT(*) AS total_employees
FROM Employees
GROUP BY department
HAVING COUNT(*) > 3;
```
✅ **Only returns departments where employee count is greater than 3.**

| Department | Total Employees |
|------------|----------------|
| IT         | 5              |
| Sales      | 7              |

---

### **📌 Example 4: Departments with an Average Salary Above 60,000**
```sql
SELECT department, AVG(salary) AS avg_salary
FROM Employees
GROUP BY department
HAVING AVG(salary) > 60000;
```
✅ **Returns departments where the average salary is above 60,000.**

| Department | Avg Salary |
|------------|-----------|
| IT         | 75,000    |
| Sales      | 68,000    |

---

## **🔹 3. Combining `GROUP BY`, `HAVING`, and `ORDER BY`**
We can **combine all three clauses** for more powerful queries.

### **📌 Example 5: Find Departments with More Than 3 Employees and Order by Employee Count**
```sql
SELECT department, COUNT(*) AS total_employees
FROM Employees
GROUP BY department
HAVING COUNT(*) > 3
ORDER BY total_employees DESC;
```
✅ **Filters out small departments and sorts by employee count (highest first).**

---

### **📝 Key Takeaways**
- **`GROUP BY`** is used to aggregate data by a specific column.
- **Aggregate functions** (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`) operate on groups of rows.
- **`HAVING`** filters grouped results (use `WHERE` for individual row filtering).
- **`ORDER BY`** can be used to sort the final grouped results.

---
