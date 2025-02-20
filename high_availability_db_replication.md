### **📌 Section 6: High Availability & Database Replication**  
#### **🔹 Lesson 1: Ensuring Database Availability & Replication**  
✅ **Topic 1: Introduction to High Availability**  
✅ **Topic 2: Types of Database Replication**  
✅ **Topic 3: Setting Up Replication in MySQL, PostgreSQL, and SQL Server**  
✅ **Topic 4: Failover & Disaster Recovery Strategies**  

---

## **🔹 1. Introduction to High Availability (HA)**
**High Availability (HA)** ensures that a database remains **accessible & operational** even if hardware/software failures occur.  
✅ **Benefits of HA:**  
- **Minimizes downtime** in case of failure  
- **Ensures data consistency** across servers  
- **Improves scalability** for high-traffic applications  

### **📍 Strategies for High Availability**
| **HA Strategy** | **Description** | **Use Case** |
|--------------|-----------------|-------------|
| **Replication** | Copies data from **Primary → Standby** | Prevents data loss & enables read scalability |
| **Clustering** | Multiple servers **work as one** | Used for distributed databases (e.g., MySQL Galera Cluster) |
| **Failover Mechanisms** | Switches to a backup database if primary fails | Automatic switchover for 24/7 uptime |
| **Load Balancing** | Spreads read/write queries across servers | Improves performance for high-traffic systems |

---

## **🔹 2. Types of Database Replication**
Replication keeps **copies of data synchronized** across multiple servers.  

| **Replication Type** | **Description** | **Use Case** |
|------------------|------------------|-------------|
| **Master-Slave (Primary-Replica)** | One **Primary** DB handles writes, multiple **Replicas** handle reads | Read scalability (e.g., analytics systems) |
| **Master-Master** | Multiple DBs handle **both reads & writes** | Multi-region applications |
| **Synchronous Replication** | Data is **immediately copied** to replicas | Strong consistency (e.g., banking systems) |
| **Asynchronous Replication** | Data is copied **with a delay** | High-speed replication with **some lag** |

---

## **🔹 3. Setting Up Replication in MySQL, PostgreSQL, and SQL Server**
Let's set up **Master-Slave Replication** in different databases.

### **📍 MySQL Replication (Master-Slave)**
#### **Step 1: Configure the Master**
1️⃣ Edit MySQL config file (`/etc/mysql/my.cnf`):
```ini
[mysqld]
server-id=1
log-bin=mysql-bin
binlog-do-db=mydatabase
```
2️⃣ Restart MySQL and create a **replication user**:
```sql
CREATE USER 'replica'@'%' IDENTIFIED BY 'replica_password';
GRANT REPLICATION SLAVE ON *.* TO 'replica'@'%';
```
3️⃣ Get the **current log position**:
```sql
SHOW MASTER STATUS;
```

#### **Step 2: Configure the Slave**
1️⃣ Edit MySQL config (`my.cnf`):
```ini
[mysqld]
server-id=2
relay-log=relay-bin
```
2️⃣ Connect the slave to the master:
```sql
CHANGE MASTER TO
MASTER_HOST='master_ip',
MASTER_USER='replica',
MASTER_PASSWORD='replica_password',
MASTER_LOG_FILE='mysql-bin.000001',
MASTER_LOG_POS=12345;
START SLAVE;
```
3️⃣ Verify replication:
```sql
SHOW SLAVE STATUS\G;
```
✅ **Now, the slave will receive real-time updates from the master!**

---

### **📍 PostgreSQL Streaming Replication**
#### **Step 1: Enable Replication on Master**
1️⃣ Edit `postgresql.conf`:
```ini
wal_level = replica
max_wal_senders = 5
hot_standby = on
```
2️⃣ Restart PostgreSQL and create a replication user:
```sql
CREATE ROLE replicator WITH REPLICATION LOGIN PASSWORD 'replica_password';
```

#### **Step 2: Set Up the Standby (Replica)**
1️⃣ Copy the master’s data directory:
```sh
pg_basebackup -h master_ip -D /var/lib/postgresql/12/main -U replicator -P
```
2️⃣ Create a `recovery.conf` file:
```ini
standby_mode = 'on'
primary_conninfo = 'host=master_ip user=replicator password=replica_password'
```
3️⃣ Start the replica:
```sh
pg_ctl start -D /var/lib/postgresql/12/main
```
✅ **The PostgreSQL replica now syncs with the master in real time!**

---

### **📍 SQL Server Replication (Transactional Replication)**
#### **Step 1: Configure the Publisher (Master)**
1️⃣ Enable Replication:
```sql
EXEC sp_replicationdboption @dbname = 'MyDatabase', @optname = 'publish', @value = 'true';
```
2️⃣ Create a **publication**:
```sql
EXEC sp_addpublication @publication = 'MyPublication', @status = 'active';
```

#### **Step 2: Configure the Subscriber (Replica)**
1️⃣ Add a **Subscription**:
```sql
EXEC sp_addsubscription @publication = 'MyPublication', @subscriber = 'ReplicaServer';
```
✅ **Now, SQL Server will continuously replicate transactions!**

---

## **🔹 4. Failover & Disaster Recovery**
When a **Primary Server fails**, automatic failover ensures continuity.

| **Failover Strategy** | **Description** |
|----------------|--------------|
| **Manual Failover** | DBA manually promotes a replica to primary. |
| **Automated Failover** | Tools like `Patroni` (PostgreSQL) or `MySQL Group Replication` detect failures and promote a new primary. |
| **Failover Clustering** | Uses multiple servers to take over in case of failure. |

### **📍 Example: Promoting a Replica to Master in PostgreSQL**
```sql
SELECT pg_promote();
```
✅ **The replica becomes the new master in case of failure.**  

---

## **🔹 5. Best Practices for High Availability**
| **Best Practice** | **Why It’s Important?** |
|------------------|----------------------|
| **Use Automated Failover** | Reduces downtime with instant switch-over. |
| **Monitor Replication Lag** | Ensures real-time updates across replicas. |
| **Geographically Distributed Replicas** | Protects against data center failures. |
| **Regularly Test Failover** | Ensures replication works under real failure scenarios. |
| **Use Load Balancing for Read Queries** | Distributes read queries across replicas to improve performance. |

---

### **📝 Summary**
| **Feature** | **Purpose** |
|------------|------------|
| **Master-Slave Replication** | Copies data from one primary DB to multiple read replicas. |
| **Master-Master Replication** | Allows multiple nodes to handle both read & write. |
| **Streaming Replication (PostgreSQL)** | Sends real-time updates to standby servers. |
| **Automatic Failover** | Detects failure and promotes a new primary. |
| **Load Balancing** | Distributes traffic across multiple replicas. |

---
