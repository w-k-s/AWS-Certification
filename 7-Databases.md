# 7. Databases

## 7.1 Database Types

**OLTP (Online Transaction Processing)**: OLTP Databases process data transactions (INSERT, UPDATE, DELETE of data). Their effectiveness is measured by transactions/second.

**OLAP (Online Analytics Processing)**: OLTP Databases are used for datawarehousing. They are able to perform complex queries and aggregate results. Their effectiveness is measure with response time.

- Amazon offers RDS (Relational Database Service) which is of the OLTP kind. Available databases are:

```
**SQL Databases (OLTP)**: 


- SQL Server
- MySQL
- Postgres
- Oracle
- Aurora
- MariaDB


**NoSQL Databases (OLTP)**


- DynamoDB
```

- **Red Shift** is used for Database warehousing. It is an OLAP database.

- **Elasticache** is an in-memory datastore that can be used to cache the results of common queries. It supports `memcache` and `redis`.

## 7.2 Security Groups

- An EC2 instance in one security group can not connect to a database in anothed security group.
- To fix this issue, the inbound rules of the EC2 instance should allow the database port (e.g. 3306 for MySQl or 5321 for Postgres) for the security group of the RDS instance.

## 7.3 Backups

There are two kind of backups

1. Automated Backups
2. Database Snapshots

### 7.3.1 Automated Backups

- Recover data to any point in time within retention period
- Retention period 1 - 35 days
- Backs up data and transaction logs
- Backups are stored on S3 at no additional expense. In other words, if your database is 10GB in size, the 10GB backup on S3 will not be included in the cost of S3.
- Backup is done during a configurable backup window. I/O is suspensded when data is being backed up
- If you delete RDS instance, backups will be deleted

### 7.3.2 Database Snapshots

- Snapshots captured manually by the user
- They will not be deleted automatically when the Database instance is deleted

### 7.3.3 Restoring backups

- When restoring a backu (snapshot or automatic backup), a new RDS instance will be created

### 7.4 Encryption


- Uses SSE-KMS
- Encrypts stored data, automated backups, snapshots read-replicas.
- Existing databases can **NOT** be encrypted. As with EC2, a snapshot must be taken, copy the snapshot and encrypt the codpy.

## 7.4 Multi-AZ & Read Replicas

### 7.4.1 Multi-AZ

- **Multi-AZ is for disaster recovery**
- When Multi-AZ is enabled, a second copy of the database is maintained in another Availability Zone (within the same region)
- If the first instance fails or goes into maintenance, the Database DNS is updated to point to the second instance. 
- Synchronization of the two databases is handled by AWS **synchronously**
- Supported for SQL databases only

### 7.4.3 Read Replicas

- **Read Replicas is for performance improvement**
- Most database operations are for reading
- To allow the master database to focus on CREATE, UPDATE and DELETE; it's useful to create replicas of the master database that are read-only.
- At most 5 read-replicas are allowed, but a replica can have read-replicas.
- Read-replicas can be in a different region than the master database
- Read-replicas can be deployed over Multi-AZ.
- Automated backups must be enabled to set up read replicas.
- Read replicas are synchronized with the master database **asynchronously**
- Availble for SQL databases (except SQL server and Oracle)

--- 

## 7.5 DynamoDB

- Very low latency
- NoSQL document-based storage
- Stored on SSD storage
- Spread across 3 gographically distinct data centres

### 7.5.1 Consistency Models

**Eventually Consistent Read (Default)**

- Data will eventually be propogated to the 3 geographically distinct data centres
- Takes upto a second

**Strongly Consistent Reads**

- Data is immediately consistent between 3 geographically distinct data centres

### 7.5.2 Pricing

- Cheap for reads
- Expensive for writes

---

## 7.6 RedShift

- Data warehousing solution
- Data warehousing use a different kind of architecture from a database perspective and an infrastructure perpsective

### 7.6.2 Configuration

Two kinds:

**Single Node**

- Stores 160GB of data


** Multi-node**

- Leader Node: Manages connections, receives queries
- Compute Node: Stores data, execute queries

### 7.6.1 Column Data Storage

- Data is stored in columns because data warehousing solutions perform a lot of aggregations. 
- Storing in columns requires fewer I/Os because data that needs to be aggregated is stored contiguously. This improves performance

