- [Azure Cosmos DB Documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/)
## Azure Cosmos DB

#### Key benefits of Azure Cosmos DB

**Global recplication** - Basically a automated/synchronous replication for your data
**Consistency levels** - 5 different consistency levels for different scenarios - each level dictates the way your data is replicated to Azure Cosmos DB. Making each level good for different application scenarios.
**Low latency** - <10 ms Read/Write with requests.
**Elastic scale-out** - Elastically scales out.


---

#### Cosmos DB Account features TODO



---
#### Authorization

** Built in CosmosDB RBAC Roles**

| Role | Description |
| ---- | ---- |
| **DocumentDB Account Contributor** | Can manage Azure Cosmos DB accounts. |
| **Cosmos DB Account Reader** | Can read Azure Cosmos DB account data. |
| **CosmosBackupOperator** | Can submit a restore request for a periodic backup-enabled database/container. Can modify backup interval/retention. Does not access data or use Data Explorer. |
| **CosmosRestoreOperator** | Can perform a restore action for an Azure Cosmos DB account with continuous backup mode. |
| **Cosmos DB Operator** | Can provision Azure Cosmos DB accounts, databases, and containers. Cannot access data or use Data Explorer. |

---

#### Azure Cosmos DB consistency levels
| Consistency Level | Read Behavior | Write Ordering |
| ---- | ---- | ---- |
| `Strong` | Always see the newest information. | Everything is in exact order. |
| `Bounded Staleness` | Might see information a tiny bit late, within a known wait time. | Mostly in order, with some room for slight delay. |
| `Session` (default) | See the newest information just for you. | Your own updates are in order. |
| `Consistent Prefix` | Won't see things out of order. | Things happen in a set sequence, but not necessarily up-to-date. |
| `Eventual` | Will see the newest information, but it might take some time. | No specific sequence, but it all works out eventually. |
`maxIntervalSeconds` and `maxStalenessPrefix` are two properties only configurable for the `Bounded Staleness` level.

---
#### Cosmos DB Supported APIs
|  |  |
| ---- | ---- |
| NoSQL | Document storage and full control over interface, service and SDK client libraries. |
| MongoDB | Documented data storage with BSON format. |
| Apache Cassandra | Stores data in column schema |
| Table | key/value format storage |
| Apache Gremin | Graph queries and data stored as ediges and vertices |
| PostgreSQL | Single-node storage or multi-node configuration |

---
#### Performance

#####  Cosmos DB query types
When you query a container in Cosmos, there are essentially three different types of query you will be doing.
1. The most optimized and common one is `in-partition query`: meaning, the query will have a equality expression `=` when performing the query, often with the partition key to match with. 
	*Example*
