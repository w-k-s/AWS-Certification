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