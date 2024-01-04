# AZ-204/AZ-204 Implement secure Azure solutions

### Explore Azure Key Vault

* Describe the benefits of using Azure Key Vault
* Explain how to authenticate to Azure Key Vault
* Set and retrieve a secret from Azure Key Vault by using the Azure CLI

Azure Key Vault solves these problems:
* **Secrets management**: Securely store and tightly control access to tokens, passwords, certificates, API Keys and other secrets.
* **Key Management**: Makes it easy to create and control encryption keys.
* **Certificate Management**: lets you easily provision, manage and deploy public and private SSL.TSL certs for use with Azure and on prem.


Azure Key Vault has two service tiers: Standard, which encrypts with a software key, and a Premium tier, which includes hardware security module(HSM)-protected keys

Key Benefits
* Centralized application secrets
* Securely store secrets and keys
  * Authentication is done via Microsoft Entra ID.
  * Authorization may be done via Azure role-based access control (Azure RBAC) or Key Vault access policy. 
    * Azure RBAC is used when dealing with the management of the vaults.
    * Key vault access policy is used when attempting to access data stored in a vault.
* Monitor access and use 
  * Can enable logging  
    * Archive in storage account
    * Stream to event hub
    * Send to Azure Monitor Logs
* Simplified administration of application secrets. 
  * Removing the need for in-house knowledge of Hardware Security Modules
  * Scaling up on short notice to meet your organization’s usage spikes.
  * Replicating the contents of your Key Vault within a region and to a secondary region. Data replication ensures high availability and takes away the need of any action from the administrator to trigger the failover.
  * Providing standard Azure administration options via the portal, Azure CLI and PowerShell.
  * Automating certain tasks on certificates that you purchase from Public CAs, such as enrollment and renewal.

##### Azure Key Vault best practices

**Authentication**
Three ways to authenticate with Key Vault
1. **Managed identities for Azure resources**: Azure resources (i.e. VM, App Service) can be assigned an identity. Azure automatically rotates the service principal client secret assocaited with the identity. This is the recommended approach/best practice.
2. **Service principal and certificate**: App owner must rotate the certificates.
3. **Service principal and secret**: Again, app owner must rotate the secret.

##### Encryption of data in transit
Azure Key Vault enforces Transport Layer Security (TLS) protocol to protect data when it’s traveling between Azure Key Vault and clients.

##### Azure Key Vault best practices
* Use separate key vaults: Recommended using a vault per application per environment (Development, Pre-Production and Production). This pattern helps you not share secrets across environments and also reduces the threat if there is a breach.
* Control access to your vault: Key Vault data is sensitive and business critical, you need to secure access to your key vaults by allowing only authorized applications and users.
* Backup: Create regular back ups of your vault on update/delete/create of objects within a Vault.
* Logging: Be sure to turn on logging and alerts.
* Recovery options: Turn on soft-delete and purge protection if you want to guard against force deletion of the secret.


##### Authentication to Key Vault
It is recommended to use a managed identity from Azure resources (or existing user with access).
If managed identity is not possible (accessing from external application perhaps), it is necessary to register an application with Microsoft Entra (and use a client secret or certificate).

Methods of Authenticating with Key Vault
* Dot Net Aure Identity SDK
* Python Aure Identity SDK
* Java Aure Identity SDK
* Javascript Aure Identity SDK
* REST - Access tokens must be sent to the service using the HTTP Authorization header:
  * ```Authorization: Bearer <access_token>```

**Types of Managed Identities**

System-assigned managed identity
: enabled directly on an Azure service instance. When the identity is enabled, Azure creates an identity for the instance in the Microsoft Entra tenant that's trusted by the subscription of the instance. After the identity is created, the credentials are provisioned onto the instance. The lifecycle of a system-assigned identity is directly tied to the Azure service instance that it's enabled on. If the instance is deleted, Azure automatically cleans up the credentials and the identity in Microsoft Entra ID.

User-assigned managed identity
: created as a standalone Azure resource. Through a create process, Azure creates an identity in the Microsoft Entra tenant that's trusted by the subscription in use. After the identity is created, the identity can be assigned to one or more Azure service instances. The lifecycle of a user-assigned identity is managed separately from the lifecycle of the Azure service instances to which it's assigned.

### Azure App Configuration service

