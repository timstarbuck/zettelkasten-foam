# AZ-204 Develop solutions that use Azure Cosmos DB

* Identify the key benefits provided by Azure Cosmos DB
* Describe the elements in an Azure Cosmos DB account and how they're organized
* Explain the different consistency levels and choose the correct one for your project
* Explore the APIs supported in Azure Cosmos DB and choose the appropriate API for your solution
* Describe how request units impact costs
* Create Azure Cosmos DB resources by using the Azure portal.

**Key benefits of global distribution**
With its novel multi-master replication protocol, every region supports both writes and reads. The multi-master capability also enables:
* Unlimited elastic write and read scalability.
* 99.999% read and write availability all around the world.
* Guaranteed reads and writes served in less than 10 milliseconds at the 99th percentile.

**Explore the resource hierarchy**
* Account - fundamental unit of global distribution and HA. Contains a unique DNS name. Azure regions can be added/removed to/from your account at any time. Soft limit of 50 account under a subscription (can be increased via support request).
* DB - Analogous to a namespace. 
* DB Container - fundamental unit of scalability (throughput measured in RU/s). 
  * Dedicated provisioned throughput model. Just for this container.
  * Shared provisioned throughput model. Shared with other container's in the DB.
  * A container is a schema-agnostic container of items. Items in a container can have arbitrary schemas. 
* Azure Cosmos DB items - Depending on which API you use, an Azure Cosmos DB item can represent either a document in a collection, a row in a table, or a node or edge in a graph.

**Explore consistency levels**

From strongest to weakest consistency:
* Strong
* Bounded staleness
* Session
* Consistent prefix
* Eventual

![Consistenty graphic](attachments/cosmos_consistency.png)

The default consistency level configured on your account applies to all Azure Cosmos DB databases and containers under that account. All reads and queries issued against a container or a database use the specified consistency level by default.

* Strong consistency
  * The reads are guaranteed to return the most recent committed version of an item. A client never sees an uncommitted or partial write. Users are always guaranteed to read the latest committed write.
* Bounded staleness consistency
  * The reads might lag behind writes by at most "K" versions (that is, "updates") of an item or by "T" time interval, whichever is reached first. In other words, when you choose bounded staleness, the "staleness" can be configured in two ways:
    * The number of versions (K) of the item
    * The time interval (T) reads might lag behind the writes
  * For a single region account, the minimum value of K and T is 10 write operations or 5 seconds. For multi-region accounts the minimum value of K and T is 100,000 write operations or 300 seconds.
* Session consistency
  * In session consistency, within a single client session reads are guaranteed to honor the consistent-prefix, monotonic reads, monotonic writes, read-your-writes, and write-follows-reads guarantees. This assumes a single "writer" session or sharing the session token for multiple writers.
* Consistent prefix consistency
  * Updates made as a batch within a transaction, are returned consistent to the transaction in which they were committed. Write operations within a transaction of multiple documents are always visible together.
  * Eventual consistency
    * In eventual consistency, there's no ordering guarantee for reads. In the absence of any further writes, the replicas eventually converge.

**Explore supported APIs**

Azure Cosmos DB offers multiple database APIs, which include:
* Azure Cosmos DB for NoSQL
  * API for NoSQL is native to Azure Cosmos DB.
  * The Azure Cosmos DB API for NoSQL stores data in document format.
  * New features are first available in API for NoSQL.
  * Should be the default choice unless using an existing application with another API.
* Azure Cosmos DB for MongoDB
  * The Azure Cosmos DB API for MongoDB stores data in a document structure, via BSON format. It's compatible with MongoDB wire protocol.
* Azure Cosmos DB for PostgreSQL
  * Azure Cosmos DB for PostgreSQL is a managed service for running PostgreSQL at any scale, with the Citus open source superpower of distributed tables.
* Azure Cosmos DB for Apache Cassandra
  * The Azure Cosmos DB API for Cassandra stores data in column-oriented schema. Apache Cassandra offers a highly distributed, horizontally scaling approach to storing large volumes of data while offering a flexible approach to a column-oriented schema. 
  * Wire protocol compatible with native Apaceh Cassandra
* Azure Cosmos DB for Table
  * The Azure Cosmos DB API for Table stores data in key/value format. If you're currently using Azure Table storage, you may see some limitations in latency, scaling, throughput, global distribution, index management, low query performance. API for Table overcomes these limitations and it's recommended to migrate your app if you want to use the benefits of Azure Cosmos DB. API for Table only supports OLTP scenarios.
