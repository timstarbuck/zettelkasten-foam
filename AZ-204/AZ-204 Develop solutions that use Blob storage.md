# AZ-204 Develop solutions that use Blob storage

[Learn path](https://learn.microsoft.com/en-us/training/paths/develop-solutions-that-use-blob-storage/){:target="_blank"}


### Explore Azure Blob Storage
* Identify the different types of storage accounts and the resource hierarchy for blob storage.
* Explain how data is securely stored.
* Enable a storage account for static website hosting.

Blob storage is designed for:

- Serving images or documents directly to a browser.
- Storing files for distributed access.
- Streaming video and audio.
- Writing to log files.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.

An Azure Storage account is the top-level container for all of your Azure Blob storage. The storage account provides a unique namespace for your Azure Storage data that is accessible from anywhere in the world over HTTP or HTTPS.

Types or Storage accounts
: * **Standard**: This is the standard general-purpose v2 account and is recommended for most scenarios using Azure Storage.
  * **Premium**: Premium accounts offer higher performance by using solid-state drives. If you create a premium account you can choose between three account types, block blobs, page blobs, or file shares.

Access tiers
: * The Hot access tier, which is optimized for frequent access of objects in the storage account. The Hot tier has the highest storage costs, but the lowest access costs. New storage accounts are created in the hot tier by default.
  * The Cool access tier, which is optimized for storing large amounts of data that is infrequently accessed and stored for at least 30 days. The Cool tier has lower storage costs and higher access costs compared to the Hot tier.
  * The Archive tier, which is available only for individual block blobs. The archive tier is optimized for data that can tolerate several hours of retrieval latency and will remain in the Archive tier for at least 180 days. The archive tier is the most cost-effective option for storing data, but accessing that data is more expensive than accessing data in the hot or cool tiers.

**Hierarchy**
Storage Account - unique namespace for you data. i.e. `http://mystorageaccount.blob.core.windows.net`
Container - Organizes a set of blobls. Similar to a directory in a file system. Storage account can incluide an unlimited # of containers and a container can store an unlimited # of blobs. Must be a valid DNS name. i.e. `https://myaccount.blob.core.windows.net/mycontainer`
  * Container names can be between 3 and 63 characters long.
  * Container names must start with a letter or number, and can contain only lowercase letters, numbers, and the dash (-) character.
  * Two or more consecutive dash characters aren't permitted in container names.
Blobs - 
  * Block blobs store text and binary data. Block blobs are made up of blocks of data that can be managed individually. Block blobs can store up to about 190.7 TiB.
  * Append blobs are made up of blocks like block blobs, but are optimized for append operations. Append blobs are ideal for scenarios such as logging data from virtual machines.
  * Page blobs store random access files up to 8 TB in size. Page blobs store virtual hard drive (VHD) files and serve as disks for Azure virtual machines.
  * example URIs
    * `https://myaccount.blob.core.windows.net/mycontainer/myblob`
    * `https://myaccount.blob.core.windows.net/mycontainer/myvirtualdirectory/myblob`

**Security Features**
* All data (including metadata) written to Azure Storage is automatically encrypted using Storage Service Encryption (SSE).
  * Data in Azure Storage is encrypted and decrypted transparently using 256-bit AES encryption, one of the strongest block ciphers available, and is FIPS 140-2 compliant. Azure Storage encryption is similar to BitLocker encryption on Windows.
* Microsoft Entra ID and Role-Based Access Control (RBAC) are supported for Azure Storage for both resource management operations and data operations, as follows:
* You can assign RBAC roles scoped to the storage account to security principals and use Microsoft Entra ID to authorize resource management operations such as key management.
* Microsoft Entra integration is supported for blob and queue data operations. You can assign RBAC roles scoped to a subscription, resource group, storage account, or an individual container or queue to a security principal or a managed identity for Azure resources.
* Data can be secured in transit between an application and Azure by using Client-Side Encryption, HTTPS, or SMB 3.0.
* OS and data disks used by Azure virtual machines can be encrypted using Azure Disk Encryption.
* Delegated access to the data objects in Azure Storage can be granted using a shared access signature.

**Static Website hosting in Azure Storage**

You can serve static content (HTML, CSS, JavaScript, and image files) directly from a storage container named $web. Hosting your content in Azure Storage enables you to use serverless architectures that include Azure Functions and other Platform as a service (PaaS) services. Azure Storage static website hosting is a great option in cases where you don't require a web server to render content.

There's no way to configure headers as part of the static website feature itself. Also, AuthN and AuthZ aren't supported. If these features are important for your scenario, consider using Azure Static Web Apps

It's easier to enable HTTP access for your custom domain, because Azure Storage natively supports it. To enable HTTPS, you have to use Azure CDN because Azure Storage doesn't yet natively support HTTPS with custom domains. Visit Map a custom domain to an Azure Blob Storage endpoint for step-by-step guidance.


### Manage the Azure Blob storage lifecycle

* Describe how each of the access tiers is optimized.
* Create and implement a lifecycle policy.
* Rehydrate blob data stored in an archive tier.

**Access Tiers**
* Hot - Optimized for storing data that is accessed frequently.
* Cool - Optimized for storing data that is infrequently accessed and stored for a minimum of 30 days.
* Cold tier - Optimized for storing data that is infrequently accessed and stored for a minimum of 90 days. The cold tier has lower storage costs and higher access costs compared to the cool tier.
* Archive - Optimized for storing data that is rarely accessed and stored for at least 180 days with flexible latency requirements, on the order of hours.

**Manage the data lifecycle**
* Transition blobs to a cooler storage tier (hot to cool, hot to archive, or cool to archive) to optimize for performance and cost
* Delete blobs at the end of their lifecycles
* Define rules to be run once per day at the storage account level
* Apply rules to containers or a subset of blobs (using prefixes as filters)

*Data stored in a premium block blob storage account cannot be tiered to Hot, Cool, or Archive using Set Blob Tier or using Azure Blob Storage lifecycle management. To move data, you must synchronously copy blobs from the block blob storage account to the Hot tier in a different account using the Put Block From URL API or a version of AzCopy that supports this API. The Put Block From URL API synchronously copies data on the server, meaning the call completes only once all the data is moved from the original server location to the destination location.*

**Lifecycle Rules**

Each rule definition includes a filter set and an action set. The filter set limits rule actions to a certain set of objects within a container or objects names. The action set applies the tier or delete actions to the filtered set of objects.

The following sample rule filters the account to run the actions on objects that exist inside container1 and start with foo.

* Tier blob to cool tier 30 days after last modification
* Tier blob to archive tier 90 days after last modification
* Delete blob 2,555 days (seven years) after last modification
* Delete blob snapshots 90 days after snapshot creation

```
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```
**Rule Filters**
| Filter name	|Type	|Is Required|
| -- | -- | -- |
|blobTypes|An array of predefined enum values.|Yes|
|prefixMatch|An array of strings for prefixes to be match. Each rule can define up to 10 prefixes. A prefix string must start with a container name.|No|
|blobIndexMatch|An array of dictionary values consisting of blob index tag key and value conditions to be matched. Each rule can define up to 10 blob index tag condition.|No|

**Rule Actions**

| Action |	Base Blob	| Snapshot | Version |
| -- | -- | -- | -- |
| tierToCool	| Supported for blockBlob	| Supported	| Supported |
| enableAutoTierToHotFromCool |Supported for blockBlob	| Not supported	| Not supported | 
| tierToArchive |Supported for blockBlob	| Supported	| Supported |
| delete	| Supported for blockBlob and appendBlob | Supported | Supported |

Run Conditions are based on age.

| Action run condition| Condition value	| Description |
| -- | -- | --|
|daysAfterModificationGreaterThan	| Integer value indicating the age in days | The condition for base blob actions|
|daysAfterCreationGreaterThan	| Integer value indicating the age in days	| The condition for blob snapshot actions |
|daysAfterLastAccessTimeGreaterThan	| Integer value indicating the age in days	| The condition for a current version of a blob when access tracking is enabled |
|daysAfterLastTierChangeGreaterThan	| Integer value indicating the age in days after last blob tier change time	| This condition applies only to tierToArchive actions and can be used only with the daysAfterModificationGreaterThan condition. |

**Rehydrate blob data from the archive tier**
While a blob is in the archive access tier, it's considered to be offline and can't be read or modified. In order to read or modify data in an archived blob, you must first rehydrate the blob to an online tier, either the hot or cool tier. There are two options for rehydrating a blob that is stored in the archive tier:

* Copy an archived blob to an online tier: You can rehydrate an archived blob by copying it to a new blob in the hot or cool tier with the Copy Blob or Copy Blob from URL operation. *Microsoft recommends this option for most scenarios.*

* Change a blob's access tier to an online tier: You can rehydrate an archived blob to hot or cool by changing its tier using the Set Blob Tier operation.

**Rehydration Priority**
When you rehydrate a blob, you can set the priority for the rehydration operation via the optional `x-ms-rehydrate-priority`` header on a *Set Blob Tier* or *Copy Blob/Copy Blob From URL* operation. Rehydration priority options include:
* Standard priority: The rehydration request is processed in the order it was received and might take up to 15 hours.
* High priority: The rehydration request is prioritized over standard priority requests and might complete in under one hour for objects under 10 GB in size.

**Copy an archived blob to an online tier**
|	| Hot tier source	| Cool tier source	| Archive tier source |
| --| -- | -- | -- |
|Hot tier destination	| Supported	| Supported	| Supported within the same account. Requires blob rehydration.|
|Cool tier destination | Supported | Supported	| Supported within the same account. Requires blob rehydration. |
| Archive tier destination	| Supported	| Supported	| Unsupported|

**Change a blob's access tier to an online tier**
The second option for rehydrating a blob from the archive tier to an online tier is to change the blob's tier by calling Set Blob Tier. With this operation, you can change the tier of the archived blob to either hot or cool.

Caution
: *Changing a blob's tier doesn't affect its last modified time. If there is a lifecycle management policy in effect for the storage account, then rehydrating a blob with Set Blob Tier can result in a scenario where the lifecycle policy moves the blob back to the archive tier after rehydration because the last modified time is beyond the threshold set for the policy.*

### Work with Azure Blob storage

**Azure Blob storage client library**
The Azure Storage client libraries for .NET offer a convenient interface for making calls to Azure Storage. The latest version of the Azure Storage client library is version 12.x. Microsoft recommends using version 12.x for new applications.

|Class | Description|
|--|--|
|BlobServiceClient	| Represents the storage account, and provides operations to retrieve and configure account properties, and to work with blob containers in the storage account.|
|BlobContainerClient	|Represents a specific blob container, and provides operations to work with the container and the blobs within.|
|BlobClient	|Represents a specific blob, and provides general operations to work with the blob, including operations to upload, download, delete, and create snapshots.|
|AppendBlobClient	|Represents an append blob, and provides operations specific to append blobs, such as appending log data.|
|BlockBlobClient	|Represents a block blob, and provides operations specific to block blobs, such as staging and then committing blocks of data.|

Blob containers support system properties and user-defined metadata, in addition to the data they contain.

***System properties***: System properties exist on each Blob storage resource. Some of them can be read or set, while others are read-only. Under the covers, some system properties correspond to certain standard HTTP headers. The Azure Storage client library for .NET maintains these properties for you.

***User-defined metadata***: User-defined metadata consists of one or more name-value pairs that you specify for a Blob storage resource. You can use metadata to store additional values with the resource. Metadata values are for your own purposes only, and do not affect how the resource behaves.

Metadata name/value pairs are valid HTTP headers, and so should adhere to all restrictions governing HTTP headers. Metadata names must be valid HTTP header names and valid C# identifiers, may contain only ASCII characters, and should be treated as case-insensitive. Metadata values containing non-ASCII characters should be Base64-encoded or URL-encoded.

`x-ms-meta-name:string-value`

To retrieve:
The URI syntax for retrieving metadata headers on a container is as follows:
`GET/HEAD https://myaccount.blob.core.windows.net/mycontainer?restype=container`
The URI syntax for retrieving metadata headers on a blob is as follows:
`GET/HEAD https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata`

To Set 
The URI syntax for retrieving metadata headers on a container is as follows:
`PUT https://myaccount.blob.core.windows.net/mycontainer?restype=container`
The URI syntax for retrieving metadata headers on a blob is as follows:
`PUT https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata`

