- [Azure Storage Documentation](https://docs.microsoft.com/en-us/azure/storage/)
- [Azure Cosmos DB Documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/)

| Topic                               | Description                                                                                                                                     |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| **What is SAS? **                    | A signed URI that includes a token with query parameters to control how storage resources may be accessed.                                       |
| **Types of SAS**                    | 1. User delegation SAS (Blob only)<br>2. Service SAS<br>3. Account SAS                                                                          |
| **Security Recommendation**  🦺       | Use Azure AD credentials when possible. Prefer User Delegation SAS for Blob storage for better security.                                         |
| **Components of a SAS URI**         | - URI to the resource<br>- SAS token                                                                                                             |
| **Components of SAS Token**         | - `sp`: Access rights<br>- `st`: Start time<br>- `se`: End time<br>- `sv`: API version<br>- `sr`: Storage type<br>- `sig`: Signature             |
| **Best Practices**   💡               | - Use HTTPS<br>- Use User Delegation SAS<br>- Short expiration time<br>- Minimum-required privileges                                              |
| **When Not to Use SAS**  🚫           | Use a middle-tier service for managing users and access when there is an unacceptable risk of using a SAS.                                       |
#### Azure Storage types

| **Types**                           |                                                                                                                                                                                                     |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -----                               |                                                                                                                                                                                                     |
| 🔵 Blob (_binary large object_) storage | This type of storage is designed for storing large amounts of unstructured data, such as text or binary data, in the form of blobs (binary large objects).                                          |
| 📂 File storage                        | File storage is often used for sharing files between applications or for storing files that need to be accessed frequently.                                                                         |
| ⏩ Queue storage                       | This type of storage is designed for storing and processing large numbers of messages, and is often used for implementing message-oriented applications or for communication between microservices. |
|  Table storage                       | This type of storage is a NoSQL key-value store that is designed for storing large amounts of structured data.                                                                                      |
### Azure Storage account types

| Account Type      | Replication Options          | Storage Tiers Supported    | Description                                          |
|-------------------|------------------------------|----------------------------|------------------------------------------------------|
| `StorageV2`       | LRS, GRS, RA-GRS, ZRS        | Hot, Cool, Archive         | General-purpose v2, most versatile.                  |
| `StorageV1`       | LRS, GRS, RA-GRS             | Hot, Cool                  | General-purpose v1, legacy, fewer features.         |
| `BlobStorage`     | LRS, GRS, RA-GRS, ZRS        | Hot, Cool, Archive         | Optimized for blob storage with access to all tiers.|
| `FileStorage`     | LRS, ZRS                     | Premium                    | Optimized for file shares, premium performance.      |
| `BlockBlobStorage`| LRS, ZRS                     | Premium                    | For high transaction rates, big data, analytics.     |

##### Azure Storage Redundancy options

| Redundacy options                          | Features                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Locally-redundant storage (LRS) - Default  | Three replicas & one region. Write is acknowledged when all replicas are committed                                                                                                                                                                                                                                                                                                                                  |
| Zone-redundant storage (ZRS)               | Three replicas, three zones & one region. Synchronous writes to all three zones.                                                                                                                               |
| Geo-redundant storage (GRS)                | This option stores six copies of your data across two Azure regions, with three copies in the primary region and three copies in the secondary (or "failover") region. Protects against major regional disasters.|
| Read-access geo-redundant storage (RA-GRS) | Recovery point object (RPO). This option is similar to GRS, but it also allows you to read data from the secondary region in the event of an outage, without having to manually fail over.                                                                                                                                                                                                |
#### Data management features for a storage account

==**Static Website**==
Allows you to serve static content (HTML,CSS,JS & Image files) directly from a container called `$web`, which is a optimal approach where you don't need a website to render the content. If you need to configure headers for the content, you have to use Azure-CDN, is it is not supported with only using the static website.
### Blob Storage

#### User-delegation SAS 
| RBAC Role                     | Description                                                                                                                                                    | Scope                  | Key Permissions                                                                                       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|-------------------------------------------------------------------------------------------------------|
| Contributor                   | Full access to manage all resources, but does not allow delegation of access.                                                                                   | Subscription, Resource | All, including `generateUserDelegationKey`                                                             |
| Storage Account Contributor   | Allows managing storage accounts, but not access to data within.                                                                                                | Subscription, Resource | `generateUserDelegationKey`                                                                           |
| Storage Blob Data Contributor | Allows read/write/delete operations on blob data. Does not allow generating User Delegation keys.                                                               | Container, Blob        | `read`, `write`, `delete`                                                                              |
| Storage Blob Data Owner       | Provides full access to blob data, including setting ownership and delegating access.                                                                           | Container, Blob        | `read`, `write`, `delete`, `setOwner`, `setPermissions`                                                |
| Storage Blob Data Reader      | Allows read-only access to blob data. Does not allow generating User Delegation keys.                                                                           | Container, Blob        | `read`                                                                                                 |
| Storage Blob Delegator        | Allows generating User Delegation SAS tokens. Usually combined with other roles for data access.                                                                | Subscription, Resource | `generateUserDelegationKey`                                                                            |


| Role                          | Description                                                                 | Scope               | Permissions                           | Access Level            |
|-------------------------------|-----------------------------------------------------------------------------|---------------------|---------------------------------------|-------------------------|
| Contributor                   | Full access to manage all resources, but does not allow delegation of access| Subscription, Resource | All, including `generateUserDelegationKey` | Full Access             |
| Storage Blob Data Owner       | Provides full access to blob data, including setting ownership and delegating access. | Container, Blob | `read`, `write`, `delete`, `setOwner`, `setPermissions` | Full Access             |
| Storage Account Contributor   | Allows managing storage accounts, but not access to data within.            | Subscription, Resource | `generateUserDelegationKey`           | Full Access (Restricted) |
| Storage Blob Delegator        | Allows generating User Delegation SAS tokens. Usually combined with other roles for data access. | Subscription, Resource | `generateUserDelegationKey`           | Full Access (Restricted) |
| Storage Blob Data Contributor | Allows read/write/delete operations on blob data. Does not allow generating User Delegation keys. | Container, Blob | `read`, `write`, `delete`             | Least Privileged        |
| Storage Blob Data Reader      | Allows read-only access to blob data. Does not allow generating User Delegation keys. | Container, Blob | `read`                                | Least Privileged        |
#### Blob Storage Tiers

| Hot 🔥 | Higher overall cost because you can have fast and easy access to the data. Good for data that you use frequently. |                                                                                                               
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Cool 🧊 | Storage recommended for 30 days storing and supports all redundancy configurations.                                                            | 
| Archive 🧓    |  Lowest storage costs but can take several hours to process and access this data.                                                                                                                                              |                                                                                                                      |

#### Rehydration💧
1. **Rehydration Time**: Rehydrating data from the archive tier to an online tier (hot, cool, or cold) in Azure Blob Storage can take up to 15 hours. This duration is influenced by the priority you specify for the rehydration operation.
2. **Priorities for Rehydration**:
- **Standard Priority**: This is the default option for rehydration and may take up to 15 hours, depending on various factors like blob size and system load.
 - **High Priority**: This option is faster but more costly. It's recommended for urgent data restoration needs. Even with high priority, the rehydration time may exceed one hour.

**Life-cycle management policy for blob storage tiers** :t_rex:
```bash
# Define the lifecycle management policy in a JSON file (policy.json)
{
  "rules": [
    {
      # Rule name
      "name": "MoveToCoolStorageAfter30Days",
      # Enable this rule
      "enabled": true,
      # Rule type is 'Lifecycle'
      "type": "Lifecycle",
      "definition": {
        "actions": {
          # Apply action to base blob type
          "baseBlob": {
            # Move to 'Cool' storage tier after 30 days
            "tierToCool": {
              "daysAfterModificationGreaterThan": 30
            }
          }
        },
        # Apply this rule to 'blockBlob' types only
        "filters": {
          "blobTypes": ["blockBlob"]
        }
      }
    }
  ]
  
}
# Apply the policy to the storage account using the az storage account management-policy create command
az storage account management-policy create --g --account-name "StorageAccountName" --policy @"policy.json"
```

##### Rehydrating blob data from archive tier

**There are two options for rehydrating a blob from the archive tier**
*online tier = hot or cold*
1. You can copy the archived blob to an online tier 
2. You can change the blob's access tier to an online tier

| Class               | Description                                            |
| ------------------- | ------------------------------------------------------ |
| BlobClient          | Used to maniuplate Azure Storage Blobs                 |
| BlobClientOptions   | Configure options for connecting to Azure Blob Storage |
| BlobContainerClient | Manipulate the containers and its blobs within the Azure storage                                                       |
| BlobServiceClient   | Manipulate Azure Storage service resources and blob containers.                                                      |
| BlobUriBuilder      | Provides ways to modify contents of a Uri instance to point to a different Azure Storage resources (account,container or a)                                                       |
##### Manage properties and metadata

*See last modified time/date*
```csharp
BlobContainerClient containerClient = new BlobContainerClient(connectionString, "yourcontainer");
await containerClient.GetPropertiesAsync().ConfigureAwait(false);

Console.WriteLine($"Last Modified: {properties.LastModified}");
```

*Sets the metadata for the fetched blob*
```csharp
BlobClient container = new BlobContainerClient(connectionString, "yourcontainer").GetBlobClient("yourblob");

var properties = await container.GetPropertiesAsync();
// Create metadata dictionary
IDictionary<string, string> metadata = new Dictionary<string, string>
{
    { "category", "text files" },
    { "author", "john" }
};

await blobClient.SetMetadataAsync(metadata).ConfigureAwait(false);
```
*Use-case Metadata can be useful for filtering blobs, say in a content management system where you want to list all blobs by an author or category.*


*Fetch and set meta data for a blob*
```csharp
BlobContainerClient container = new BlobContainerClient(connectionString, "yourcontainer")
BlobClient blobClient = container.GetBlobClient("yourblob");

// Fetch existing metadata 

BlobProperties properties = await blobClient.GetPropertiesAsync(); 
IDictionary<string, string> metadata = properties.Metadata;

// Set new metadata 
metadata["newKey"] = "newValue"; await blobClient.SetMetadataAsync(metadata);

```

---
### Azure Storage Queue

**Storage queue functionality**
`maxDequeueCount` defaults to a retry-count of 5.
`timeout` can max be 10 mins for consumption plan.
`cold-start` is not supported for consumption plan, meaning there can be several minutes delay for the function to execute if it has been idle.




