# Fundamentals Certifications - Describe Azure architecture and services

[Learn Path](https://learn.microsoft.com/en-us/training/paths/azure-fundamentals-describe-azure-architecture-services/)

**Describe Azure regions, region pairs, and sovereign regions**
> Region Pairs: Most Azure regions are paired with another region within the same geography (such as US, Europe, or Asia) at least 300 miles away.
> 
> Sovereign regions: Sovereign regions are instances of Azure that are isolated from the main instance of Azure. US DoD Central, US Gov Virginia, US Gov Iowa and more. China East, China North, and more.


Describe Availability Zones.
> Availability zones are physically separate datacenters within an Azure region. Each availability zone is made up of one or more datacenters equipped with independent power, cooling, and networking. An availability zone is set up to be an isolation boundary. If one zone goes down, the other continues working. Availability zones are connected through high-speed, private fiber-optic networks.
> 
> Azure can help make your app highly available through availability zones.
> 
Describe Azure datacenters.
Describe Azure resources and Resource Groups.
> Resources are anything you create, provision or deploy. i.e. VM, App Service, Virtual Network, DB, etc.
> Resource Groups are a convenient way to group resources. i.e. a Dev resource group can be deleted and all resources will be deleted. Access can be assigned at a Resource Group level and apply to all resources within.
> 
Describe subscriptions.
> Subscriptions are a unit of management, billing, and scale. Subscriptions can be used to seperate Environments, Org structures or Billing.
> 
Describe management groups.
> Azure management groups provide a level of scope above subscriptions. You organize subscriptions into containers called management groups and apply governance conditions to the management groups. All subscriptions within a management group automatically inherit the conditions applied to the management group, the same way that resource groups inherit settings from subscriptions and resources inherit from resource groups.
> 
> Some examples of how you could use management groups might be:
>Create a hierarchy that applies a policy. You could limit VM locations to the US West Region in a group called Production. This policy will inherit onto all the subscriptions that are descendants of that management group and will apply to all VMs under those subscriptions. This security policy can't be altered by the resource or subscription owner, which allows for improved governance.
> Provide user access to multiple subscriptions. By moving multiple subscriptions under a management group, you can create one Azure role-based access control (Azure RBAC) assignment on the management group. Assigning Azure RBAC at the management group level means that all sub-management groups, subscriptions, resource groups, and resources underneath that management group would also inherit those permissions. One assignment on the management group can enable users to have access to everything they need instead of scripting Azure RBAC over different subscriptions.
>
>Important facts about management groups:
> 1) 10,000 management groups can be supported in a single directory.
> 2) A management group tree can support up to six levels of depth. This limit doesn't include the root level or the subscription level.
> 3) Each management group and subscription can support only one parent.



**Describe Azure storage services**

