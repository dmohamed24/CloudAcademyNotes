
### Instance Stores
- **Definition**: Block storage is a data storage technology that splits data into equal-sized blocks and stores them on physical storage for quick access
- **EC2 Instances** provide access to CPU, memory, network, and block-level storage (similar to a hard drive in a personal computer).
- **Block-level storage** is used to store files in blocks, updating only the changed parts of the file, making it efficient for applications like databases and file systems.
- **Instance Store**:
    - Temporary block-level storage attached directly to the host computer running the EC2 instance.
    - **Lifespan**: Instance stores only last as long as the EC2 instance is running.
    - **Data Loss**: Data in the instance store is lost when the EC2 instance is stopped or terminated.
    - When restarted, EC2 instances might be hosted on a different physical machine, meaning the previous instance store is no longer accessible.
- **Best Use Cases**: Temporary data such as scratch files, temporary cache, or data that can be easily recreated without any impact.

#### Amazon Elastic Block Store (EBS)

- **Amazon EBS** provides **persistent block-level storage** for Amazon EC2 instances, unlike the temporary instance store.
- **EBS Volumes**:
    - Virtual hard drives that can be attached to EC2 instances.
    - Persist across EC2 instance stops, terminations, and restarts.
    - Not tied to the host where the EC2 instance runs, so data remains available after instance changes.
- **Backups with EBS Snapshots**:
    - **Incremental backups**: Only save data blocks that have changed since the last snapshot.
    - **First snapshot**: Full backup of all data.
    - Regular snapshots are crucial for data recovery in case of corruption.
- **Difference from full backups**: Full backups copy all data, even unchanged data, while incremental backups only store new or modified blocks of data.

### Object Storage 

- **Definition**: A data storage method that stores large amounts of unstructured data in a flat structure, with each piece of data called an object
- **Amazon S3** is a highly scalable **object storage** service that allows you to store and retrieve unlimited amounts of data.
- **Data stored as objects**:
    - Objects consist of **data**, **metadata**, and a **key**.
    - **Data**: The file being stored (e.g., images, videos, text files).
    - **Metadata**: Descriptive information about the data, such as its size and how it is used.
    - **Key**: A unique identifier for the object.
- **Buckets**:
    - Objects are stored in **buckets**, similar to how files are stored in directories.
    - Buckets help organize and manage the storage of objects in S3.
- Ideal for storing a wide variety of data types like receipts, images, spreadsheets, and videos.

#### Amazon Simple Storage Service (S3)

- **Amazon S3** provides **object-level storage** where data is stored as objects in **buckets**.
- **Key Features**:
    - Stores any type of file (e.g., images, videos, text files) with a maximum file size of **5 TB**.
    - Offers **unlimited storage space**.
- **Permissions & Versioning**:
    - **Permissions**: Control visibility and access to objects.
    - **Versioning**: Track changes to the object over time using S3 versioning, allowing you to retain previous versions of objects.
- **Storage Classes**:
    - Objects can be stored across different **storage classes** (tiers), which offer varying levels of access and cost.
- **Use Cases**: Ideal for backups, media files, archived documents, and more, with flexible access and security options.

#### Amazon S3 Storage Classes

- **Amazon S3** offers a range of storage classes based on data access frequency and availability requirements. Key considerations:
    - **How often** you need to retrieve data.
    - **How available** you need the data to be.

##### 1. **S3 Standard**
- Designed for **frequently accessed** data.
- Stores data in **at least 3 Availability Zones**.
- Provides **high availability** (99.999999999% durability).
- Use cases: websites, content distribution, data analytics, static website hosting.

##### 2. **S3 Standard-Infrequent Access (S3 Standard-IA)**
- Ideal for **infrequently accessed** data but needs **high availability**.
- Lower storage cost, higher retrieval cost than S3 Standard.
- Suitable for backups, disaster recovery files.

##### 3. **S3 One Zone-Infrequent Access (S3 One Zone-IA)**
- Stores data in a **single Availability Zone**.
- Lower storage cost than S3 Standard-IA, but less durability.
- Best for data that can be easily reproduced if an Availability Zone fails.

