### **📌 Section 5: Best Practices in SQL**  
#### **🔹 Lesson 1: Working with Different Database Systems**  
✅ **Topic: SQL in MySQL, PostgreSQL, and SQL Server**  

Different database systems (RDBMS) share **core SQL principles**, but each has **syntax variations & unique features**. Let's compare **MySQL, PostgreSQL, and SQL Server** across different SQL operations.

---

## **🔹 1. Basic Query Syntax Comparison**  
| **Operation** | **MySQL** | **PostgreSQL** | **SQL Server** |
|--------------|----------|---------------|--------------|
| **Select Data** | `SELECT * FROM Employees;` | `SELECT * FROM Employees;` | `SELECT * FROM Employees;` |
| **Limit Rows** | `SELECT * FROM Employees LIMIT 5;` | `SELECT * FROM Employees LIMIT 5;` | `SELECT TOP 5 * FROM Employees;` |
| **String Concatenation** | `SELECT CONCAT(name, ' works in ', department) FROM Employees;` | `SELECT name || ' works in ' || department FROM Employees;` | `SELECT name + ' works in ' + department FROM Employees;` |
| **Auto-Increment (PK)** | `INT AUTO_INCREMENT PRIMARY KEY` | `SERIAL PRIMARY KEY` | `INT IDENTITY(1,1) PRIMARY KEY` |
| **Current Timestamp** | `NOW()` | `CURRENT_TIMESTAMP` | `GETDATE()` |

🔹 **Key Differences:**  
- **MySQL & PostgreSQL use `LIMIT`**, while **SQL Server uses `TOP`**.  
- **String concatenation differs** (`CONCAT()`, `||`, `+`).  
- **Auto-increment primary keys differ** (`AUTO_INCREMENT` vs. `SERIAL` vs. `IDENTITY`).  

---

## **🔹 2. Joins & Set Operations**
### **📍 LEFT JOIN Example**
| **MySQL** | **PostgreSQL** | **SQL Server** |
|----------|---------------|--------------|
| ```sql SELECT e.name, d.department_name FROM Employees e LEFT JOIN Departments d ON e.department_id = d.department_id; ``` | ```sql SELECT e.name, d.department_name FROM Employees e LEFT JOIN Departments d ON e.department_id = d.department_id; ``` | ```sql SELECT e.name, d.department_name FROM Employees e LEFT JOIN Departments d ON e.department_id = d.department_id; ``` |

✅ **No major difference in JOIN syntax across RDBMS.**  

---

### **📍 UNION Example**
| **MySQL** | **PostgreSQL** | **SQL Server** |
|----------|---------------|--------------|
| ```sql SELECT name FROM Employees UNION SELECT name FROM Customers; ``` | ```sql SELECT name FROM Employees UNION SELECT name FROM Customers; ``` | ```sql SELECT name FROM Employees UNION SELECT name FROM Customers; ``` |

✅ **Same syntax for `UNION`, `INTERSECT`, and `EXCEPT` in PostgreSQL & SQL Server** but **MySQL does not support `INTERSECT` and `EXCEPT`**.

---

## **🔹 3. Transactions & Error Handling**
### **📍 Example: Using Transactions**
| **MySQL** | **PostgreSQL** | **SQL Server** |
|----------|---------------|--------------|
| ```sql START TRANSACTION; UPDATE Accounts SET balance = balance - 100 WHERE account_id = 1; COMMIT; ``` | ```sql BEGIN; UPDATE Accounts SET balance = balance - 100 WHERE account_id = 1; COMMIT; ``` | ```sql BEGIN TRANSACTION; UPDATE Accounts SET balance = balance - 100 WHERE account_id = 1; COMMIT; ``` |

✅ **Syntax is almost identical, except MySQL uses `START TRANSACTION` and SQL Server uses `BEGIN TRANSACTION`**.  

---

### **📍 Example: Rollback on Error**
| **MySQL** | **PostgreSQL** | **SQL Server** |
|----------|---------------|--------------|
| ```sql START TRANSACTION; UPDATE Accounts SET balance = balance - 100 WHERE account_id = 1; ROLLBACK; ``` | ```sql BEGIN; UPDATE Accounts SET balance = balance - 100 WHERE account_id = 1; ROLLBACK; ``` | ```sql BEGIN TRANSACTION; UPDATE Accounts SET balance = balance - 100 WHERE account_id = 1; ROLLBACK; ``` |

