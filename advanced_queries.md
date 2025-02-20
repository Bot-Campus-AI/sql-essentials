### **📌 Section 3: Advanced Join Queries**  
#### **🔹 Lesson 2: SELF JOIN, CROSS JOIN, NATURAL JOIN**  

Advanced joins allow us to **query complex relationships within a table** or across multiple tables. Let’s explore **SELF JOIN, CROSS JOIN, and NATURAL JOIN**.

---

## **🔹 1. SELF JOIN (Joining a Table with Itself)**
A **SELF JOIN** is when a table is joined with itself.  
✅ This is useful for **hierarchical relationships**, such as **managers and employees**.

### **📍 Example: Find Each Employee's Manager**
#### **🔹 Step 1: Modify the Employees Table**
We add a **manager_id** column that references another employee.

```sql
ALTER TABLE Employees ADD COLUMN manager_id INT;
```

#### **🔹 Step 2: Update Employee-Manager Relationships**
```sql
UPDATE Employees SET manager_id = 2 WHERE name IN ('Charlie Davis', 'David Brown');
UPDATE Employees SET manager_id = 5 WHERE name IN ('Emily White', 'Frank Black');
UPDATE Employees SET manager_id = 7 WHERE name IN ('Harry Adams', 'Isla Blue');
```
✅ Now, some employees report to a **manager**.

#### **🔹 Step 3: Query Employees and Their Managers**
```sql
SELECT e.name AS Employee, m.name AS Manager
FROM Employees e
LEFT JOIN Employees m ON e.manager_id = m.employee_id;
```

**Expected Output:**
| Employee     | Manager    |
|-------------|-----------|
| Charlie Davis | Bob Smith  |
| David Brown   | Bob Smith  |
| Emily White   | Grace Green  |
| Frank Black   | Grace Green  |

🔹 **Key Takeaway:**  
- The table **joins itself** (`Employees e JOIN Employees m`).  
- `manager_id` links employees to their **manager’s `employee_id`**.

---

## **🔹 2. CROSS JOIN (Cartesian Product)**
A **CROSS JOIN** returns **all possible combinations** of two tables.  
✅ This is useful for **generating test data or combinations**.

### **📍 Example: Combine Employees and Departments**
```sql
SELECT e.name, d.department_name
FROM Employees e
CROSS JOIN Departments d;
```
**Expected Output:**
Every employee is paired with every department:

| Employee      | Department  |
|--------------|------------|
| Alice Johnson | HR         |
| Alice Johnson | IT         |
| Alice Johnson | Finance    |
| Bob Smith    | HR         |
| Bob Smith    | IT         |
| Bob Smith    | Finance    |

🔹 **Key Takeaway:**  
- No `ON` condition → **Every row of Table A pairs with every row of Table B**.  
- If Employees = 20 and Departments = 5, we get **20 × 5 = 100 rows**.

---

## **🔹 3. NATURAL JOIN (Auto-Matching Columns)**
A **NATURAL JOIN** automatically joins tables **based on common column names**.  
✅ This is useful when both tables **share the same column names**.

### **📍 Example: Join Employees and Departments**
```sql
SELECT * 
FROM Employees 
NATURAL JOIN Departments;
```
💡 **Assumptions:**  
- **`Employees` and `Departments` both have `department_id`**.  
- `NATURAL JOIN` automatically **matches `department_id`** without needing an `ON` condition.

**Expected Output:** *(Similar to INNER JOIN but automatic matching)*
| employee_id | name | department_id | salary | department_name |
|------------|------|--------------|--------|----------------|
| 1          | Alice Johnson | 1  | 55000 | HR |
| 2          | Bob Smith     | 2  | 75000 | IT |
| 3          | Charlie Davis | 3  | 60000 | Finance |

🔹 **Key Takeaway:**  
- **Auto-detects common columns** (e.g., `department_id`).  
- Works like **INNER JOIN**, but without specifying `ON department_id = department_id`.

---

### **📝 Summary**
| Join Type   | Purpose |
|------------|---------|
| **SELF JOIN** | Matches rows within the same table (e.g., Employee → Manager). |
| **CROSS JOIN** | Generates all possible row combinations between two tables. |
| **NATURAL JOIN** | Automatically joins tables based on common column names. |

---
