### **🔹 Default Queries You Can Execute in PostgreSQL**  
Once your PostgreSQL database is up and running, here are some essential queries to **explore databases, tables, users, and configurations**.

---

## **🔹 1. Database-Level Queries**
#### ✅ **View All Databases**
```sql
\l
```
(Shortcut for `\list` - Shows all databases in the PostgreSQL instance)

#### ✅ **Switch to a Different Database**
```sql
\c database_name
```
(Example: `\c companyDB` to switch to `companyDB`)

#### ✅ **Check the Current Database**
```sql
SELECT current_database();
```

---

## **🔹 2. Table-Level Queries**
#### ✅ **View All Tables in the Current Database**
```sql
\dt
```
(Shows all tables in the current database)

#### ✅ **Describe a Specific Table (Table Schema)**
```sql
\d table_name
```
(Example: `\d Employees` to see the table structure)

#### ✅ **List All Columns of a Table**
```sql
SELECT column_name, data_type 
FROM information_schema.columns 
WHERE table_name = 'table_name';
```
(Example: `table_name = 'employees'`)

---

## **🔹 3. User & Role Queries**
#### ✅ **View All Users/Roles**
```sql
\du
```
(Lists all database users and their roles)

#### ✅ **Check the Current User**
```sql
SELECT current_user;
```

#### ✅ **Create a New User**
```sql
CREATE USER new_user WITH PASSWORD 'secure_password';
```

#### ✅ **Grant a User Access to a Database**
```sql
GRANT ALL PRIVILEGES ON DATABASE companyDB TO new_user;
```

---

## **🔹 4. Checking Active Connections & Running Queries**
#### ✅ **View All Active Database Connections**
```sql
SELECT * FROM pg_stat_activity;
```

#### ✅ **Kill a Specific Query (Using PID)**
```sql
SELECT pg_terminate_backend(pid);
```
(*Replace `pid` with the process ID from `pg_stat_activity` output*)

---

## **🔹 5. System & Configuration Queries**
#### ✅ **Check PostgreSQL Version**
```sql
SELECT version();
```

#### ✅ **Show Server Configuration Settings**
```sql
SHOW all;
```
(Displays all system parameters and settings)

#### ✅ **Check Where PostgreSQL is Installed**
```sql
SHOW data_directory;
```

---

## **🔹 6. Checking and Managing Indexes**
#### ✅ **View Indexes on a Table**
```sql
SELECT indexname, indexdef 
FROM pg_indexes 
WHERE tablename = 'table_name';
```
(Example: Replace `table_name` with your table, like `'employees'`)

#### ✅ **Create an Index**
```sql
CREATE INDEX idx_salary ON Employees (salary);
```
(Improves query performance when searching by salary)

---

## **🔹 7. Database Backup & Restore**
#### ✅ **Backup a Database**
Run this in **Command Prompt (cmd)**:
```sh
pg_dump -U postgres -d companyDB -F c -f backup_file.dump
```
(Creates a compressed backup of `companyDB`)

#### ✅ **Restore a Database**
```sh
pg_restore -U postgres -d companyDB backup_file.dump
```
(Restores the database from backup)

---

### **🔹 Summary**
| **Command** | **Purpose** |
|------------|------------|
| `\l` | List all databases |
| `\c db_name` | Switch to a database |
| `\dt` | Show tables in a database |
| `\d table_name` | Describe a table (structure) |
| `\du` | List users/roles |
| `SELECT * FROM pg_stat_activity;` | View running queries |
| `SELECT version();` | Check PostgreSQL version |
| `SHOW data_directory;` | Find PostgreSQL installation directory |
| `pg_dump -U postgres -d db_name -F c -f backup_file.dump` | Backup database |
| `pg_restore -U postgres -d db_name backup_file.dump` | Restore database |
