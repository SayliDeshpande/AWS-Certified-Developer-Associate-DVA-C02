# AWS-Certified-Developer-Associate-DVA-C02
## Technologies and concepts that might appear on the exam
Analytics:
* [Amazon Athena](#athena)
* Amazon Kinesis
* Amazon OpenSearch Service

Application Integration:
*	AWS AppSync
*	Amazon EventBridge
*	Amazon Simple Notification Service (Amazon SNS)
*	Amazon Simple Queue Service (Amazon SQS)
*	AWS Step Functions
  
Compute:
*	Amazon EC2
*	AWS Elastic Beanstalk
*	AWS Lambda
*	AWS Serverless Application Model (AWS SAM)
  
Containers:
*	AWS Copilot
*	Amazon Elastic Container Registry (Amazon ECR)
*	Amazon Elastic Container Service (Amazon ECS)
*	Amazon Elastic Kubernetes Service (Amazon EKS)

Database:
*	Amazon Aurora
*	[Amazon DynamoDB](#dynamodb)
*	Amazon ElastiCache
*	Amazon MemoryDB for Redis
*	Amazon RDS

Developer Tools:
*	AWS Amplify
*	AWS Cloud9
*	AWS CloudShell
*	AWS CodeArtifact
*	AWS CodeBuild
*	AWS CodeCommit
*	AWS CodeDeploy
*	Amazon CodeGuru
*	AWS CodePipeline
*	AWS CodeStar
*	Amazon CodeWhisperer
*	AWS X-Ray

Management and Governance:
*	AWS AppConfig
*	AWS CLI
*	AWS Cloud Development Kit (AWS CDK)
*	AWS CloudFormation
*	AWS CloudTrail
*	Amazon CloudWatch
*	Amazon CloudWatch Logs
*	AWS Systems Manager

Networking and Content Delivery:
*	Amazon API Gateway
*	Amazon CloudFront
*	Elastic Load Balancing (ELB)
*	Amazon Route 53
*	Amazon VPC

Security, Identity, and Compliance:
*	AWS Certificate Manager (ACM)
*	Amazon Cognito
*	AWS Identity and Access Management (IAM)
*	AWS Key Management Service (AWS KMS)
*	AWS Private Certificate Authority
*	AWS Secrets Manager
*	AWS Security Token Service (AWS STS)
*	AWS WAF

Storage:
*	Amazon Elastic Block Store (Amazon EBS)
*	Amazon Elastic File System (Amazon EFS)
*	Amazon S3
*	Amazon S3 Glacier


 ## Amazon Athena <a id="athena"></a>
 ## Amazon DynamoDb <a id="dynamodb"></a>
 - Amazon DynamoDB is a fully managed NoSQL database service and is schemaless.
 - other than the primary key attributes, you don't have to define any attributes or data types when you create tables.
 - Scales to massive workloads, distributed database. (Scales horizontaly)
 - In DynamoDB, tables, items, and attributes are the core components.
 - A table is a collection of items, and each item is a collection of attributes.
 - DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more querying flexibility. 
 - DynamoDB is made of Tables
 - Each table has a Primary Key (must be decided at creation time)
 - Each table can have an infinite number of items (= rows)
 - Each item has attributes (can be added over time – can be null)
 - Maximum size of an item is 400KB
 - Supported data types :
   - Scalar types : String, Number, Binary, Boolean, Null
   - Document type : List, Map
   - Set types : String Set, Number Set, Binary Set
 - DynamoDB – Primary Keys
   - Two types of primary keys :
      1. Partition Key (HASH attribute) :
         
       - composed of one attribute
       - must be unique for each item
       - DynamoDB uses the partition key's value as input to an internal hash function.
       - The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored.
         
      2. Partition Key + Sort Key (HASH attribute + RANGE attribute)
         
       - composed of two attributes (partition key and sort key)
       - the combination must be unique for each item.
       - DynamoDB uses the partition key value as input to an internal hash function.
       - The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored.
       - All items with the same partition key value are stored together, in sorted order by sort key value.
       - In a table that has a partition key and a sort key, it's possible for multiple items to have the same partition key value. However, those items must have different sort key values.
- DynamoDB – Secondary indexes :
  - Secondary indexes are alternate keys in addition to queries against primary keys.
  - Not mandatory, but give applications more flexibility when querying your data.
  - Every index belongs to a table, which is called the base table for the index.
  - DynamoDB maintains indexes automatically. When you add, update, or delete an item in the base table, DynamoDB adds, updates, or deletes the corresponding item in any indexes that belong to that table.
  - When you create an index, you specify which attributes will be copied, or projected, from the base table to the index.
  - At a minimum, DynamoDB projects the key attributes from the base table into the index.
  - two kinds of indexes :
    1. Local secondary index – An index that has the same partition key as the base table, but a different sort key.
    2. Global secondary index – An index with a partition key and sort key that can be different from those on the base table.
![image](https://github.com/SayliDeshpande/AWS-Certified-Developer-Associate-DVA-C02/assets/44116052/e0830d00-c805-4ad3-b90f-79eba9b4de42)
- DynamoDB Time to Live (TTL) :
  - Time To Live (TTL) for DynamoDB is a cost-effective method for deleting items that are no longer relevant.
  - Automatically delete items after an expiry timestamp.
  - Once deleted, items go into DynamoDB Streams as service deletions instead of user deletes.
  - TTL deletions can be identified in DynamoDB Streams, but only in the Region where the deletion occurred. 
  - Doesn’t consume any WCUs (i.e., no extra cost)
  - The TTL attribute must be a “Number” data type with “Unix Epoch timestamp” value. (expiration time must be in epoch format, in seconds).
  - If you use any other format, the TTL processes ignore the item.
  - The item will be expired after the specified time.
  - Items that are deleted by the Time to Live process after expiration have the following fields:

          Records[<index>].userIdentity.type "Service"
          Records[<index>].userIdentity.principalId "dynamodb.amazonaws.com"
    
  - Expired items deleted within 48 hours of expiration
  - Expired items, that haven’t been deleted, appears in reads/queries/scans (if you don’t want them, filter them out).These items still count towards storage and read costs until they are deleted.
  - Expired items are deleted from both LSIs and GSIs.
  - A delete operation for each expired item enters the DynamoDB Streams (can help recover expire items).
  - TTl can be enabled in the Amazon DynamoDB Console, the AWS Command Line Interface (AWS CLI), or using the Amazon DynamoDB API Reference with any of the supposed AWS SDKs.
  - It takes approximately one hour for TTL to be enabled across all partitions.
  - Use cases: reduce stored data by keeping only current items, adhere to regulatory obligations etc.
    
- DynamoDB Streams :
  - Dynamo DB captures time-ordered stream of item-level modifications (create/update/delete) in a table. (optional feature - we can enable/disable this feature).
  - DynamoDB Streams operates asynchronously, so there is no performance impact on a table if you enable a stream.
  - Each event is represented by a stream record.
  - A stream record contains : item-level modifications (create/update/delete) in a table, along with table name, event timestamp, other metadata.
  - Each stream record appears exactly once in the stream.
  - For each item that is modified in a DynamoDB table, the stream records appear in the same sequence as the actual modifications to the item.
  - Stream records have a lifetime of 24 hours; after that, they are automatically removed from the stream.
  - DynamoDB Streams writes stream records in near-real time so that you can build applications that consume these streams and take action based on the contents.
  - AWS maintains separate endpoints for DynamoDB and DynamoDB Streams.
  - To read and process DynamoDB Streams records, your application must access a DynamoDB Streams endpoint in the same Region.
  - The naming convention for DynamoDB Streams endpoints is streams.dynamodb.<region>.amazonaws.com. e.g. dynamodb.us-west-2.amazonaws.com
  - The AWS SDKs provide separate clients for DynamoDB and DynamoDB Streams.
  - We can enable/disable DynamoDb streams any time using AWS management console, AWS CLI or one of the AWS SDKs.
  - Following information need to choose that will be written to the streams :
      * KEYS_ONLY — Only the key attributes of the modified item.
      * NEW_IMAGE — The entire item, as it appears after it was modified.
      * OLD_IMAGE — The entire item, as it appeared before it was modified.
      * NEW_AND_OLD_IMAGES — Both the new and the old images of the item.
  - If we try to enable a stream on a table that already has a stream we get ResourceInUseException.
  - If we try to disable a stream on a table that doesn't have a stream ew get ValidationException.
  - DynamoDB Streams are made of shards, just like Kinesis Data Streams
  - You don’t provision shards, this is automated by AWS.Shards are ephemeral: They are created and deleted automatically, as needed.
  - Any shard can also split into multiple new shards; this also occurs automatically.
  - No more than two processes at most should be reading from the same stream's shard at the same time. Having more than two readers per shard can result in throttling.
  - Use cases:
    - react to changes in real-time (welcome email to users)
    - Analytics
    - Insert into derivative tables
    - Insert into OpenSearch Service
    - Implement cross-region replication

- DynamoDB API
  - To work with Amazon DynamoDB, your application must use a few simple API operations.
  - Control plane :
    * CreateTable – Creates a new table. Optionally, you can create one or more secondary indexes, and enable DynamoDB Streams for the table.
    * DescribeTable– Returns information about a table, such as its primary key schema, throughput settings, and index information.
    * ListTables – Returns the names of all of your tables in a list.
    * UpdateTable – Modifies the settings of a table or its indexes, creates or removes new indexes on a table, or modifies DynamoDB Streams settings for a table.
    * DeleteTable – Removes a table and all of its dependent objects from DynamoDB.
  - Data Plane :
    * PutItem – Writes a single item to a table. You must specify the primary key attributes, but you don't have to specify other attributes.
    * BatchWriteItem – Writes up to 25 items to a table. This is more efficient than calling PutItem multiple times because your application only needs a single network round trip to write the items.
    * GetItem – Retrieves a single item from a table. You must specify the primary key for the item that you want. You can retrieve the entire item, or just a subset of its attributes.
    * BatchGetItem – Retrieves up to 100 items from one or more tables, more efficient than calling GetItem multiple times because your application only needs a single network round trip to read the items.
    * Query – Retrieves all items that have a specific partition key
    * Scan – Retrieves all items in the specified table or index. You can retrieve entire items, or just a subset of their attributes.can apply a filtering condition
    * UpdateItem – Modifies one or more attributes in an item. You must specify the primary key for the item that you want to modify.
    * DeleteItem – Deletes a single item from a table. You must specify the primary key for the item that you want to delete.
    * BatchWriteItem – Deletes up to 25 items from one or more tables, more efficient than calling DeleteItem multiple times because application only needs a single network round trip to delete the items.

    PartiQL - A SQL-compatible query language
    * ExecuteStatement – Reads multiple items from a table. You can also write or update a single item from a table.
    * BatchExecuteStatement – Writes, updates or reads multiple items from a table, more efficient than ExecuteStatement because application only needs a single network round trip to write/read  items.
  - DynamoDb Stream :
    * ListStreams – Returns a list of all your streams, or just the stream for a specific table.
    * DescribeStream – Returns information about a stream, such as its Amazon Resource Name (ARN) and where your application can begin reading the first few stream records.
    * GetShardIterator – Returns a shard iterator, which is a data structure that your application uses to retrieve the records from the stream.
    * GetRecords – Retrieves one or more stream records, using a given shard iterator.
  - Transactions :
    * ExecuteTransaction – A batch operation that allows CRUD operations to multiple items both within and across tables with a guaranteed all-or-nothing result.  (PartiQL)
    * TransactWriteItems – A batch operation that allows Put, Update, and Delete operations to multiple items both within and across tables with a guaranteed all-or-nothing result.
    * TransactGetItems – A batch operation that allows Get operations to retrieve multiple items from one or more tables.
   
- Read consistency
  * Amazon DynamoDB reads data from tables, local secondary indexes (LSIs), global secondary indexes (GSIs), and streams.
  * There are two read consistency options:
    1. eventually consistent (default)
       * supported on tables, LSIs, GSIs and streams
       * may not reflect the results of a recently completed write operation
       * If you repeat your read request after a short time, the response should eventually return the more recent item.
       * Eventually consistent reads are half the cost of strongly consistent reads.
    
    2. Strongly consistent reads
       * only supported on tables and local secondary indexes
       * If we read just after a write, we will get the correct data
       * Set “ConsistentRead” parameter to True in API calls (GetItem, BatchGetItem, Query, Scan)
       * Consumes twice the RCU
      
    3. Global tables read consistency
       * A global table is composed of multiple replica tables in different AWS Regions.
       * Any change made to any item in any replica table is replicated to all the other replicas within the same global table, typically within a second, and are eventually consistent.

- Read/write capacity mode
  - The read/write capacity mode controls how you are charged for read and write throughput and how you manage capacity.
  - Secondary indexes inherit the read/write capacity mode from the base table.
  - There are two modes :
    1. Provisioned (default, free-tier eligible)
       * you specify the number of reads and writes per second
       * You need to plan capacity beforehand
       * Pay for provisioned read & write capacity units
       * Good option when :
         - You have predictable application traffic.
         - traffic is consistent or ramps gradually.
         - You can forecast capacity requirements to control costs.

    3. On Demand
       * Read/writes automatically scale up/down with your workloads.
       * automatically adapt to your application’s traffic volume.
       * accommodates double than previous traffic peak on table,
       * e.g 50,000 reads per second in the previous traffic peak, on-demand capacity mode instantly accommodates traffic of up to 100,000 reads per second.
       * However, throttling can occur if you exceed double your previous peak within 30 minutes.
       * No capacity planning needed (WCU / RCU)
       * Unlimited WCU & RCU, no throttle, more expensive
       * You’re charged for reads/writes that you use in terms of RRU and WRU
       * Read Request Units (RRU) – throughput for reads (same as RCU)
       * Write Request Units (WRU) – throughput for writes (same as WCU)
       * 2.5x more expensive than provisioned capacity (use with care)
       * Use cases: unknown workloads, unpredictable application traffic
 
      ![image](https://github.com/SayliDeshpande/AWS-Certified-Developer-Associate-DVA-C02/assets/44116052/fb51e25b-ffee-47bd-b12f-f6fad456916b)
      ![image](https://github.com/SayliDeshpande/AWS-Certified-Developer-Associate-DVA-C02/assets/44116052/353fcf01-0d6f-46d8-a7c0-9e9d5598825f)

      ![image](https://github.com/SayliDeshpande/AWS-Certified-Developer-Associate-DVA-C02/assets/44116052/9fb60ae9-412b-4841-a4a2-aa00dc935ada)
      ![image](https://github.com/SayliDeshpande/AWS-Certified-Developer-Associate-DVA-C02/assets/44116052/33df9fff-f7f9-4209-acbe-8924cff50edd)

    - DynamoDB – Throttling
      * If we exceed provisioned RCUs or WCUs, we get HTTP 400 code (Bad Request) and “ProvisionedThroughputExceededException”.
      * Reasons:
          * Hot Keys – one partition key is being read too many times (e.g., popular item)
          * Hot Partitions
          * Very large items, remember RCU and WCU depends on size of items
      * Solutions:
          * Exponential backoff when exception is encountered (already in SDK)
          * Distribute partition keys as much as possible
          * If RCU issue, we can use DynamoDB Accelerator (DAX)
       
    - Partitions and data distribution
      * Amazon DynamoDB stores data in partitions.
      * Partition management occurs automatically in the background- you never have to manage partitions yourself.
      * While creating table (status = CREATING), DynamoDb allocates partitions based on provisioned throughput (RCUs and WCUs).
      * We can begin reading and writing data when table is created. (status = ACTIVE)
      * Amazon DynamoDB allocates additional partitions to a table :
        - If you increase the table's provisioned throughput
        - If an existing partition fills to capacity and more storage space is required.
      * Global secondary indexes in DynamoDB are also composed of partitions.
      * The data in a global secondary index is stored separately from the data in its base table, but index partitions behave in much the same way as table partitions.