Compare Azure storage services
> * Azure Blobs: A massively scalable object store for text and binary data. Also includes support for big data analytics through Data Lake Storage Gen2.
> Blob storage is ideal for:
>   * Serving images or documents directly to a browser.
>   * Storing files for distributed access.
>   * Streaming video and audio.
>   * Storing data for backup and restore, disaster recovery, and archiving.
>   * Storing data for analysis by an on-premises or Azure-hosted service.
> Blob storage tiers:
>   * Hot access tier: Optimized for storing data that is accessed frequently (for example, images for your website).
>   * Cool access tier: Optimized for data that is infrequently accessed and stored for at least 30 days (for example, invoices for your customers).
>   * Cold access tier: Optimized for storing data that is infrequently accessed and stored for at least 90 days.
>   * Archive access tier: Appropriate for data that is rarely accessed and stored for at least 180 days, with flexible latency requirements (for example, long-term backups).
> * Azure Files: Managed file shares for cloud or on-premises deployments.
> Azure files Key Benefits
>   * Shared access: Azure file shares support the industry standard SMB and NFS protocols, meaning you can seamlessly replace your on-premises file shares with Azure file shares without worrying about application compatibility.
>   * Fully managed: Azure file shares can be created without the need to manage hardware or an OS. This means you don't have to deal with patching the server OS with critical security upgrades or replacing faulty hard disks.
>   * Scripting and tooling: PowerShell cmdlets and Azure CLI can be used to create, mount, and manage Azure file shares as part of the administration of Azure applications. You can create and manage Azure file shares using Azure portal and Azure Storage Explorer.
>   * Resiliency: Azure Files has been built from the ground up to always be available. Replacing on-premises file shares with Azure Files means you don't have to wake up in the middle of the night to deal with local power outages or network issues.
>   * Familiar programmability: Applications running in Azure can access data in the share via file system I/O APIs. Developers can therefore use their existing code and skills to migrate existing applications. In addition to System IO APIs, you can use Azure Storage Client Libraries or the Azure Storage REST API.
> * Azure Queues: A messaging store for reliable messaging between application components.
>   * Azure Queue storage is a service for storing large numbers of messages. Once stored, you can access the messages from anywhere in the world via authenticated calls using HTTP or HTTPS. A queue can contain as many messages as your storage account has room for (potentially millions). Each individual message can be up to 64 KB in size. Queues are commonly used to create a backlog of work to process asynchronously.
>   * Queue storage can be combined with compute functions like Azure Functions to take an action when a message is received. For example, you want to perform an action after a customer uploads a form to your website. You could have the submit button on the website trigger a message to the Queue storage. Then, you could use Azure Functions to trigger an action once the message was received.
> * Azure Disks: Block-level storage volumes for Azure VMs.
>   * Azure Disk storage, or Azure managed disks, are block-level storage volumes managed by Azure for use with Azure VMs. Conceptually, they’re the same as a physical disk, but they’re virtualized – offering greater resiliency and availability than a physical disk. With managed disks, all you have to do is provision the disk, and Azure will take care of the rest.
> * Azure Tables: NoSQL table option for structured, non-relational data.
>   * Azure Table storage stores large amounts of structured data. Azure tables are a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud. This enables you to use Azure tables to build your hybrid or multi-cloud solution and have your data always available. Azure tables are ideal for storing structured, non-relational data.

| Storage service |	Endpoint |
| ---------------- | ------------------------|
| Blob Storage	| https://<storage-account-name>.blob.core.windows.net |
| Data Lake Storage Gen2 | https://<storage-account-name>.dfs.core.windows.net |
| Azure Files | https://<storage-account-name>.file.core.windows.net |
| Queue Storage | https://<storage-account-name>.queue.core.windows.net |
| Table Storage | https://<storage-account-name>.table.core.windows.net |

Describe storage tiers
> Data in an Azure Storage account is always replicated three times in the primary region
> 
> 
Describe redundancy options
> Locally redundant storage (LRS)
> Replicates your data three times within a single data center in the primary region. LRS provides at least 11 nines of durability (99.999999999%) of objects over a given year. If a disaster such as fire or flooding occurs within the data center, all replicas of a storage account using LRS may be lost or unrecoverable. 
> 
> Zone-redundant storage (ZRS)
> For Availability Zone-enabled Regions, zone-redundant storage (ZRS) replicates your Azure Storage data synchronously across three Azure availability zones in the primary region. ZRS offers durability for Azure Storage data objects of at least 12 nines (99.9999999999%) over a given year.
> Microsoft recommends using ZRS in the primary region for scenarios that require high availability
> 
> Geo-redundant storage (GRS)
> GRS copies your data synchronously three times within a single physical location in the primary region using LRS. It then copies your data asynchronously to a single physical location in the secondary region (the region pair) using LRS. GRS offers durability for Azure Storage data objects of at least 16 nines (99.99999999999999%) over a given year.
>
> Read-access geo-redundant storage (RA-GRS)
> By default the secondary region in GRS is not available for read access. However, it can be enabled to allow RA-GRS.
> 
> Geo-zone-redundant storage (GZRS)
> GZRS combines the high availability provided by redundancy across availability zones with protection from regional outages provided by geo-replication. Data in a GZRS storage account is copied across three Azure availability zones in the primary region (similar to ZRS) and is also replicated to a secondary geographic region, using LRS, for protection from regional disasters. Microsoft recommends using GZRS for applications requiring maximum consistency, durability, and availability, excellent performance, and resilience for disaster recovery.
> 
> Read-access geo-zone-redundant storage (RA-GZRS)
> > By default the secondary region in GZRS is not available for read access. However, it can be enabled to allow RA-GZRS.