### 7.6.2 Advanced Compression

- Columns stores data of the same type and it only works with a single column and not adjacent columns. This means it can compress the data in the column a lot better
- RedShift does not need IDs, it only needs data to calculate with; this saves on storage of indexes.
- RedShift automatically decides the best compression strategy for your data.

### 7.6.3 Pricing

- Compute Node hours
- Backup
- Data transferred

### 7.6.4 Security

- HTTPS In transit security
- AWS - SSE KMS
- AES256 encryption


---

## 7.7 ElastiCache

- Webservice for deploying an in-memory cache
- Caching stores results of I/O intensive queries, or the results of computationally intensive calculations
- Improves the latency of read heavy applications

### 7.7.1 Engines


**Memcached**

- Object Caching system

**Redis**

- Key value storage
- Value can be single items, list or sets

---

## 7.8 Aurora

- MySQL compatible 
- RDbMS
- 5X performance MySQL

### 7.8.1 Scaling

- Starts with 10GB storage. Scales in 10GB increments upto 64TB
- Scaling incurs downtime

- 2 copies stored in 3 availability zones (6 copies of your data)
- Self healing: scans data blocks for errors and corrects them
- 2 copies of data can be lost without affecting write availability. 3 copies of data can be lost without affecting read availability

### 7.8.2 Replicas

- 15 Aurora replicas
- 5 MySQL Read replicas

----

**Quiz**

1. What AWS DB platform is most suitable for OLTP?

- [ ] A. Elasticache
- [ ] B. RDS/DynamoDB
- [ ] C. Redshift

2. When replicating data from your primary RDS instance to your secondary RDS instance, what is the charge?

- [ ] A. Double the standard data transfer charge
- [ ] B. Same as the standard data transfer charge
- [ ] C. No Charge, Its free
- [ ] D. Half of the standard data transfer charge

3. What AWS service is best suited for non relational databases?

- [ ] A. RDS
- [ ] B. Redshift
- [ ] C. DynamoDB
- [ ] D. Elasticache

4. When you add a rule to an RDS security group you do not need to specify a port number or protocol?

- [ ] A. True
- [ ] B. False

5. If you are using Amazon RDS Provisioned IOPS storage with MySQL and Oracle database engines what is the maximum size RDS volume you can have by default?

- [ ] A. 500GB
- [ ] B. 6TB
- [ ] C. 3TB
- [ ] D. 16TB
- [ ] E. 5TB

6. What happens to the I/O operations while you take a database snapshot

- [ ] A. I/O operations to the database are suspended for the duration of the snapshot
- [ ] B. Nothing
- [ ] C. I/O operations to the database are sent to a Replica (if available) for the duration of the snapshot
- [ ] D. I/O operations will be functioning nomally

7. What AWS service is best used for Business Intelligence Tools/Data Warehousing?

- [ ] A. RDS
- [ ] B. Redshift
- [ ] C. DynamoDB
- [ ] D. Elasticache

8. In RDS when using multiple availability zones, can you use the secondary database as an independent read node?

- [ ] A. No
- [ ] B. Only in us-west-1
- [ ] C. Depends on how you set it up
- [ ] D. Yes

9. Amazon's Elasticache uses which two engines?


- [ ] A. Redis & Memory
- [ ] B. Reddit & Memcrush
- [ ] C. Redis & Memcached
- [ ] D. MyISAM & InnoDB

10. By default, the maximum provisioned IOPS capacity on an Oracle and MySQL RDS instance (using provisioned IOPS) is 30,000 IOPS.


- [ ] A. True
- [ ] B. False

**Answers**

1. B. RDS and DynamoDB are OLTP Databases. Elasticache is for in-memory caching. RedShift is for OLAP.
2. D. There is no additional charge for any kind of replica
3. C. DynamoDB is a noSql data store
4. B. Port number and protocol need to be specifed and you must allow traffic from the security group of the EC2 instance that wants to access the database
5. D
6. A. For the sake of consistency, I/O Operations are paused during snapshot. They are not sent to a replica because it would lead to complicated to consistency scenarios
7. B. 
8. A. This is not possible
9. C
10. False. Privisoins change over time