```sql
SELECT * FROM c WHERE c.OrderId = 'O_12343' and c.ProductName = 'Egg'
```
2. `Cross-partition query`: When you don't specify a partition key together with the query, this becomes a cross-partition query, meaning you will be performing one query per physical partition until you find your desired item.*Example*
```sql
SELECT * FROM c WHERE c.Location = 'Seattle`
```
3. ``Parallel cross-partition``: A new feature introduced with Cosmos DB SDKs => 1.9.0. This will allow you to perform cross-partition queries with better performance, by utilizing two settings
	- `MaxConcurrency`:Set the maximum number of simultaneous network connections to the container's partitions. If you set it to -1, SDK manages the parallelism for you, else you have to specify this yourself.
	- `MaxBufferedItemCount`: Trades query latency versus client-side memory utilization. If this option is omitted or to set to -1. 


#### Indexing policies

**Including/Excluding property paths**
By including/excluding paths you can gain reduce latency and RU charge of write operations. This is because you minimize the amount of paths that are indexed for each operation.

1. Any indexing policy has to include the root path `/*` as either an included or an excluded path.
2. If the indexing mode is set to **consistent**, the system properties `id` and `_ts` are automatically indexed.

*Example*
```json
{ "indexingMode": "consistent",
  "includedPaths": 
	 [ 
	 { "path": "/*" } 
	 ], 
	 "excludedPaths": [ 
		 { 
		 "path": "/path/to/single/excluded/property/?"
		 },
		 { 
		 "path": "/path/to/root/of/multiple/excluded/properties/*" 
		 } 
	 ] 
 }
```

##### Composite indexes
|**Composite Index**|**Sample `ORDER BY` Query**|**Supported by Composite Index?**|
|---|---|---|
|`(name ASC, age ASC)`|`SELECT * FROM c ORDER BY c.name ASC, c.age asc`|`Yes`|
|`(name ASC, age ASC)`|`SELECT * FROM c ORDER BY c.age ASC, c.name asc`|`No`|
|`(name ASC, age ASC)`|`SELECT * FROM c ORDER BY c.name DESC, c.age DESC`|`Yes`|
|`(name ASC, age ASC)`|`SELECT * FROM c ORDER BY c.name ASC, c.age DESC`|`No`|
|`(name ASC, age ASC, timestamp ASC)`|`SELECT * FROM c ORDER BY c.name ASC, c.age ASC, timestamp ASC`|`Yes`|
|`(name ASC, age ASC, timestamp ASC)`|`SELECT * FROM c ORDER BY c.name ASC, c.age ASC`|`No`|

---
#### Partitioning
Partitioning is important in Azure Cosmos DB because it allows you to scale your database horizontally. This means that you can distribute your data across multiple partitions, which can improve the performance of your database and make it more scalable. By partitioning your data, you can also control how data is distributed within your database and ensure that your application can access the data it needs quickly and efficiently.

The two components that make a partition key is the `key-path` which is the attribute to use for partitioning and the `key-value` is the actual value of that attribute. Example below.
`key-path` could be ==/orderId== here and then the `key-value` is the actual ==orderid==.
```json
{
  "orderId": "001",
  "customerId": "C123",
  "amount": 200,
  "status": "Shipped",
  ...
}
```
#### Difference between Logical/Physical partition keys
| Property | Logical Partition Key | Physical Partition Key |
| ---- | ---- | ---- |
| Definition | Property of the item chosen by the application developer | Property of the item used by Cosmos DB to determine the partition where the item is stored |
| Function | Used to distribute the items across partitions | Used by Cosmos DB to distribute the items across partitions |
| Example | `ITproducts` | `ComputerHardwareProducts` |
##### Best practices when creating partition keys

1. **Concatenate multiple properties of an item**
```json
{
"id": "productP43242_20230101",
"productId": "P43242",
"dateCreated":"20230101",
"partitionKey": "P43242_20230101"
}
```

2. U**se a partition key with a randum suffix**
```json
 {
  "id": "user56789_342",
  "partitionKey": "56789_342", 
  "UserId": "56789",
 
}
  
```

#### What is a Synthetic partition key?

It is a partition key that has more than one value to identifying it. It is used when you wanna combine essential information to partition key within CosmosDB.

```json
{
"carId": "09d72710-e343-4dd5-bc19-e456b31e5bd9",
"model-year": "2020",
"cars": [
	]
};
{
"carId": "09d72710-e343-4dd5-bc19-e456b31e5bd9",
"model-year": "2020",
"partitionKey": "09d72710-e343-4dd5-bc19-e456b31e5bd9_2020", (synthetic partition key)
"cars": [
	]
};
```


---
#### What are the advantage of having a storage account together with Cosmos DB?
It saves the amount of RequestUnits that are being used for data that can be stored in a container-blob, for exaple images, pdf files etc. It is cheaper in the long run to fetch them from a blob instead of storing in Cosmos DB.
#### Change feed feature
Azure Cosmos DB Change Feed is a feature that allows you to track changes to containers in an Azure Cosmos DB database. It provides a way to access and process the insert and update operations performed on containers in the database in real-time.
##### Changed feed processor 
##### Main-concepts 
1. **Monitored Container**: The data source. It's where your changes happen and get tracked.
2. **Lease Container**: The coordinator. It keeps track of who's reading what in the change feed and ensures no overlaps.
3. **Compute Instance** (Host): The worker. Hosts the change feed processor, listening for changes. Could be a VM, Kubernetes pod, or Azure App Service.
4. **Delegate**: The action taker. Your custom code that decides what to do when changes are detected.
[Change feed processor docs](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/change-feed-processor?tabs=dotnet)
##### Sub-concepts
`Error-Message queue (DLQ)`: To prevent the change feed processor from getting "stuck" and retrying the same batch over and over, implement logic to handle this scenarios by moving this batch to a error-message queue (i.ea DLQ).
`Change feed estimator`: monitor's the progress of your change feed processor instances as they read the change feed.
#### Azure Cosmos Code examples (.NET)

Create / Update
```csharp
// Get container
CosmosClient client = new CosmosClient(endpoint, key);
Container container = client.GetContainer(databaseName, collectionName);

// CreateItem
User user = new User {
id = Guid.NewGuid(),
name = "Peter",
role = "HR"
}

// 2 Options
await newUser = await container.CreateItemAsync(user); // creating new item
await newUser = await container.UpserItemAsync(user); // Create or update item
```

Read

```csharp
// Get container
CosmosClient client = new CosmosClient(endpoint, key);
Container container = client.GetContainer(databaseName, collectionName);

// CreateItem
string userId = '123123-123123123-123123123-123123123'
PartitionKey partitionKey = new PartitionKey("Users")

ItemsResponse<User> response = await container.ReadItemAsync<User>(userId,partitionKey);

//Serialize rsponse
User user = response.Resource;
```

Delete

```csharp
// Get container
public async Task DeleteDocumentAsync(string documentId) {
CosmosClient client = new CosmosClient(endpoint, key);
Container container = client.GetContainer(databaseName, collectionName);

// Execute
// Delete the document with the specified ID
await container.DeleteItemAsync<dynamic>(documentId, new PartitionKey("<partition-key-value>"));
}
```

Query

```csharp
// Get container
CosmosClient client = new CosmosClient(endpoint, key);
Container container = client.GetContainer(databaseName, collectionName);

// CreateItem
FeedIterator<User> iterator  = container.GetItemQueryIterator<User>(
"SELECT * FROM users u WHERE u.Role = 'HR'"
)

while(iterator.HasMoreResults)
{
FeedResponse<User> batch = await iterator.ReadNextAsync();
	foreach(User item in batch)
		{
	//do something
		}
}

```

#### Built in CosmosDB RBAC Roles

| Role                      | Description                                                                                                       |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **DocumentDB Account Contributor** | Can manage Azure Cosmos DB accounts.                                                                              |
| **Cosmos DB Account Reader**      | Can read Azure Cosmos DB account data.                                                                            |
| **CosmosBackupOperator**          | Can submit a restore request for a periodic backup-enabled database/container. Can modify backup interval/retention. Does not access data or use Data Explorer. |
| **CosmosRestoreOperator**         | Can perform a restore action for an Azure Cosmos DB account with continuous backup mode.                           |
| **Cosmos DB Operator**             | Can provision Azure Cosmos DB accounts, databases, and containers. Cannot access data or use Data Explorer.        |


---

