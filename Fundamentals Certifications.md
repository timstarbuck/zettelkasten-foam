# Fundamentals Certifications

[Certification Overview](https://learn.microsoft.com/en-us/credentials/certifications/azure-fundamentals/)

#### Define cloud computing.
> Cloud computing is the delivery of computing services over the internet. Computing services include common IT infrastructure such as virtual machines, storage, databases, and networking. Cloud services also expand the traditional IT offerings to include things like Internet of Things (IoT), machine learning (ML), and artificial intelligence (AI).
> 
Describe the shared responsibility model.
> Physical security, power, cooling, and network connectivity are the responsibility of the cloud provider.
> At the same time, the consumer is responsible for the data and information stored in the cloud. (You wouldn’t want the cloud provider to be able to read your information.) The consumer is also responsible for access security, meaning you only give access to those who need it.
![Shared Responsibility Chart](./assets/images/shared-responsibility-b3829bfe.svg)

Define cloud models, including public, private, and hybrid.
>Private Cloud - a cloud (delivering IT services over the internet) that’s used by a single entity. 
> Public Cloud - A public cloud is built, controlled, and maintained by a third-party cloud provider. 
> Hybrid Cloud - A hybrid cloud is a computing environment that uses both public and private clouds in an inter-connected environment. A hybrid cloud environment can be used to allow a private cloud to surge for increased, temporary demand by deploying public cloud resources. Hybrid cloud can be used to provide an extra layer of security. 

Describe the consumption-based model.
> This consumption-based model has many benefits, including:
>  - No upfront costs.
>  - No need to purchase and manage costly infrastructure that users might not use to its fullest potential.
>  - The ability to pay for more resources when they're needed.
>  - The ability to stop paying for resources that are no longer needed.


#### Describe the benefits of using cloud services

Describe the benefits of high availability and scalability in the cloud.
> High Availability - Azure is a highly available cloud environment with uptime guarantees depending on the service.
> 99% = 7.2 hours of downtime/month. 
> 99.9% 43.2 minutes of downtime/month.
>
> Scalability  - Scalability refers to the ability to adjust resources to meet demand. If you suddenly experience peak traffic and your systems are overwhelmed, the ability to scale means you can add more resources to better handle the increased demand.
> Vertical Scaling - Adding more processing power.
> Horizontal Scaling - Adding more VMs/Containers.
> 
Describe the benefits of reliability and predictability in the cloud.
> Reliability is the ability of a system to recover from failures and continue to function.
> Predictability can be focused on performance predictability or cost predictability.
> Performance predictability focuses on predicting the resources needed to deliver a positive experience for your customers. Autoscaling, load balancing, and high availability are just some of the cloud concepts that support performance predictability.
> Cost predictability is focused on predicting or forecasting the cost of the cloud spend.
> 
Describe the benefits of security and governance in the cloud.

Describe the benefits of manageability in the cloud.

#### Describe cloud service types
Describe Infrastructure as a Service (IaaS).
> IaaS places the largest share of responsibility with you. The cloud provider is responsible for maintaining the physical infrastructure and its access to the internet. You’re responsible for installation and configuration, patching and updates, and security.
> Some common scenarios where IaaS might make sense include:
> Lift-and-shift migration: You’re standing up cloud resources similar to your on-prem datacenter, and then simply moving the things running on-prem to running on the IaaS infrastructure.
> Testing and development: You have established configurations for development and test environments that you need to rapidly replicate. You can stand up or shut down the different environments rapidly with an IaaS structure, while maintaining complete control.
> 
Describe Platform as a Service (PaaS).
> In a PaaS environment, the cloud provider maintains the physical infrastructure, physical security, and connection to the internet. They also maintain the operating systems, middleware, development tools, and business intelligence services that make up a cloud solution. In a PaaS scenario, you don't have to worry about the licensing or patching for operating systems and databases.
>
> PaaS is well suited to provide a complete development environment without the headache of maintaining all the development infrastructure.
> Common uses - 
> Development framework: PaaS provides a framework that developers can build upon to develop or customize cloud-based applications.
> Analytics or business intelligence tools.
>
> 
Describe Software as a Service (SaaS).
> Software as a service (SaaS) is the most complete cloud service model from a product perspective. With SaaS, you’re essentially renting or using a fully developed application. Email, financial software, messaging applications, and connectivity software are all common examples of a SaaS implementation.
> In the shared responsibility model, most responsibility is placed on the cloud provider. Customer is responsible for the data.
> Common use cases:
>   Email/messaging, Business productivity applications, Finance and expense tracking.
> 
Identify appropriate use cases for each cloud service (IaaS, PaaS, SaaS).


#### Describe Azure compute and networking services
Three of the compute options are virtual machines, containers, and Azure functions.

<b>VMs</b>
> VMs are IaaS.
> 
> Virtual machine scale sets let you create and manage a group of identical, load-balanced VMs. Scale sets allow you to centrally manage, configure, and update a large number of VMs in minutes.
> 
> Virtual machine availability sets are designed to ensure that VMs stagger updates and have varied power and network connectivity, preventing you from losing all your VMs with a single network or power failure.
> Availability sets do this by grouping VMs in two ways: update domain and fault domain. By default, an availability set will split your VMs across up to three fault domains.

<b>Azure Virtual Desktop</b>
> Azure Virtual Desktop is a desktop and application virtualization service that enables you to use a cloud-hosted version of Windows from any location. Azure Virtual Desktop works across devices and operating systems, and works with apps that you can use to access remote desktops or most modern browsers.

<b>Containers</b>
> Azure Container Instances
Azure Container Instances offer the fastest and simplest way to run a container in Azure; without having to manage any virtual machines or adopt any additional services. Azure Container Instances are a platform as a service (PaaS) offering. Azure Container Instances allow you to upload your containers and then the service will run the containers for you.

> Azure Container Apps
Azure Container Apps are similar in many ways to a container instance. They allow you to get up and running right away, they remove the container management piece, and they're a PaaS offering. Container Apps have extra benefits such as the ability to incorporate load balancing and scaling. These other functions allow you to be more elastic in your design.

>Azure Kubernetes Service
Azure Kubernetes Service (AKS) is a container orchestration service. An orchestration service manages the lifecycle of containers. When you're deploying a fleet of containers, AKS can make fleet management simpler and more efficient.

<b>Azure Functions</b>
> Azure Functions is an event-driven, serverless compute option that doesn’t require maintaining virtual machines or containers.
> Functions scale automatically based on demand, so they may be a good choice when demand is variable.
> Functions can be either stateless or stateful. When they're stateless (the default), they behave as if they're restarted every time they respond to an event. When they're stateful (called Durable Functions), a context is passed through the function to track prior activity

<b>Describe application hosting options</b>
> App Service enables you to build and host web apps, background jobs, mobile back-ends, and RESTful APIs in the programming language of your choice without managing infrastructure. It offers automatic scaling and high availability. App Service supports Windows and Linux. It enables automated deployments from GitHub, Azure DevOps, or any Git repo to support a continuous deployment model.
> 
<b>Describe Azure virtual networking</b>
> Azure virtual networks provide the following key networking capabilities:
> Isolation and segmentation
> Internet communications
> Communicate between Azure resources
> Communicate with on-premises resources
> Route network traffic
> Filter network traffic
> Connect virtual networks

<b>Describe Azure virtual private networks</b>
> A virtual private network (VPN) uses an encrypted tunnel within another network. VPNs are typically deployed to connect two or more trusted private networks to one another over an untrusted network (typically the public internet). Traffic is encrypted while traveling over the untrusted network to prevent eavesdropping or other attacks.

<b>Describe Azure ExpressRoute</b>
> Azure ExpressRoute lets you extend your on-premises networks into the Microsoft cloud over a private connection, with the help of a connectivity provider. This connection is called an ExpressRoute Circuit.
> 
>There are several benefits to using ExpressRoute as the connection service between Azure and on-premises networks.
>Connectivity to Microsoft cloud services across all regions in the geopolitical region.
>Global connectivity to Microsoft services across all regions with the ExpressRoute Global Reach.
>Dynamic routing between your network and Microsoft via Border Gateway Protocol (BGP).
>Built-in redundancy in every peering location for higher reliability.

> 
Compare compute types, including container instances, virtual machines, and functions
Describe virtual machine (VM) options, including VMs, Virtual Machine Scale Sets, availability sets, Azure Virtual Desktop
Describe resources required for virtual machines
Describe application hosting options, including Azure Web Apps, containers, and virtual machines
Describe virtual networking, including the purpose of Azure Virtual Networks, Azure virtual subnets, peering, Azure DNS, VPN Gateway, and ExpressRoute
Define public and private endpoints