✅ **Rollback behavior is the same across databases.**  

---

## **🔹 4. Stored Procedures & Functions**
### **📍 Example: Creating a Stored Procedure**
| **MySQL** | **PostgreSQL** | **SQL Server** |
|----------|---------------|--------------|
| ```sql DELIMITER $$ CREATE PROCEDURE UpdateSalary (IN emp_id INT, IN new_salary DECIMAL) BEGIN UPDATE Employees SET salary = new_salary WHERE employee_id = emp_id; END $$ DELIMITER ; ``` | ```sql CREATE FUNCTION UpdateSalary(emp_id INT, new_salary DECIMAL) RETURNS VOID AS $$ BEGIN UPDATE Employees SET salary = new_salary WHERE employee_id = emp_id; END $$ LANGUAGE plpgsql; ``` | ```sql CREATE PROCEDURE UpdateSalary (@emp_id INT, @new_salary DECIMAL) AS BEGIN UPDATE Employees SET salary = @new_salary WHERE employee_id = @emp_id; END; ``` |

✅ **Key Differences:**  
- **MySQL uses `DELIMITER $$` to define multi-line procedures.**  
- **PostgreSQL prefers functions over procedures (`plpgsql`).**  
- **SQL Server uses `@param` syntax instead of `IN` parameters.**  

---

## **🔹 5. Indexing & Optimization**
### **📍 Creating an Index**
| **MySQL** | **PostgreSQL** | **SQL Server** |
|----------|---------------|--------------|
| ```sql CREATE INDEX idx_employee_name ON Employees (name); ``` | ```sql CREATE INDEX idx_employee_name ON Employees (name); ``` | ```sql CREATE INDEX idx_employee_name ON Employees (name); ``` |

✅ **All three databases use similar syntax for indexing.**  

---

### **📍 Query Execution Plans**
| **MySQL** | **PostgreSQL** | **SQL Server** |
|----------|---------------|--------------|
| ```sql EXPLAIN SELECT * FROM Employees WHERE name = 'Alice'; ``` | ```sql EXPLAIN ANALYZE SELECT * FROM Employees WHERE name = 'Alice'; ``` | ```sql SET STATISTICS IO ON; SET STATISTICS TIME ON; SELECT * FROM Employees WHERE name = 'Alice'; ``` |

✅ **PostgreSQL provides detailed execution plans using `EXPLAIN ANALYZE`**.  
✅ **SQL Server requires `SET STATISTICS IO/TIME ON` for performance analysis**.  

---

## **🔹 6. Best Practices for Writing Cross-Compatible SQL**
| **Best Practice** | **Reason** |
|------------------|-----------|
| **Use ANSI Standard SQL** (`JOIN`, `GROUP BY`, `ORDER BY`) | Works across all databases. |
| **Avoid Proprietary Functions** (`IFNULL()` in MySQL vs. `COALESCE()` in PostgreSQL/SQL Server) | Ensures compatibility. |
| **Use Parameterized Queries for Security** | Prevents SQL injection. |
| **Use Explicit Transactions (`BEGIN`, `COMMIT`)** | Ensures data consistency. |
| **Optimize with Indexes & Query Plans (`EXPLAIN`)** | Improves performance. |

---

### **📝 Summary Table**
| Feature | MySQL | PostgreSQL | SQL Server |
|---------|------|------------|-----------|
| **Auto-increment** | `AUTO_INCREMENT` | `SERIAL` | `IDENTITY(1,1)` |
| **String Concatenation** | `CONCAT(a, b)` | `a || b` | `a + b` |
| **LIMIT rows** | `LIMIT` | `LIMIT` | `TOP` |
| **Transactions** | `START TRANSACTION` | `BEGIN` | `BEGIN TRANSACTION` |
| **Stored Procedures** | `DELIMITER $$` | `plpgsql` functions | `@parameters` syntax |
| **Indexing** | `CREATE INDEX` | `CREATE INDEX` | `CREATE INDEX` |
| **Execution Plan** | `EXPLAIN` | `EXPLAIN ANALYZE` | `SET STATISTICS IO/TIME ON` |

---
