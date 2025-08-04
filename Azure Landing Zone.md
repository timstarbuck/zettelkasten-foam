# Azure Landing Zone

The [conceptual architecture reference](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/media/azure-landing-zone-architecture-diagram-hub-spoke.svg#lightbox) represents an opinionated architecture for your Azure Landing Zone.


### Design Areas
- Azure billing and Active Directory tenant
- Microsoft Entra tenant
- Resource organization 
- Network topology and connectivity
- Security
- Management 
- Governance
- Platform automation and DevOps

### Landing Zone Approaches

| Application landing zone management approach |Description|
| ---- | ---- |
|Central team management	| A central IT team fully operates the landing zone. The team applies controls and platform tools to the platform and application landing zones. |
|Application team management |	A platform administration team delegates the entire application landing zone to an application team. The application team manages and supports the environment. The management group policies ensure that the platform team still governs the application landing zone. You can add other policies at the subscription scope and use alternative tooling for deploying, securing, or monitoring application landing zones. |
|Shared management	| With technology platforms such as AKS or AVS, a central IT team manages the underlying service. The application teams are responsible for the applications running on top of the technology platforms. You need to use different controls or access permissions for this model. These controls and permissions differ from the ones you use to manage application landing zones centrally. |

[Subscription Vending](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-area/subscription-vending)
[Subscription vending implementation guidance](https://learn.microsoft.com/en-us/azure/architecture/landing-zones/subscription-vending)
[Bicep Subscription Vending](https://github.com/Azure/bicep-registry-modules/tree/main/avm/ptn/lz/sub-vending)


### Managing Application Envionments

|Environment	|Description	|Management group|
|--|--|--|
|Sandbox	|The environment that's used for rapid innovation of prototypes but not production-bound configurations	|Sandbox management group|
|Development|	The environment that's used to build potential release candidates	|Archetype management group, like corp or online|
|Test	|The environment that's used to perform testing, including unit testing, user acceptance testing, and quality assurance testing	|Archetype management group, like corp or online|
|Production	|The environment that's used to deliver value to customers	|Archetype management group, like corp or online|

[Handling dev, test, prod](https://youtu.be/8ECcvTxkrJA)
Is the governance model different between test and prod? The answer should be no. So, test and prod should be in same management groups.
If dev requires same governance, place in same Management Group as prod and test subscriptions.
If dev requires different governance (perhaps using new Azure services, place under sandboxes).
*Sandbox* 
- Same general landing zone policies, but in audit mode. 
- sandbox-specific policies (to reduce risk, e.g. deny on-prem, deny vnet-peering, etc.).
- Can have corp-sandbox and online-sandbox.



[How many subscriptions should I use in Azure](https://youtu.be/R-5oeguxFpo)

Availability zones across subscriptions may not be the same. AZ1, AZ2 and AZ3 in one subscription may not be the same as AZ1, AZ2 and AZ3 in another.

Governance model between test and prod should be the same. 
Dev may require same Governance, if so, ise same Management Group as prod and test. If not, use a sandbox landing zone with sandbox-specific policies.

Corp management groups (archetype) - no public IPs allowed.
Online management groups (archetype) - public IPs allowed.