Identify options for moving files, including AzCopy, Azure Storage Explorer, and Azure File Sync

Azure Migrate
: Azure Migrate is a service that helps you migrate from an on-premises environment to the cloud. Azure Migrate functions as a hub to help you manage the assessment and migration of your on-premises datacenter to Azure.

Azure Data Box
: Azure Data Box is a physical migration service that helps transfer large amounts of data in a quick, inexpensive, and reliable way. The secure data transfer is accelerated by shipping you a proprietary Data Box storage device that has a maximum usable storage capacity of 80 terabytes.
: Data Box is ideally suited to transfer data sizes larger than 40 TBs in scenarios with no to limited network connectivity. The data movement can be one-time, periodic, or an initial bulk data transfer followed by periodic transfers.

AzCopy
: AzCopy is a command-line utility that you can use to copy blobs or files to or from your storage account. With AzCopy, you can upload files, download files, copy files between storage accounts, and even synchronize files. AzCopy can even be configured to work with other cloud providers to help move files back and forth between clouds.

Azure Storage Explorer
: Azure Storage Explorer is a standalone app that provides a graphical interface to manage files and blobs in your Azure Storage Account. It works on Windows, macOS, and Linux operating systems and uses AzCopy on the backend to perform all of the file and blob management tasks. With Storage Explorer, you can upload to Azure, download from Azure, or move between storage accounts.

Azure File Sync
: Azure File Sync is a tool that lets you centralize your file shares in Azure Files and keep the flexibility, performance, and compatibility of a Windows file server. It’s almost like turning your Windows file server into a miniature content delivery network. Once you install Azure File Sync on your local Windows server, it will automatically stay bi-directionally synced with your files in Azure.


**Describe Azure identity, access, and security**

Describe directory services in Azure, including Microsoft Entra ID and Microsoft Entra Domain Services
> Microsoft Entra ID is a directory service that enables you to sign in and access both Microsoft cloud applications and cloud applications that you develop. Microsoft Entra ID can also help you maintain your on-premises Active Directory deployment.
> Microsoft Entra ID provides services such as:
> * **Authentication**: This includes verifying identity to access applications and resources. It also includes providing functionality such as self-service password reset, multifactor authentication, a custom list of banned passwords, and smart lockout services.
> * **Single sign-on**: Single sign-on (SSO) enables you to remember only one username and one password to access multiple applications. A single identity is tied to a user, which simplifies the security model. As users change roles or leave an organization, access modifications are tied to that identity, which greatly reduces the effort needed to change or disable accounts.
> * **Application management**: You can manage your cloud and on-premises apps by using Microsoft Entra ID. Features like Application Proxy, SaaS apps, the My Apps portal, and single sign-on provide a better user experience.
> * **Device management**: Along with accounts for individual people, Microsoft Entra ID supports the registration of devices. Registration enables devices to be managed through tools like Microsoft Intune. It also allows for device-based Conditional Access policies to restrict access attempts to only those coming from known devices, regardless of the requesting user account.
> 
> A Microsoft Entra Domain Services managed domain lets you run legacy applications in the cloud that can't use modern authentication methods, or where you don't want directory lookups to always go back to an on-premises AD DS environment. 
> 

Describe authentication methods in Azure, including single sign-on (SSO), multifactor authentication (MFA), and passwordless
> Passwordless authentication is high security and high convenience while passwords on their own are low security but high convenience.
> 

Describe external identities and guest access in Azure
> An external identity is a person, device, service, etc. that is outside your organization. Microsoft Entra External ID refers to all the ways you can securely interact with users outside of your organization.
> * Business to business (B2B) collaboration - Collaborate with external users by letting them use their preferred identity to sign-in to your Microsoft applications or other enterprise applications (SaaS apps, custom-developed apps, etc.). B2B collaboration users are represented in your directory, typically as guest users.
> * B2B direct connect - Establish a mutual, two-way trust with another Microsoft Entra organization for seamless collaboration. B2B direct connect currently supports Teams shared channels, enabling external users to access your resources from within their home instances of Teams. B2B direct connect users aren't represented in your directory, but they're visible from within the Teams shared channel and can be monitored in Teams admin center reports.
> * Microsoft Entra business to customer (B2C) - Publish modern SaaS apps or custom-developed apps (excluding Microsoft apps) to consumers and customers, while using Azure AD B2C for identity and access management.