* Azure Cosmos DB for Apache Gremlin
  * The Azure Cosmos DB API for Gremlin allows users to make graph queries and stores data as edges and vertices.

**Discover request units**

You pay for:
  * throughput you provisioned (per hour)
  * storage you consume (per hour)

The cost of all database operations is normalized by Azure Cosmos DB and is expressed by request units (or RUs, for short). A request unit represents the system resources such as CPU, IOPS, and memory that are required to perform the database operations supported by Azure Cosmos DB.

The cost to do a point read, which is fetching a single item by its ID and partition key value, for a ***1-KB item is 1RU***. All other database operations are similarly assigned a cost using RUs. No matter which API you use to interact with your Azure Cosmos container, costs are measured by RUs. Whether the database operation is a write, point read, or query, costs are measured in RUs.

The type of Azure Cosmos DB account you're using determines the way consumed RUs get charged. There are three modes in which you can create an account:
* **Provisioned throughput mode:** In this mode, you provision the number of RUs for your application on a per-second basis in increments of 100 RUs per second. To scale the provisioned throughput for your application, you can increase or decrease the number of RUs at any time in increments or decrements of 100 RUs. You can make your changes either programmatically or by using the Azure portal. You can provision throughput at container and database granularity level.
* **Serverless mode:** In this mode, you don't have to provision any throughput when creating resources in your Azure Cosmos DB account. At the end of your billing period, you get billed for the number of request units that have been consumed by your database operations.
* **Autoscale mode:** In this mode, you can automatically and instantly scale the throughput (RU/s) of your database or container based on its usage. This scaling operation doesn't affect the availability, latency, throughput, or performance of the workload. This mode is well suited for mission-critical workloads that have variable or unpredictable traffic patterns, and require SLAs on high performance and scale.

**Writing stored procedures**
Stored procedures can create, update, read, query, and delete items inside an Azure Cosmos container. Stored procedures are registered per collection, and can operate on any document or an attachment present in that collection.
Creating an item is an asynchronous operation and depends on the JavaScript callback functions. The callback function has two parameters:
* The error object in case the operation fails
* A return value

**Triggers**

Pretriggers
: Executed before modifying a database item. Cannot have any input parameters. Obtain the item being modified via context.getRequest().getBody(),

Post-triggers
: Executed after modifying a database item. Obtain the item being modified via context.getResponse().getBody().

Triggers aren't automatically executed, they must be specified for each database operation where you want them to execute. After you define a trigger, you should register it by using the Azure Cosmos DB SDKs.

Example calling a pretrigger:
```
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPreValidateToDoItemTimestamp";
await container.items.create({
    category: "Personal",
    name: "Groceries",
    description: "Pick up strawberries",
    isComplete: false
}, {preTriggerInclude: [triggerId]});
```

Example calling a post-trigger:
```
const item = {
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPostUpdateMetadata";
await container.items.create(item, {postTriggerInclude: [triggerId]});
```
When triggers are registered, you can specify the operations that it can run with. This trigger should be created with a TriggerOperation value of TriggerOperation.Create, which means using the trigger in a replace operation isn't permitted.

**User-defined functions**

Example creating a UDF
```
const container = client.database("myDatabase").container("myContainer");
const udfId = "Tax";
await container.userDefinedFunctions.create({
    id: udfId,
    body: require(`../js/${udfId}`)
})
```

Example calling a UDF
```
const container = client.database("myDatabase").container("myContainer");
const sql = "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000";
const {result} = await container.items.query(sql).toArray();
```


**Explore change feed in Azure Cosmos DB**
Change feed in Azure Cosmos DB is a persistent record of changes to a container in the order they occur. Change feed support in Azure Cosmos DB works by listening to an Azure Cosmos DB container for any changes. It then outputs the sorted list of documents that were changed in the order in which they were modified. The persisted changes can be processed asynchronously and incrementally, and the output can be distributed across one or more consumers for parallel processing.

Push Model
: Change feed processor pushed work to a client. Checking for work and storing state for last processed work is handled withing the change processor.

Pull Model
: Client pulls work from the server. Client has to store the state for the last processed work, handling load balancing across multiple clients and handling errors.

Azure Functions with Azure Cosmos DB trigger can be used to handle pushed changes. Under the covers this uses the Change feed processor library.

Change Feed Process library can be used with .NET and Java SDKs.