##### 4. **S3 Intelligent-Tiering**
- Designed for data with **changing access patterns**.
- Automatically moves objects between frequent and infrequent access tiers based on usage.
- Requires a small monthly fee for monitoring and automation.

##### 5. **S3 Glacier**
- **Low-cost storage** for **archiving**.
- Ideal for long-term storage, with **retrieval times** from a few minutes to hours.
- Suitable for archived data like customer records or old media files.
- Supports **vault lock policies** for compliance (e.g., WORM).

##### 6. **S3 Glacier Deep Archive**
- **Lowest-cost** storage for **long-term archiving**.
- Data retrieval takes up to **12 hours**.
- Best for data you rarely need to access but must store for long periods.

##### **S3 Lifecycle Policies**
- Automates data movement between storage classes (e.g., S3 Standard → S3-IA → S3 Glacier) based on predefined schedules, optimizing storage costs without manual intervention.


### Comparing Amazon EBS and Amazon S3

| **Feature**                  | **Amazon S3 (Object Storage)**                                                                   | **Amazon EBS (Block Storage)**                                              |
| ---------------------------- | ------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| **Storage Type**             | Object storage (stores files as complete objects)                                                | Block-level storage (files are broken into blocks)                          |
| **Use Case**                 | Static data (e.g., images, backups, documents)                                                   | Frequently updated data (e.g., video editing, databases)                    |
| **Data Updates**             | Re-upload entire file when changes are made                                                      | Updates only modified blocks, no need to re-upload whole file               |
| **Durability**               | 99.999999999% (11 nines) across multiple Availability Zones                                      | Durable within the same Availability Zone, manual backups needed            |
| **Access & Management**      | Web-enabled, each object has a URL for easy access                                               | Requires EC2 instances for use, not web-enabled                             |
| **Performance (Latency)**    | Higher latency compared to block storage                                                         | Lower latency, better for applications needing rapid data access            |
| **Performance (Throughput)** | Lower throughput than block storage                                                              | Higher throughput for data-intensive applications                           |
| **Cost Efficiency**          | Cost-effective for large-scale, static content storage                                           | More expensive but suited for high-change workloads                         |
| **Backup Strategy**          | Built-in durability and redundancy, no need for extra backups                                    | Manual backups required (e.g., using EBS snapshots)                         |
| **Serverless**               | Yes, does not require EC2 instances                                                              | No, requires EC2 instances for operation                                    |
| **Data Transfer Rates**      | Slower data transfer rates compared to block storage                                             | Faster data transfer rates, ideal for applications with frequent I/O        |
| **Best For**                 | Storing large volumes of unstructured data (e.g., content delivery networks, big data analytics) | Databases, virtual machines, or any application needing fast access to data |

#### Key Takeaways:
- **Amazon S3** is best for static, unstructured data with **infrequent changes**.
- **Amazon EBS** is optimal for **frequently updated files** requiring **low latency and high throughput**.
- Block storage is faster and better suited for applications needing rapid data access, while object storage is more cost-effective for large-scale, static content.



### File Storage Overview

- **Definition**: File storage is a data storage system that organizes data into files and folders. It's the most popular storage system and is used on most personal computers
- **File Storage** allows multiple clients (users, applications, servers) to access shared data in file folders through a file system.
    - Uses block storage on a local file system to organize files.
    -  File storage is often built on top of block storage. File storage uses a hierarchical architecture, and the data appears similarly to how it was written and read.
    - Clients access data via **file paths**.
- **Ideal Use Cases**:
    - Shared environments where multiple services/resources need to access the same data simultaneously.
    - Common for **shared file systems** in applications like analytics.

#### Amazon Elastic File System (Amazon EFS)
- **Scalable File System**: Automatically grows and shrinks as files are added or removed.
- **On-Demand Scaling**: Can scale to petabytes without disrupting running applications.
- **Multiple Access Points**: Allows multiple instances to access data at the same time, making it suitable for shared data environments.

