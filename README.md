### **📌 Section 4: Advanced Database Objects**  
#### **🔹 Lesson 2: Triggers & Event-Driven Automation**  
✅ **Topic 1: What Are Triggers?**  
✅ **Topic 2: Creating and Managing Triggers**  
✅ **Topic 3: Practical Use Cases of Triggers**  

---

## **🔹 1. What Are Triggers?**  
A **Trigger** is an **automatic action** executed **before or after** specific database events like `INSERT`, `UPDATE`, or `DELETE`.  
✅ **Triggers enforce business rules & data integrity.**  
✅ **Triggers execute automatically** when an event occurs.  

---

## **🔹 2. Creating and Managing Triggers**  

### **📍 Basic Syntax for Creating a Trigger**
```sql
CREATE TRIGGER trigger_name
{ BEFORE | AFTER } { INSERT | UPDATE | DELETE }
ON table_name
FOR EACH { ROW | STATEMENT }
EXECUTE FUNCTION function_name();
```
💡 **Triggers require a function** that contains the logic to execute.

---

### **📍 Example 1: Audit Log for Salary Changes**  
✅ Whenever an employee's **salary is updated**, PostgreSQL **automatically logs the changes** into an `AuditLog` table.  

#### **Step 1: Create an Audit Log Table**
```sql
CREATE TABLE AuditLog (
    log_id SERIAL PRIMARY KEY,
    employee_id INT,
    old_salary DECIMAL(10,2),
    new_salary DECIMAL(10,2),
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **Step 2: Create a Function for the Trigger**
```sql
CREATE OR REPLACE FUNCTION log_salary_changes()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO AuditLog (employee_id, old_salary, new_salary)
    VALUES (OLD.employee_id, OLD.salary, NEW.salary);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
✅ The function **stores the old and new salary values** whenever an update happens.

#### **Step 3: Create a Trigger That Calls the Function**
```sql
CREATE TRIGGER salary_update_trigger
AFTER UPDATE ON Employees
FOR EACH ROW
WHEN (OLD.salary IS DISTINCT FROM NEW.salary)
EXECUTE FUNCTION log_salary_changes();
```
✅ Now, every time an employee's **salary is updated**, PostgreSQL **logs the change automatically**.

#### **Step 4: Test the Trigger**
```sql
UPDATE Employees SET salary = 85000 WHERE employee_id = 3;
SELECT * FROM AuditLog;
```
✅ **Expected Result:**  
| log_id | employee_id | old_salary | new_salary | updated_at |
|--------|------------|------------|------------|-------------|
| 1      | 3          | 60000       | 85000      | 2025-02-20  |

---

### **📍 Example 2: Prevent Deleting Employees Without Manager Approval**
✅ If an employee is deleted **without manager approval**, PostgreSQL **blocks the action**.

#### **Step 1: Create a Function to Restrict Deletion**
```sql
CREATE OR REPLACE FUNCTION prevent_employee_deletion()
RETURNS TRIGGER AS $$
BEGIN
    RAISE EXCEPTION 'Employee cannot be deleted without manager approval.';
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;
```
✅ This **prevents accidental employee deletion**.

#### **Step 2: Create a `BEFORE DELETE` Trigger**
```sql
CREATE TRIGGER prevent_delete_trigger
BEFORE DELETE ON Employees
FOR EACH ROW
EXECUTE FUNCTION prevent_employee_deletion();
```

#### **Step 3: Test the Trigger**
```sql
DELETE FROM Employees WHERE employee_id = 4;
```
❌ **Error Message:** `Employee cannot be deleted without manager approval.`  
✅ **Ensures no unauthorized deletion occurs.**

---

## **🔹 3. Practical Use Cases of Triggers**
| Use Case | Trigger Action |
|----------|---------------|
| **Audit Log** | Logs changes when data is updated (`UPDATE`). |
| **Data Validation** | Prevents invalid operations like deleting records without permission. |
| **Automatic Calculations** | Updates a column automatically after `INSERT` (e.g., tax calculations). |
| **Enforce Business Rules** | Prevents salary reductions below a minimum threshold. |
| **Trigger-Based Notifications** | Sends email alerts on database updates. |

---

## **🔹 Summary**
| **Concept** | **Purpose** |
|------------|------------|
| **Trigger** | Automates actions on `INSERT`, `UPDATE`, `DELETE` |
| **BEFORE Trigger** | Executes **before** an event (to validate or block). |
| **AFTER Trigger** | Executes **after** an event (to log or process data). |
| **FOR EACH ROW** | Runs for every row affected. |
| **FOR EACH STATEMENT** | Runs once per query execution. |

---