Describe Microsoft Entra Conditional Access
> Conditional Access is useful when you need to:
> * Require multifactor authentication (MFA) to access an application depending on the requester’s role, location, or network. For example, you could require MFA for administrators but not regular users or for people connecting from outside your corporate network.
> * Require access to services only through approved client applications. For example, you could limit which email applications are able to connect to your email service.
> * Require users to access your application only from managed devices. A managed device is a device that meets your standards for security and compliance.
> * Block access from untrusted sources, such as access from unknown or unexpected locations.

Describe Azure Role Based Access Control (RBAC)
> RBAC
> : Role Based Access Control
> 
> The principle of least privilege says you should only grant access up to the level needed to complete a task. 
> Role-based access control is applied to a scope, which is a resource or set of resources that this access applies to.
> Scopes include:
> * A management group (a collection of multiple subscriptions).
> * A single subscription.
> * A resource group.
> * A single resource.
>
> 
Describe the concept of Zero Trust
> Zero Trust is a security model that assumes the worst case scenario and protects resources with that expectation. Zero Trust assumes breach at the outset, and then verifies each request as though it originated from an uncontrolled network.
> * Verify explicitly - Always authenticate and authorize based on all available data points.
> * Use least privilege access - Limit user access with Just-In-Time and Just-Enough-Access (JIT/JEA), risk-based adaptive policies, and data protection.
> * Assume breach - Minimize blast radius and segment access. Verify end-to-end encryption. Use analytics to get visibility, drive threat detection, and improve defenses.
> 
Describe the purpose of the defense in depth model
> The objective of defense-in-depth is to protect information and prevent it from being stolen by those who aren't authorized to access it.
>
> A defense-in-depth strategy uses a series of mechanisms to slow the advance of an attack that aims at acquiring unauthorized access to data.
> * The physical security layer is the first line of defense to protect computing hardware in the datacenter.
> * The identity and access layer controls access to infrastructure and change control.
> * The perimeter layer uses distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for users.
> * The network layer limits communication between resources through segmentation and access controls.
> * The compute layer secures access to virtual machines.
> * The application layer helps ensure that applications are secure and free of security vulnerabilities.
> * The data layer controls access to business and customer data that you need to protect.


Describe the purpose of Microsoft Defender for Cloud
> Defender for Cloud is a monitoring tool for security posture management and threat protection. It monitors your cloud, on-premises, hybrid, and multi-cloud environments to provide guidance and notifications aimed at strengthening your security posture.
> Defender for Cloud helps you detect threats across:
> * Azure PaaS services – Detect threats targeting Azure services including Azure App Service, Azure SQL, Azure Storage Account, and more data services. You can also perform anomaly detection on your Azure activity logs using the native integration with Microsoft Defender for Cloud Apps (formerly known as Microsoft Cloud App Security).
> * Azure data services – Defender for Cloud includes capabilities that help you automatically classify your data in Azure SQL. You can also get assessments for potential vulnerabilities across Azure SQL and Storage services, and recommendations for how to mitigate them.
> * Networks – Defender for Cloud helps you limit exposure to brute force attacks. By reducing access to virtual machine ports, using the just-in-time VM access, you can harden your network by preventing unnecessary access. You can set secure access policies on selected ports, for only authorized users, allowed source IP address ranges or IP addresses, and for a limited amount of time.
>
> Defender for Cloud can also protect resources in other clouds (such as AWS and GCP).
> Defender for Cloud fills three vital needs as you manage the security of your resources and workloads in the cloud and on-premises:
> * Continuously assess – Know your security posture. Identify and track vulnerabilities.
> * Secure – Harden resources and services with Azure Security Benchmark.
> * Defend – Detect and resolve threats to resources, workloads, and services.


