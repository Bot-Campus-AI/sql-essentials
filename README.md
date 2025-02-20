### **📌 Section 3: Database Design and Optimization**  
#### **🔹 Lesson 1: Database Schema Design**  
✅ **Topic 1: Normalization and Relationships**  
✅ **Topic 2: Creating and Managing Tables**  

---

## **🔹 1. Normalization and Relationships**  
Normalization is the process of **structuring a relational database** to reduce **redundancy** and **improve integrity**. It follows **Normal Forms (NF)**, which are rules for organizing data.

### **📍 Key Normal Forms**  
| **Normal Form** | **Rule** |
|---------------|------------------------------|
| **1NF (First Normal Form)** | No duplicate rows, atomic values only. |
| **2NF (Second Normal Form)** | 1NF + No **partial dependencies** (each column depends on the entire primary key). |
| **3NF (Third Normal Form)** | 2NF + No **transitive dependencies** (non-key attributes depend only on the primary key). |
| **BCNF (Boyce-Codd NF)** | Stronger 3NF: Every determinant must be a candidate key. |

---
### **📌 Example: Normalizing an Employee Database**  

#### **💡 Unnormalized Table (Repeating Data)**  
Before normalization, department details are repeated for each employee.

```sql
CREATE TABLE EmployeeUnnormalized (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    department_location VARCHAR(100)
);
```

| **employee_id** | **name**  | **department** | **department_location** |
|---------------|-----------|---------------|---------------------|
| 1            | Alice     | IT            | New York           |
| 2            | Bob       | HR            | London             |
| 3            | Charlie   | IT            | New York           |

❌ **Issue:**  
- **Department names & locations are repeated** for multiple employees.  
- **If department locations change, we need to update multiple rows**.  

---

#### ✅ **Normalized Schema (3NF)**
To remove redundancy, **split the department details into a separate table**.

```sql
CREATE TABLE Departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(50) UNIQUE,
    location VARCHAR(100)
);

CREATE TABLE Employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department_id INT REFERENCES Departments(department_id),
    salary DECIMAL(10,2)
);
```

---

## **🔹 2. Creating and Managing Tables**
### **📍 Creating a Table**
```sql
CREATE TABLE Employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department_id INT REFERENCES Departments(department_id),
    salary DECIMAL(10,2) CHECK (salary > 0)
);
```
✅ **Features Used:**  
- `SERIAL PRIMARY KEY` → Auto-incrementing ID.  
- `VARCHAR(100) NOT NULL` → Name cannot be empty.  
- `REFERENCES` → Establishes foreign key relationship.  
- `CHECK (salary > 0)` → Ensures salary is positive.

---

### **📍 Modifying a Table (`ALTER TABLE`)**
1️⃣ **Add a New Column**  
```sql
ALTER TABLE Employees ADD COLUMN email VARCHAR(100) UNIQUE;
```
2️⃣ **Modify Column Data Type**  
```sql
ALTER TABLE Employees ALTER COLUMN salary TYPE FLOAT;
```
3️⃣ **Drop a Column**  
```sql
ALTER TABLE Employees DROP COLUMN email;
```

---

### **📍 Deleting a Table (`DROP TABLE`)**
```sql
DROP TABLE Employees;
```
⚠ **Caution:** This **permanently deletes** the table and its data.

---

## **🔹 Summary**
| Concept | Purpose |
|---------|---------|
| **Normalization** | Reduces redundancy & improves integrity |
| **1NF → 3NF → BCNF** | Increasing levels of optimization |
| **Primary Key (PK)** | Uniquely identifies rows |
| **Foreign Key (FK)** | Links tables to enforce relationships |
| **CREATE TABLE** | Defines a new table |
| **ALTER TABLE** | Modifies an existing table |
| **DROP TABLE** | Deletes a table |

---
