# Fundamentals Certifications - Describe Azure management and governance

[Learn Path](https://learn.microsoft.com/en-us/training/paths/describe-azure-management-governance/){:target="_blank"}

**Describe factors that can affect costs in Azure.**
> * Resource type
> * Consumption
> * Maintenance
> * Geography
> * Subscription type
> * Azure Marketplace

**Compare the Pricing calculator and Total Cost of Ownership (TCO) calculator.**
> * [Pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/?cdn=disable){:target="_blank"}
>   * The pricing calculator is designed to give you an estimated cost for provisioning resources in Azure.
> * [TCO calculator](https://azure.microsoft.com/en-us/pricing/tco/calculator/){:target="_blank"}
>   * The TCO calculator is designed to help you compare the costs for running an on-premises infrastructure compared to an Azure Cloud infrastructure. 
>
> 
Describe the Microsoft Cost Management Tool.
> Cost Management provides the ability to quickly check Azure resource costs, create alerts based on resource spend, and create budgets that can be used to automate management of resources.



Describe the purpose of tags.
> One way to organize related resources is to place them in their own subscriptions. You can also use resource groups to manage related resources. Resource tags are another way to organize resources. Tags provide extra information, or metadata, about your resources. This metadata is useful for:
> Resource management Tags enable you to locate and act on resources that are associated with specific workloads, environments, business units, and owners.
> Cost management and optimization Tags enable you to group resources so that you can report on costs, allocate internal cost centers, track budgets, and forecast estimated cost.
> Operations management Tags enable you to group resources according to how critical their availability is to your business. This grouping helps you formulate service-level agreements (SLAs). An SLA is an uptime or performance guarantee between you and your users.
> Security Tags enable you to classify data by its security level, such as public or confidential.
> Governance and regulatory compliance Tags enable you to identify resources that align with governance or regulatory compliance requirements, such as ISO 27001. Tags can also be part of your standards enforcement efforts. For example, you might require that all resources be tagged with an owner or department name.
> Workload optimization and automation Tags can help you visualize all of the resources that participate in complex deployments. For example, you might tag a resource with its associated workload or application name and use software such as Azure DevOps to perform automated tasks on those resources.

| Name | Value |
| ---------------- | ------------------------|
| AppName	| The name of the application that the resource is part of.|
| CostCenter	| The internal cost center code. |
| Owner	| The name of the business owner who's responsible for the resource. |
|Environment |An environment name, such as "Prod," "Dev," or "Test."|
| Impact | How important the resource is to business operations, such as "Mission-critical," "High-impact," or "Low-impact."|

**Describe features and tools in Azure for governance and compliance**
Describe the purpose of Microsoft Purview
> Microsoft Purview is a family of data governance, risk, and compliance solutions that helps you get a single, unified view into your data. Microsoft Purview brings insights about your on-premises, multicloud, and software-as-a-service data together.
> Two main solution areas comprise Microsoft Purview: risk and compliance and unified data governance.
> Microsoft 365 features as a core component of the Microsoft Purview risk and compliance solutions. Microsoft Teams, OneDrive, and Exchange are just some of the Microsoft 365 services that Microsoft Purview uses to help manage and monitor your data.
> Microsoft Purview’s robust data governance capabilities enable you to manage your data stored in Azure, SQL and Hive databases, locally, and even in other clouds like Amazon S3.
> 
Describe the purpose of Azure Policy
> Azure Policy is a service in Azure that enables you to create, assign, and manage policies that control or audit your resources. These policies enforce different rules across your resource configurations so that those configurations stay compliant with corporate standards.
> Azure Policies can be set at each level, enabling you to set policies on a specific resource, resource group, subscription, and so on. Additionally, Azure Policies are inherited.
> An Azure Policy initiative is a way of grouping related policies together. The initiative definition contains all of the policy definitions to help track your compliance state for a larger goal.
> 
Describe the purpose of resource locks
> A resource lock prevents resources from being accidentally deleted or changed.
> There are two types of resource locks, one that prevents users from deleting (Delete) and one that prevents users from changing or deleting a resource (ReadOnly).
> 
Describe the purpose of the Service Trust portal
> The Microsoft Service Trust Portal is a portal that provides access to various content, tools, and other resources about Microsoft security, privacy, and compliance practices.
> [Service Trust Portal](https://servicetrust.microsoft.com/){:target: "_blank"}

**Describe features and tools for managing and deploying Azure resources**
Describe Azure portal
> The Azure portal is a web-based, unified console that provides an alternative to command-line tools. With the Azure portal, you can manage your Azure subscription by using a graphical user interface.
> 
Describe Azure Cloud Shell, including Azure CLI and Azure PowerShell
> Azure Cloud Shell is a browser-based shell tool that allows you to create, configure, and manage Azure resources using a shell. Azure Cloud Shell support both Azure PowerShell and the Azure Command Line Interface (CLI), which is a Bash shell.
> You can access Azure Cloud Shell via the Azure portal by selecting the Cloud Shell icon in the portal.
> Azure PowerShell uses PowerShell commands, the Azure CLI uses Bash commands.
> 
Describe the purpose of Azure Arc
> Arc lets you extend your Azure compliance and monitoring to your hybrid and multi-cloud configurations. Azure Arc simplifies governance and management by delivering a consistent multi-cloud and on-premises management platform.

>Azure Arc provides a centralized, unified way to:
> * Manage your entire environment together by projecting your existing non-Azure resources into ARM.
> * Manage multi-cloud and hybrid virtual machines, Kubernetes clusters, and databases as if they are running in Azure.
> * Use familiar Azure services and management capabilities, regardless of where they live.
> * Continue using traditional ITOps while introducing DevOps practices to support new cloud and native patterns in your environment.
> * Configure custom locations as an abstraction layer on top of Azure Arc-enabled Kubernetes clusters and cluster extensions.

>Currently, Azure Arc allows you to manage the following resource types hosted outside of Azure:
> * Servers
> * Kubernetes clusters
> * Azure data services
> * SQL Server
> * Virtual machines (preview)
> 
Describe Azure Resource Manager (ARM) and Azure ARM templates
> Azure Resource Manager (ARM) is the deployment and management service for Azure. It provides a management layer that enables you to create, update, and delete resources in your Azure account. Anytime you do anything with your Azure resources, ARM is involved.
> ARM Templaces and Bicep files are used as IaC (Infrastructure as Code)

**Describe monitoring tools in Azure**
Describe the purpose of Azure Advisor
> Azure Advisor evaluates your Azure resources and makes recommendations to help improve reliability, security, and performance, achieve operational excellence, and reduce costs. Azure Advisor is designed to help you save time on cloud optimization. 
> * Reliability is used to ensure and improve the continuity of your business-critical applications.
> * Security is used to detect threats and vulnerabilities that might lead to security breaches.
> * Performance is used to improve the speed of your applications.
> * Operational Excellence is used to help you achieve process and workflow efficiency, resource manageability, and deployment best practices.
> * Cost is used to optimize and reduce your overall Azure spending.
> 
Describe Azure Service Health
> Azure Service Health helps you keep track of Azure resource, both your specifically deployed resources and the overall status of Azure. Azure service health does this by combining three different Azure services:

> * Azure Status is a broad picture of the status of Azure globally. Azure status informs you of service outages in Azure on the Azure Status page. The page is a global view of the health of all Azure services across all Azure regions. It’s a good reference for incidents with widespread impact.
> * Service Health provides a narrower view of Azure services and regions. It focuses on the Azure services and regions you're using. This is the best place to look for service impacting communications about outages, planned maintenance activities, and other health advisories because the authenticated Service Health experience knows which services and resources you currently use. You can even set up Service Health alerts to notify you when service issues, planned maintenance, or other changes may affect the Azure services and regions you use.
> * Resource Health is a tailored view of your actual Azure resources. It provides information about the health of your individual cloud resources, such as a specific virtual machine instance. Using Azure Monitor, you can also configure alerts to notify you of availability changes to your cloud resources.
> 
Describe Azure Monitor, including Azure Log Analytics, Azure Monitor Alerts, and Application Insights
> Azure Monitor is a platform for collecting data on your resources, analyzing that data, visualizing the information, and even acting on the results.
> * Azure Log Analytics
>   * Azure Log Analytics is the tool in the Azure portal where you’ll write and run log queries on the data gathered by Azure Monitor. Log Analytics is a robust tool that supports both simple, complex queries, and data analysis. You can write a simple query that returns a set of records and then use features of Log Analytics to sort, filter, and analyze the records. You can write an advanced query to perform statistical analysis and visualize the results in a chart to identify a particular trend. Whether you work with the results of your queries interactively or use them with other Azure Monitor features such as log query alerts or workbooks, Log Analytics is the tool that you're going to use to write and test those queries.
> * Azure Monitor Alerts
>   * Azure Monitor Alerts are an automated way to stay informed when Azure Monitor detects a threshold being crossed. You set the alert conditions, the notification actions, and then Azure Monitor Alerts notifies when an alert is triggered. Depending on your configuration, Azure Monitor Alerts can also attempt corrective action.
> * Application Insights
>   * Application Insights, an Azure Monitor feature, monitors your web applications. Application Insights is capable of monitoring applications that are running in Azure, on-premises, or in a different cloud environment.

