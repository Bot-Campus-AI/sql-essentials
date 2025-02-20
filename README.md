### **📌 Section 2: Data Retrieval and Manipulation**  
#### **🔹 Lesson 1: Filtering and Sorting Data**  
✅ **Topic: `WHERE`, `ORDER BY`, and `LIMIT` Clauses**  

---

## **🔹 1. The `WHERE` Clause (Filtering Data)**
The `WHERE` clause is used to **filter records** based on specific conditions.  

### **📍 Syntax:**
```sql
SELECT column1, column2 
FROM table_name 
WHERE condition;
```

### **📌 Example 1: Filter Employees by Department**
```sql
SELECT * FROM Employees 
WHERE department = 'IT';
```
✅ **Returns only employees in the IT department.**  

---

## **🔹 2. The `ORDER BY` Clause (Sorting Data)**
The `ORDER BY` clause **sorts the results** in ascending (`ASC`) or descending (`DESC`) order.  

### **📍 Syntax:**
```sql
SELECT column1, column2 
FROM table_name 
ORDER BY column_name ASC|DESC;
```

### **📌 Example 2: Sort Employees by Salary (Descending)**
```sql
SELECT name, salary 
FROM Employees 
ORDER BY salary DESC;
```
✅ **Returns employees sorted from highest to lowest salary.**  

### **📌 Example 3: Sort by Multiple Columns**
```sql
SELECT name, department, salary 
FROM Employees 
ORDER BY department ASC, salary DESC;
```
✅ **Sorts first by department (A–Z) and then by highest salary within each department.**  

---

## **🔹 3. The `LIMIT` Clause (Restricting Results)**
The `LIMIT` clause **restricts the number of rows** returned in a query.  

### **📍 Syntax:**
```sql
SELECT column1, column2 
FROM table_name 
LIMIT number_of_rows;
```

### **📌 Example 4: Get Top 3 Highest-Paid Employees**
```sql
SELECT name, salary 
FROM Employees 
ORDER BY salary DESC 
LIMIT 3;
```
✅ **Returns only the top 3 employees with the highest salaries.**  

### **📌 Example 5: Pagination Using `LIMIT` and `OFFSET`**
To get **the next 3 employees (pagination):**
```sql
SELECT name, salary 
FROM Employees 
ORDER BY salary DESC 
LIMIT 3 OFFSET 3;
```
✅ **Skips the first 3 rows and fetches the next 3.**  

---

### **📝 Key Takeaways**
- `WHERE` **filters data** based on a condition.  
- `ORDER BY` **sorts results** in `ASC` (default) or `DESC`.  
- `LIMIT` **restricts the number of records** returned.  
- `OFFSET` is used for **pagination**.  

---
