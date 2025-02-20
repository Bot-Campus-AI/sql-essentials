### **📌 Section 5: Best Practices in SQL**  
#### **🔹 Lesson 3: Database Backup & Recovery Strategies**  
✅ **Topic 1: Types of Database Backups**  
✅ **Topic 2: Backup Strategies in MySQL, PostgreSQL, and SQL Server**  
✅ **Topic 3: Recovery Techniques (Point-in-Time Recovery, Full Restoration, WAL Logs)**  

---

## **🔹 1. Types of Database Backups**  

| **Backup Type** | **Description** | **Use Case** |
|---------------|---------------|------------|
| **Full Backup** | Backs up the entire database (schema + data). | Complete recovery after system failure. |
| **Incremental Backup** | Backs up only **changed** data since the last backup. | Reduces backup time & storage. |
| **Differential Backup** | Backs up all data changed **since the last full backup**. | Faster restores than incremental backups. |
| **Logical Backup** | Exports database as SQL statements (`pg_dump`, `mysqldump`). | Used for migrating databases. |
| **Physical Backup** | Copies entire database files (binary format). | Used for **fast disaster recovery**. |
| **Point-in-Time Recovery (PITR)** | Restores database **to a specific moment**. | Used for undoing accidental changes. |

---

## **🔹 2. Backup Strategies in Different RDBMS**
### **📍 PostgreSQL: Full Backup Using `pg_dump`**
```sh
pg_dump -U postgres -h localhost -d mydatabase -F c -f backup_file.dump
```
✅ **Backup stored in a compressed format (`.dump`)**  
✅ Can be restored using `pg_restore`  

**Restore the Backup:**
```sh
pg_restore -U postgres -d mydatabase -F c backup_file.dump
```
---

### **📍 MySQL: Full Backup Using `mysqldump`**
```sh
mysqldump -u root -p mydatabase > backup.sql
```
✅ **Exports SQL statements**  
✅ **Portable format** for migration  

**Restore the Backup:**
```sh
mysql -u root -p mydatabase < backup.sql
```
---

### **📍 SQL Server: Full Backup Using `BACKUP DATABASE`**
```sql
BACKUP DATABASE MyDatabase
TO DISK = 'C:\backup\MyDatabase.bak'
WITH FORMAT, COMPRESSION;
```
✅ **Optimized for large databases**  
✅ **Supports compression**  

**Restore the Backup:**
```sql
RESTORE DATABASE MyDatabase 
FROM DISK = 'C:\backup\MyDatabase.bak';
```

---

## **🔹 3. Advanced Recovery Techniques**
### **📍 1️⃣ Point-in-Time Recovery (PITR)**
🔹 Used when you need to **recover to a specific moment** (e.g., before a mistake).  

#### **🔹 PostgreSQL: Using WAL Logs for PITR**
1️⃣ **Enable WAL (Write-Ahead Logging)**
```sh
echo "archive_mode = on" >> postgresql.conf
echo "archive_command = 'cp %p /backup/%f'" >> postgresql.conf
```
2️⃣ **Restore from a Full Backup**
```sh
pg_restore -U postgres -d mydatabase -F c backup_file.dump
```
3️⃣ **Replay WAL Logs Up to the Desired Time**
```sh
pg_ctl start -D /data --restore_command "cp /backup/%f %p"
```
✅ **Restores database up to a specific timestamp**  

---

### **📍 2️⃣ Incremental Backups for MySQL**
1️⃣ **Enable Binary Logging (`my.cnf`)**
```sh
log-bin=mysql-bin
expire_logs_days=7
```
2️⃣ **Create a Full Backup**
```sh
mysqldump --single-transaction --flush-logs -u root -p mydatabase > full_backup.sql
```
3️⃣ **Apply Incremental Changes**
```sh
mysqlbinlog mysql-bin.000001 | mysql -u root -p mydatabase
```
✅ **Restores MySQL to a specific state**  

---

## **🔹 4. Best Practices for Backup & Recovery**
| **Best Practice** | **Why It's Important?** |
|------------------|------------------------|
| **Schedule Regular Backups** | Ensures data is recoverable. |
| **Store Backups Offsite** | Protects against physical disasters. |
| **Test Restorations Regularly** | Prevents surprises during actual recovery. |
| **Use PITR for Critical Data** | Recovers data **before accidental deletions**. |
| **Automate Backup Jobs** | Reduces human error with scripts & cron jobs. |

---
