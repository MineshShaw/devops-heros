# AWS DynamoDB & RDS - Database Services

AWS provides managed database services for different data models. **DynamoDB** is a serverless, distributed NoSQL database optimized for predictable low-latency access at scale. **Amazon RDS** manages relational database engines while preserving familiar SQL and database features.

---

## DynamoDB

### NoSQL

DynamoDB is a non-relational key-value and document database. It does not require a fixed relational schema or joins. Applications model tables around access patterns and retrieve items efficiently by key.

### Tables, items, and attributes

- A **table** is a collection of items.
- An **item** is a record identified by its primary key.
- An **attribute** is a typed value within an item, such as a string, number, list, or map.

Items in one table can have different non-key attributes. Keep item sizes and access patterns in mind because DynamoDB capacity and performance are tied to item reads and writes.

### Partition key and sort key

A table's primary key is either:

- A **simple primary key** containing only a partition key.
- A **composite primary key** containing a partition key and sort key.

The partition key determines the logical partition used to distribute data. It should have high cardinality and an even distribution to avoid hot partitions. The sort key orders related items within the same partition-key value and supports range queries, such as all events for a customer between two timestamps.

Design keys from the queries the application must perform. Avoid scans for routine requests; use `Query` with keys and indexes instead. Global secondary indexes and local secondary indexes support additional access patterns, with their own capacity and consistency considerations.

### Capacity and reliability

Use on-demand capacity for unpredictable workloads or provisioned capacity with auto scaling for stable, measurable traffic. DynamoDB supports eventually consistent reads and, where required, strongly consistent reads in supported contexts. Point-in-time recovery, encryption, backups, streams, TTL, and global tables address common operational needs.

### DynamoDB use cases

- High-scale user profiles, sessions, and preferences.
- Shopping carts, orders, and product catalogs.
- Event metadata, IoT records, and gaming state.
- Serverless applications needing low operational overhead.
- Applications requiring predictable single-digit-millisecond access at scale.

---

## Amazon RDS

### Relational database

Amazon Relational Database Service (RDS) is a managed service for relational databases. It handles common operational tasks such as provisioning, patching, backups, monitoring, and some maintenance while the application continues to use SQL, tables, relationships, transactions, and indexes.

RDS is not a completely serverless abstraction for every engine. You still choose capacity, storage, networking, parameter settings, maintenance windows, and availability options.

### Supported engines

RDS supports managed versions of:

- Amazon Aurora (compatible with MySQL and PostgreSQL)
- PostgreSQL
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server

Check current AWS engine-version and feature support before selecting an engine. Licensing, extensions, compatibility, and operational requirements can affect the choice.

### DB instances and storage

A DB instance is the compute environment for an RDS database. You choose an instance class, storage type and size, database engine, subnet group, parameter group, and security settings. Some deployments also use Aurora clusters with a writer and readers.

Place RDS in private subnets using a DB subnet group that spans Availability Zones. Use security groups to allow database ports only from approved application security groups.

### Security

Use encryption at rest with AWS KMS, TLS in transit, strong authentication, and credentials stored in AWS Secrets Manager rather than application source code. Apply least privilege to database users and IAM roles. Restrict network access with private subnets and security groups, and audit connections and administrative actions.

### Backups

RDS automated backups support point-in-time recovery within the configured retention period. Manual snapshots persist until deleted and can be copied or shared according to service and security rules. Test restores, define retention and deletion policies, and consider cross-Region copies for disaster recovery.

### Multi-AZ

Multi-AZ deployments maintain a synchronous standby in another Availability Zone for high availability. RDS can fail over to the standby during certain failures and maintenance events; the standby is normally not used for read traffic. A failover changes the endpoint target, so applications should reconnect safely.

Multi-AZ improves availability and durability but does not replace backups or application retry logic.

### Read replicas

Read replicas asynchronously replicate data from a source database and can serve read traffic. They help scale read-heavy workloads and can be promoted for some migration or recovery scenarios. Replication lag, read-after-write behavior, engine limitations, and additional cost must be considered.

Multi-AZ is primarily an availability feature; read replicas are primarily a read-scaling and replication feature. They can be used together.

### RDS use cases

- Transactional business applications requiring SQL and ACID transactions.
- Existing applications that use MySQL, PostgreSQL, SQL Server, Oracle, or MariaDB.
- Systems requiring joins, constraints, stored procedures, or relational reporting.
- Managed production databases where patching and backups should be simplified.
- Read-heavy applications using replicas and caching.

## Choosing between DynamoDB and RDS

| Requirement | Prefer DynamoDB | Prefer RDS |
| --- | --- | --- |
| Data model | Key-value or document | Relational tables and relationships |
| Access pattern | Known key-based queries at scale | Flexible SQL queries and joins |
| Operations | Serverless, highly managed scaling | Managed engine with more configuration |
| Transactions | Supported, modeled around items | Rich relational transactions |
| Existing application | NoSQL/serverless design | Existing SQL or relational application |

Choose based on access patterns and consistency requirements, not on the database label alone. DynamoDB does not remove data-modeling work, and RDS does not remove capacity, security, or backup responsibilities.

## Further reading

- [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)
- [Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/)
- [Amazon RDS features](https://aws.amazon.com/rds/features/)

