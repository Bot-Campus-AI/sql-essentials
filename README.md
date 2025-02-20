Here is the corrected version with properly formatted tables so you can directly copy-paste into your Markdown file.

---

# **📌 Normalization Forms (1NF, 2NF, 3NF, BCNF) – Simple Examples**  

## **🔹 1NF (First Normal Form) – Remove Repeating Groups**  

### **❌ Unnormalized Table (Repeating Groups)**  

| order_id | customer_name | items_ordered         |
|----------|--------------|----------------------|
| 101      | Alice        | Apple, Orange        |
| 102      | Bob          | Banana               |
| 103      | Charlie      | Grapes, Mango, Apple |

❌ **Problem:**  
- The `items_ordered` column contains **multiple values (not atomic)**.  
- Queries like "How many Apples were ordered?" become difficult.

### ✅ **Normalized Table (1NF)**  

| order_id | customer_name | item_ordered |
|----------|--------------|-------------|
| 101      | Alice        | Apple       |
| 101      | Alice        | Orange      |
| 102      | Bob          | Banana      |
| 103      | Charlie      | Grapes      |
| 103      | Charlie      | Mango       |
| 103      | Charlie      | Apple       |

✅ **Fix:**  
- Each row now represents only **one item per order**.  
- Querying individual items is now **much easier**.

---

## **🔹 2NF (Second Normal Form) – Remove Partial Dependencies**  

### **❌ 1NF Table with Partial Dependency**  

| order_id | item_ordered | customer_name |
|----------|-------------|--------------|
| 101      | Apple       | Alice        |
| 101      | Orange      | Alice        |
| 102      | Banana      | Bob          |
| 103      | Grapes      | Charlie      |

❌ **Problem:**  
- **Primary Key = (`order_id`, `item_ordered`)**  
- `customer_name` **depends only on `order_id`, not on `item_ordered`**.  
- If Alice places multiple orders, her name is repeated.  

### ✅ **Normalized Tables (2NF)**  

#### **Orders Table**  

| order_id | customer_name |
|----------|--------------|
| 101      | Alice        |
| 102      | Bob          |
| 103      | Charlie      |

#### **OrderDetails Table**  

| order_id | item_ordered |
|----------|-------------|
| 101      | Apple       |
| 101      | Orange      |
| 102      | Banana      |
| 103      | Grapes      |

✅ **Fix:**  
- `customer_name` is now stored **only once**.  
- `OrderDetails` table now only stores **order-specific data**.

---

## **🔹 3NF (Third Normal Form) – Remove Transitive Dependencies**  

### **❌ 2NF Table with Transitive Dependency**  

| employee_id | employee_name | department | department_location |
|------------|--------------|-----------|------------------|
| 1          | Alice        | IT        | New York        |
| 2          | Bob          | HR        | London          |
| 3          | Charlie      | IT        | New York        |

❌ **Problem:**  
- **Primary Key = `employee_id`**  
- `department_location` **depends on `department`, NOT on `employee_id`**.  
- If IT moves to a new location, we must update multiple rows.  

### ✅ **Normalized Tables (3NF)**  

#### **Departments Table**  

| department_id | department_name | location  |
|--------------|---------------|----------|
| 1            | IT            | New York |
| 2            | HR            | London   |

#### **Employees Table**  

| employee_id | employee_name | department_id |
|------------|--------------|--------------|
| 1          | Alice        | 1            |
| 2          | Bob          | 2            |
| 3          | Charlie      | 1            |

✅ **Fix:**  
- `department_location` is now stored **only once** in `Departments`.  
- Employees reference their department **via a foreign key**.

---

## **🔹 BCNF (Boyce-Codd Normal Form) – Stronger 3NF**  

### **❌ 3NF Table with BCNF Violation**  

| student_id | course_id | instructor  |
|------------|----------|------------|
| 1          | DB101    | Prof. Smith |
| 2          | DB101    | Prof. Smith |
| 3          | ML201    | Prof. Brown |
| 4          | ML201    | Prof. Brown |

❌ **Problem:**  
- **Primary Key = (`student_id`, `course_id`)**  
- `instructor` **depends only on `course_id`**, not on (`student_id`, `course_id`).  
- If DB101 changes instructors, multiple rows must be updated.

### ✅ **Normalized Tables (BCNF)**  

#### **Courses Table**  

| course_id | instructor  |
|----------|------------|
| DB101     | Prof. Smith |
| ML201     | Prof. Brown |

#### **CourseEnrollment Table**  

| student_id | course_id |
|------------|----------|
| 1          | DB101    |
| 2          | DB101    |
| 3          | ML201    |
| 4          | ML201    |

✅ **Fix:**  
- Instructors are **stored only once** in the `Courses` table.  
- The `CourseEnrollment` table **only tracks enrollments**.

---

## **📌 Summary Table**  

| Normal Form | Fixes |
|------------|---------------------------------|
| **1NF**    | No repeating groups (atomic values). |
| **2NF**    | No partial dependencies (each column depends on the full primary key). |
| **3NF**    | No transitive dependencies (non-key attributes depend only on the primary key). |
| **BCNF**   | Every determinant must be a candidate key (removes remaining anomalies). |

---
