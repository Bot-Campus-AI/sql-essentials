### **📌 Section 3: Database Design and Optimization**  
#### **🔹 Lesson 3: Transactions & Concurrency Control**  
✅ **Topic 1: ACID Properties**  
✅ **Topic 2: COMMIT & ROLLBACK**  
✅ **Topic 3: Isolation Levels (READ COMMITTED, REPEATABLE READ, SERIALIZABLE)**  

---

## **🔹 1. ACID Properties (Ensuring Data Integrity in Transactions)**
A **transaction** is a group of SQL commands that **execute as a single unit**.  
To maintain **data integrity**, transactions follow **ACID** properties:

| **ACID Property** | **Definition** | **Example** |
|------------------|--------------|------------|
| **Atomicity** | All or nothing: If one part fails, everything rolls back. | Transferring money: If **debit** succeeds but **credit** fails, rollback. |
| **Consistency** | The database remains in a **valid state** before and after the transaction. | Ensuring an account never goes into **negative balance**. |
| **Isolation** | Transactions execute **independently** without interfering. | Two users booking the **last movie ticket** at the same time. |
| **Durability** | Once committed, data is permanently saved. | A successful order remains in the database even after a **system crash**. |

---

## **🔹 2. COMMIT & ROLLBACK (Managing Transactions)**
### **📍 Example 1: Atomicity (Ensuring All Steps Execute or None)**
```sql
BEGIN;

UPDATE Accounts SET balance = balance - 500 WHERE account_id = 1; -- Debit
UPDATE Accounts SET balance = balance + 500 WHERE account_id = 2; -- Credit

COMMIT; -- Confirms both actions
```
✅ **If both updates succeed, transaction is COMMITTED.**  
❌ **If an error occurs, we ROLLBACK (undo changes).**

---

### **📍 Example 2: Using ROLLBACK (Undo Changes on Error)**
```sql
BEGIN;

UPDATE Accounts SET balance = balance - 500 WHERE account_id = 1;
-- Simulating an error
UPDATE Accounts SET balance = balance + 500 WHERE account_id = 999; -- Non-existing account!

ROLLBACK; -- Undo both updates
```
❌ Since **account 999 does not exist**, the transaction fails.  
✅ `ROLLBACK` **undoes the debit**, preventing incorrect money transfer.

---

### **📍 Example 3: Explicit Savepoints (Partial Rollbacks)**
A **savepoint** allows partial rollbacks inside a transaction.

```sql
BEGIN;

UPDATE Employees SET salary = salary + 5000 WHERE employee_id = 1;
SAVEPOINT before_bonus;

UPDATE Employees SET salary = salary * 1.1 WHERE department_id = 2; -- Give 10% bonus to IT department

ROLLBACK TO before_bonus; -- Undo only the IT department bonus

COMMIT; -- Salary increase for employee_id = 1 is still applied
```
✅ **Only the IT department bonus is rolled back, while the first salary update is committed.**

---

## **🔹 3. Isolation Levels (Handling Concurrent Transactions)**
Isolation levels define **how transactions interact with each other**.

### **📍 1️⃣ READ COMMITTED (Default in PostgreSQL)**
🔹 A transaction **sees only committed data** from other transactions.  
🔹 If another transaction modifies the data before committing, our transaction **won’t see it**.

#### **Example: Preventing Dirty Reads**
🔸 **Transaction 1 (Uncommitted Update)**
```sql
BEGIN;
UPDATE Accounts SET balance = balance - 1000 WHERE account_id = 1;
-- Transaction is still open (not committed)
```

🔹 **Transaction 2 (Trying to Read)**
```sql
SELECT balance FROM Accounts WHERE account_id = 1;
```
✅ **Transaction 2 sees the original balance** (ignores uncommitted updates).  
❌ **Prevents Dirty Reads (reading uncommitted changes).**

🔹 **If Transaction 1 is rolled back, no incorrect data is seen.**  
```sql
ROLLBACK;
```

---

### **📍 2️⃣ REPEATABLE READ (Prevents Non-Repeatable Reads)**
🔹 **Ensures a transaction sees the same data throughout execution**.  
🔹 **Prevents another transaction from modifying rows that were already read**.

#### **Example: Preventing Inconsistent Data Reads**
🔸 **Transaction 1 (Read Employee Salary)**
```sql
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT salary FROM Employees WHERE employee_id = 1;  -- Returns 50000
```

🔹 **Transaction 2 (Another User Updates Salary)**
```sql
UPDATE Employees SET salary = 60000 WHERE employee_id = 1;
COMMIT;
```

🔹 **Transaction 1 (Re-reads Salary)**
```sql
SELECT salary FROM Employees WHERE employee_id = 1;
```
✅ **Still returns 50000 (same value as first read), even though salary was updated by another transaction!**  
❌ **Prevents inconsistent reads by locking the row for the transaction’s duration.**

---

### **📍 3️⃣ SERIALIZABLE (Strictest Isolation)**
🔹 **Ensures full isolation** by **executing transactions one at a time** (as if running sequentially).  
🔹 Prevents **phantom reads**, but can lead to **performance overhead**.

#### **Example: Preventing Phantom Reads**
🔸 **Transaction 1 (Count Employees in IT Department)**
```sql
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SELECT COUNT(*) FROM Employees WHERE department_id = 2;  -- Returns 5
```

🔹 **Transaction 2 (Another User Inserts a New Employee in IT)**
```sql
INSERT INTO Employees (name, department_id, salary) VALUES ('New Hire', 2, 70000);
COMMIT;
```

🔹 **Transaction 1 (Recounts Employees)**
```sql
SELECT COUNT(*) FROM Employees WHERE department_id = 2;
```
✅ **Still returns 5 (ignores the newly inserted record until COMMIT).**  
❌ **Prevents Phantom Reads (seeing new data that wasn’t there when the transaction started).**

---

## **🔹 Summary Table**
| **Isolation Level** | **Prevents** | **Example Scenario** |
|---------------------|-------------|----------------------|
| **READ COMMITTED** | Dirty Reads | See only committed data. |
| **REPEATABLE READ** | Non-Repeatable Reads | No changes in already read data. |
| **SERIALIZABLE** | Phantom Reads | Complete transaction isolation. |

---