#### Key Differences

- **Compared to Block Storage and Object Storage**:
    - File storage is ideal for scenarios where **simultaneous access by multiple clients** is needed.
    - Block storage is optimized for fast, frequent updates (databases, virtual machines).
    - Object storage is better suited for storing static data with fewer updates (media files, backups).


### Comparing Amazon EBS and Amazon EFS

| **Property**                 | **Amazon EBS (Block Storage)**                                                                 | **Amazon EFS (File Storage)**                                                                 |
| ---------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Storage Type**             | Block-level storage (files are broken into blocks)                                             | File storage (files are stored in a hierarchical file system)                                 |
| **Use Case**                 | High-performance access to frequently updated data (e.g., databases, virtual machines)         | Shared data access by multiple clients (e.g., shared analytics, file shares)                  |
| **Data Updates**             | Updates only modified blocks of data, avoiding re-upload of the whole file                     | Automatically manages data access and changes from multiple instances                         |
| **Durability**               | Data stored in a single Availability Zone (requires backup strategy)                           | Stores data across multiple Availability Zones, providing higher durability                   |
| **Access & Management**      | Requires an EC2 instance to use, data access is tied to the EC2 instance in the same AZ        | Can be accessed by multiple EC2 instances and on-premises servers using AWS Direct Connect    |
| **Performance (Latency)**    | Low latency (optimized for rapid data access)                                                  | Slightly higher latency than block storage but supports multiple clients simultaneously       |
| **Performance (Throughput)** | High throughput for block-level workloads (e.g., databases, read-write intensive applications) | Moderate throughput, designed for shared access use cases                                     |
| **Cost Efficiency**          | Fixed size, pay for provisioned capacity (e.g., 2TB volume must be resized manually)           | Pay only for the storage you use (scales automatically)                                       |
| **Backup Strategy**          | Requires manual backup with EBS snapshots                                                      | Redundant storage across Availability Zones, automatic scaling and redundancy                 |
| **Serverless**               | No, requires EC2 instances                                                                     | Yes, file storage is scalable and doesn't require manual scaling or EC2 instances for scaling |
| **Data Transfer Rates**      | Higher data transfer rates for block-level I/O-intensive applications                          | Lower transfer rates than EBS but optimized for shared access scenarios                       |
| **Best For**                 | Databases, high-performance applications, virtual machines                                     | Shared file systems, multiple users or services accessing the same data simultaneously        |
| **Availability**             | Availability Zone-level resource (EC2 and EBS must be in the same AZ)                          | Regional service (accessible across multiple Availability Zones)                              |
| **Scaling**                  | Manual (must provision new storage if capacity is reached)                                     | Automatic scaling up/down without disrupting applications                                     |
| **Concurrent Access**        | Designed for single-instance access per volume                                                 | Supports concurrent access from multiple EC2 instances and on-premise servers                 |
| **Example Use Cases**        | Running a database, virtual machines, applications that need block-level updates               | Shared file systems for applications, analytics, content management, and data sharing         |

#### Key Takeaways:

- **Amazon EBS** is ideal for workloads that require **frequent updates** and **low latency** access (databases, virtual machines).
- **Amazon EFS** is best for **shared file storage** where **multiple clients** need simultaneous access to the same data (shared analytics, file shares).



### Relational Databases

SQL (relational) database is a collection of data that has pre-defined relationships between them. The collection of data is organized in table of rows and columns. The column represents a field of information that all the entities posses while a row holds all the formation that belongs to a single entity. 

#### Amazon Relational Database Service (RDS)

- **Amazon RDS**: A managed service that simplifies running relational databases in the AWS Cloud by automating tasks such as **hardware provisioning, database setup, patching, and backups**.
- **Integration**: Can be integrated with other AWS services, such as AWS Lambda, to execute database queries from serverless applications.
- **Security**: Provides encryption options for data **at rest** (stored data) and **in transit** (data being transferred).

#### Amazon RDS Database Engines

Amazon RDS supports six database engines, offering optimized solutions for memory, performance, or input/output (I/O):

