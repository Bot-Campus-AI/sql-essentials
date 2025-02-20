### **📌 Section 3: Working with Joins in SQL**  
Joins allow us to **combine data from multiple tables** based on a common column.  

---

## **🔹 Step 1: Create Two Tables**
We will create **Employees** and **Departments** tables.  

```sql
CREATE TABLE Departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(50) UNIQUE
);

CREATE TABLE Employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department_id INT REFERENCES Departments(department_id),
    salary DECIMAL(10,2)
);
```
🔹 **`Departments`** contains a list of departments.  
🔹 **`Employees`** contains employee details and references `department_id` from `Departments`.  

---

## **🔹 Step 2: Insert Data into Departments Table**
```sql
INSERT INTO Departments (department_name) VALUES
('HR'),
('IT'),
('Finance'),
('Sales'),
('Marketing');
```

✅ **Now, `Departments` contains:**  
| department_id | department_name |
|--------------|----------------|
| 1            | HR             |
| 2            | IT             |
| 3            | Finance        |
| 4            | Sales          |
| 5            | Marketing      |

---

## **🔹 Step 3: Insert Data into Employees Table**
```sql
INSERT INTO Employees (name, department_id, salary) VALUES
('Alice Johnson', 1, 55000),
('Bob Smith', 2, 75000),
('Charlie Davis', 3, 60000),
('David Brown', 4, 62000),
('Emily White', 2, 85000),
('Frank Black', 1, 58000),
('Grace Green', 2, 79000),
('Harry Adams', 3, 72000),
('Isla Blue', 5, 50000),
('Jack Carter', 5, 52000),
('Karen Miller', 4, 65000),
('Liam Scott', 2, 90000),
('Mia Roberts', 4, 58000),
('Noah Wilson', 2, 87000),
('Olivia Harris', 1, 56000),
('Peter Evans', 3, 67000),
('Quinn Lee', 2, 78000),
('Ryan Walker', 4, 70000),
('Sophia Allen', 5, 54000),
('Tom Baker', 2, 81000);
```

✅ **Now, `Employees` contains:**  
| employee_id | name         | department_id | salary |
|------------|-------------|--------------|--------|
| 1          | Alice Johnson | 1           | 55000  |
| 2          | Bob Smith     | 2           | 75000  |
| 3          | Charlie Davis | 3           | 60000  |
| ...        | ...           | ...         | ...    |

---

## **🔹 Step 4: Perform Different Types of Joins**

---

### **1️⃣ INNER JOIN (Matching Data in Both Tables)**
The `INNER JOIN` returns **only the records that have matching values** in both tables.

```sql
SELECT e.name, e.salary, d.department_name
FROM Employees e
INNER JOIN Departments d ON e.department_id = d.department_id;
```
✅ **This returns only employees who have a valid department.**  
💡 **Employees assigned to non-existing departments will NOT be shown.**

---

### **2️⃣ LEFT JOIN (All Employees, Even Without Departments)**
The `LEFT JOIN` returns **all records from the left table (`Employees`)** and **matching records from the right table (`Departments`)**.

```sql
SELECT e.name, e.salary, d.department_name
FROM Employees e
LEFT JOIN Departments d ON e.department_id = d.department_id;
```
✅ **Shows all employees, even if they don’t belong to a department.**  
💡 **If there’s no department, `NULL` is shown for `department_name`.**

---

### **3️⃣ RIGHT JOIN (All Departments, Even Without Employees)**
The `RIGHT JOIN` returns **all records from the right table (`Departments`)** and **matching records from the left table (`Employees`)**.

```sql
SELECT e.name, e.salary, d.department_name
FROM Employees e
RIGHT JOIN Departments d ON e.department_id = d.department_id;
```
✅ **Shows all departments, even if no employees belong to them.**  
💡 **If there’s no employee in a department, `NULL` appears for `name` and `salary`.**

---

### **4️⃣ FULL JOIN (All Data from Both Tables)**
The `FULL JOIN` returns **all records from both tables**.  
If there is **no match**, `NULL` is returned for missing values.

```sql
SELECT e.name, e.salary, d.department_name
FROM Employees e
FULL JOIN Departments d ON e.department_id = d.department_id;
```
✅ **Shows all employees and all departments, even if some don’t match.**

---

## **🔹 Example Results**
| name           | salary  | department_name |
|---------------|--------|----------------|
| Alice Johnson | 55000  | HR             |
| Bob Smith     | 75000  | IT             |
| Charlie Davis | 60000  | Finance        |
| David Brown   | 62000  | Sales          |
| NULL          | NULL   | Marketing      | (RIGHT JOIN case) |
| NULL          | NULL   | IT Support     | (FULL JOIN case) |

---

### **📝 Key Takeaways**
- **`INNER JOIN`** → Only matching records from both tables.  
- **`LEFT JOIN`** → All employees, even if they don’t have a department.  
- **`RIGHT JOIN`** → All departments, even if they don’t have employees.  
- **`FULL JOIN`** → All data from both tables, filling missing values with `NULL`.

---
