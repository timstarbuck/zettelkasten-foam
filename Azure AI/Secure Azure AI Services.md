# Azure AI/Secure Azure AI Services

### Authentication

**Regenerate Keys** - keys should be regenerated regularly. This can be done without downtime by regenerating Key 2, swapping the running resources Key 1 with Key 2, then regenerate Key 1, swap again.

**Key Vault** - store keys in Azure Key Vault.

**Token-based authentication** -  Some AI services support (or require) token-based auth with the REST interace. In these cases the key is used to obtain an auth token which is valid for 10 minutes. 

### Microsoft Entra ID authentication

1. Authenticate using service principals
   1. Create a custom subdomain
   2. Assign a role to a service principal
   3. Register an application
2. Authenticate using managed identities
   1. System Assigned or User Assigned
   2. Assign a role to the managed identity (i.e. Cognitive Services Contributor)
 
### Implement Network Security

**Network Access Restrictions**
By default, Azure AI services are accessible from all networks. Some individual AI services resources (such as Azure AI Face service, Azure AI Vision, and others) can be configured to restrict access to specific network addresses - either public Internet addresses or addresses on virtual networks.