1. **Amazon Aurora**
2. **PostgreSQL**
3. **MySQL**
4. **MariaDB**
5. **Oracle Database**
6. **Microsoft SQL Server**

#### Key Benefits of Amazon RDS

- **Automated Maintenance**: Includes patching and backups, reducing the need for manual database maintenance.
- **Redundancy and Failover**: Provides built-in **redundancy, failover,** and **disaster recovery** features.
- **Operational Efficiency**: Allows more focus on business goals by reducing the burden of database administration.

#### Amazon Aurora

- **High Performance**: Up to **five times faster than standard MySQL** databases and **three times faster than standard PostgreSQL** databases.
- **Compatibility**: Compatible with MySQL and PostgreSQL.
- **Cost-Effectiveness**: Priced at **1/10th the cost of commercial-grade databases**.

#### Amazon Aurora Features

- **High Availability**: Replicates six copies of data across three Availability Zones and continuously backs up to Amazon S3, allowing for **point-in-time recovery**.
- **Scalability**: Supports up to 15 **read replicas** for offloading read operations and improving performance.

Consider Amazon Aurora for workloads that require high availability, scalability, and cost-effective database solutions.


### Nonrelational Databases

Non-SQL databases are referred to as non relational databases because the data does not have a pre-defined schema to match. 

Nonrelational databases, also known as **NoSQL databases**, offer a more flexible data structure than traditional relational databases. They are optimized for **high-performance** and **scalability**—essential for use cases where data types are less rigid and access rates are high.

- **Key-Value Pairs**: A common data structure for NoSQL databases, where each **key** has an associated **value**. Items in the database can vary in their attributes, meaning each entry doesn't need to follow a strict schema.

#### Example Structure of a Nonrelational Database

|Key|Value|
|---|---|
|1|Name: John Doe, Address: 123 Any Street, Favorite Drink: Medium latte|
|2|Name: Mary Major, Address: 100 Main Street, Birthday: July 5, 1994|

This flexibility enables dynamic data handling, allowing attributes to be added or removed as needed.

#### Amazon DynamoDB

Amazon DynamoDB is a **serverless, key-value NoSQL database** that provides **single-digit millisecond performance** at any scale.

#### Key Features of Amazon DynamoDB

- **Table-Based Structure**: Data is stored in tables, where each table holds items (keys) with attributes (values).
- **Automatic Scaling**: DynamoDB automatically scales up or down based on data volume, ensuring consistent performance even with millions of users.
- **Serverless Architecture**: Eliminates the need to manage servers, install software, or handle maintenance tasks.
- **High Performance and Low Latency**: Optimized for applications needing **millisecond response times** at any scale, making it ideal for high-demand use cases.
- **Redundant Storage**: Data is stored redundantly across multiple Availability Zones (AZs) and mirrored across drives, ensuring **high availability**.

DynamoDB is particularly suited for applications requiring **reliability, scalability, and rapid performance** across varying data structures, making it an optimal choice for use cases with potentially millions of users or dynamically changing data requirements.



### Amazon RDS VS Amazon DynamoDB