Azure App Configuration stores configuration data as key-value pairs.

App Configuration offers the following benefits:
* A fully managed service that can be set up in minutes
* Flexible key representations and mappings
* Tagging with labels
* Point-in-time replay of settings
* Dedicated UI for feature flag management
* Comparison of two sets of configurations on custom-defined dimensions
* Enhanced security through Azure-managed identities
* Encryption of sensitive information at rest and in transit
* Native integration with popular frameworks

App Configuration makes it easier to implement the following scenarios:
* Centralize management and distribution of hierarchical configuration data for different environments and geographies
* Dynamically change application settings without the need to redeploy or restart an application
* Control feature availability in real-time

How to Connect
* provider for Dot Net
* builder for Dot Net
* client for Spring cloud
* client for Javascript
* client for Python
* [Rest API](https://learn.microsoft.com/en-us/rest/api/appconfiguration/)

##### App Configuration Keys
* Common to use hierarchical namepsace in key name by using a delimiter such as ```/``` or ```:```
* Keys are case-sensitive, unicode-based strings.
* Use any unicode character exept ```*,\```, reserved character can be escaped.
* Limit of 10k characters on key-value pairs
* Key heirarchy naming
  * Based on component services
    ```
      AppName:Service1:ApiEndpoint
      AppName:Service2:ApiEndpoint
    ```
  * Based on deployment regions
    ```
      AppName:Region1:DbEndpoint
      AppName:Region2:DbEndpoint
    ```

Label Keys
: Keys can have a label attribute (```null by default```). Common usage:
```
Key = AppName:DbEndpoint & Label = Test
Key = AppName:DbEndpoint & Label = Staging
Key = AppName:DbEndpoint & Label = Production
```

Query key values
: Each key value is uniquely identified by its key plus a label that can be null. You query an App Configuration store for key values by specifying a pattern. The App Configuration store returns all key values that match the pattern and their corresponding values and attributes.

Values
: Values assigned to keys are also unicode strings. You can use all unicode characters for values. There's an optional user-defined content type associated with each value. Use this attribute to store information, for example an encoding scheme, about a value that helps your application to process it properly.

*Configuration data stored in an App Configuration store, which includes all keys and values, is encrypted at rest and in transit. App Configuration isn't a replacement solution for Azure Key Vault. **Don't store application secrets in it.***

##### Feature Management Concepts
* Feature flag: A feature flag is a variable with a binary state of on or off. The feature flag also has an associated code block. The state of the feature flag triggers whether the code block runs or not.
* Feature manager: A feature manager is an application package that handles the lifecycle of all the feature flags in an application. The feature manager typically provides extra functionality, such as caching feature flags and updating their states.
* Filter: A filter is a rule for evaluating the state of a feature flag. A user group, a device or browser type, a geographic location, and a time window are all examples of what a filter can represent.

The feature manager supports appsettings.json as a configuration source for feature flags. The following example shows how to set up feature flags in a JSON file:

```
"FeatureManagement": {
    "FeatureA": true, // Feature flag set to on
    "FeatureB": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```
Azure App Configuration is designed to be a centralized repository for feature flags. You can use it to define different kinds of feature flags and manipulate their states quickly and confidently. You can then use the App Configuration libraries for various programming language frameworks to easily access these feature flags from your application.

##### Secure app configuration data

By default Azure App Configuration encrypts sensitive information at rest using a 256-bit AES encryption key provided by Microsoft. Every App Configuration instance has its own encryption key managed by the service and used to encrypt sensitive information.

Customer-managed keys are supported. Requirements:
* Standard tier Azure App Configuration instance
* Azure Key Vault with soft-delete and purge-protection features enabled
* An RSA or RSA-HSM key in the vault. Key must be enabled and have wrap and unwrap capabilities enabled.
* Azure App Configuration instance must have a managed identity assigned.
* Managed identity must be granted ```GRANT, WRAP and UPWRAP``` permissions in the Key Vault access policy.

**Private Endpoints**
* Secure your application configuration details by configuring the firewall to block all connections to App Configuration on the public endpoint.
* Increase security for the virtual network (VNet) ensuring data doesn't escape from the VNet.
* Securely connect to the App Configuration store from on-premises networks that connect to the VNet using VPN or ExpressRoutes with private-peering.