|**Feature**|**Amazon RDS**|**Amazon DynamoDB**|
|---|---|---|
|**Type**|Relational database|Non-relational, NoSQL (key-value store)|
|**Use Cases**|Complex data relationships, data analytics, business reporting, and relational joins|High-speed single-table lookups, applications with flexible schemas, simple and high-scale data storage|
|**Primary Data Structure**|Tables with rows and columns; supports complex schemas|Key-value pairs and attributes; supports flexible schemas|
|**Data Models Supported**|Relational (SQL-based)|Key-value and document model (NoSQL-based)|
|**Supported Database Engines**|MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Amazon Aurora|Single database engine provided by DynamoDB|
|**Performance**|Optimized for transactional integrity and data consistency; has more overhead|Designed for millisecond response times, optimized for rapid, scalable data access|
|**Scalability**|Manual vertical and horizontal scaling|Automatically scalable; DynamoDB adjusts storage and throughput based on demand|
|**Data Consistency**|Offers strong consistency with ACID transactions|Supports both eventually consistent and strongly consistent reads|
|**State**|Stateful (retains relationships and transactions across tables)|Stateless (ideal for independent records or items without relationships)|
|**Transaction Support**|Full ACID compliance for complex transactional needs|Supports transactions but limited compared to RDS, focused on high-speed operations|
|**Data Redundancy**|Supports Multi-AZ for high availability; replication depends on the chosen engine|Replicates data across multiple AZs for high availability|
|**Data Backup**|Automated backups, snapshots, point-in-time recovery|Automated backups with on-demand and continuous backup options|
|**Maintenance**|Managed by AWS, including patches, updates, and failover; some manual configuration may be needed|Fully managed and serverless, no need for user involvement in patching or updates|
|**Cost Efficiency**|Cost varies based on instance type, storage, and usage|Pay-as-you-go pricing based on data storage and read/write capacity units (no server cost)|
|**Typical Use Case Example**|Sales chain management system requiring multi-table analysis, relational joins, complex queries|Employee contact list or user profile database with simple schema, no relational requirements, requiring fast lookups|

---

**Summary:**

- **Amazon RDS** is ideal for use cases that require **complex relational joins and analytical processing**, benefiting from the power and structure of SQL.
- **Amazon DynamoDB** shines for **high-speed, single-table lookups** and applications where **flexibility and scalability** are more critical than relational complexity.


### Redshift

- **Purpose**: Amazon Redshift is a managed data warehousing service for big data analytics, designed to handle historical analysis rather than real-time data.
- **Use Cases**: Ideal for business intelligence (BI) tasks, analyzing historical data, identifying trends, and understanding relationships across large data sets.
- **Handling Big Data**: Traditional databases may struggle with historical data due to volume and variety. Redshift is optimized to scale efficiently, storing petabytes of data.
- **Data Consolidation**: Integrates data from various sources (inventory, financial, sales) to offer comprehensive analytics and reporting.
- **Performance**: Up to 10x faster than traditional databases for BI workloads. Redshift Spectrum allows queries on exabytes of unstructured data in data lakes.
- **Ease of Use**: Can be launched with a single API call, allowing data teams to focus on insights without managing complex infrastructure.


### AWS Database Migration Service (AWS DMS) Key Points

- **Purpose**: AWS DMS enables easy and secure migration of relational, nonrelational, and other data stores to AWS.
- **Migration Types**:
    - _Homogeneous Migration_: Source and target databases are the same type (e.g., MySQL to MySQL).
    - _Heterogeneous Migration_: Source and target databases are different types (e.g., MySQL to Amazon Aurora). This requires the AWS Schema Conversion Tool to convert schema and code.
- **Minimal Downtime**: Source database remains operational during migration, reducing downtime for applications.
- **Use Cases**:
    - **Development & Testing**: Migrate a copy of production data to test environments.
    - **Database Consolidation**: Combine multiple databases into a single central one.
    - **Continuous Replication**: Set up ongoing data copies for disaster recovery or geographic separation.

### Additional AWS Database Services Overview

- **Amazon DocumentDB**: A document database compatible with MongoDB. Ideal for managing content, catalogs, and user profiles.
    
- **Amazon Neptune**: A graph database service designed for applications requiring highly connected data, such as social networks, recommendation engines, fraud detection, and knowledge graphs.
    
- **Amazon Quantum Ledger Database (QLDB)**: An immutable ledger database that keeps a complete history of all data changes, useful for auditing and tracking data lineage.
    
- **Amazon Managed Blockchain**: Allows you to create and manage blockchain networks for distributed ledger applications, enabling secure, multi-party transactions without a central authority.
    
- **Amazon ElastiCache**: Adds caching to improve database read times, supporting both Redis and Memcached as in-memory data stores.
    
- **Amazon DynamoDB Accelerator (DAX)**: An in-memory cache specifically for DynamoDB, enhancing read response times from milliseconds to microseconds for nonrelational data.