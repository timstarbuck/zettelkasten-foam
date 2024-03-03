

[Learn Link](https://learn.microsoft.com/en-us/training/modules/configure-virtual-networks/?source=recommendations)

Azure virtual networks are an essential component for creating private networks in Azure. They allow different Azure resources to securely communicate with each other, the internet, and on-premises networks.



* An Azure virtual network is a logical isolation of the Azure cloud resources.
* You can use virtual networks to provision and manage virtual private networks (VPNs) in Azure.
* Each virtual network has its own Classless Inter-Domain Routing (CIDR) block and can be linked to other virtual networks and on-premises networks.
* You can link virtual networks with an on-premises IT infrastructure to create hybrid or cross-premises solutions, when the CIDR blocks of the connecting networks don't overlap.


### Things to know about subnets
There are certain conditions for the IP addresses in a virtual network when you apply segmentation with subnets.

* Each subnet contains a range of IP addresses that fall within the virtual network address space.
* The address range for a subnet must be unique within the address space for the virtual network.
* The range for one subnet can't overlap with other subnet IP address ranges in the same virtual network.
* The IP address space for a subnet must be specified by using CIDR notation.
* You can segment a virtual network into one or more subnets in the Azure portal. Characteristics about the IP addresses for the subnets are listed.

### Reserved addresses
For each subnet, Azure reserves five IP addresses. The first four addresses and the last address are reserved.

Let's examine the reserved addresses in an IP address range of 192.168.1.0/24.

|Reserved address|	Reason|
|--|--|
|```192.168.1.0```	|This value identifies the virtual network address.|
|```192.168.1.1```	|Azure configures this address as the default gateway.|
|```192.168.1.2 and 192.168.1.3```	|Azure maps these Azure DNS IP addresses to the virtual network space.|
|```192.168.1.255```	|This value supplies the virtual network broadcast address.|


### Things to consider when using subnets
When you plan for adding subnet segments within your virtual network, there are several factors to consider. Review the following scenarios.

* Consider service requirements. Each service directly deployed into a virtual network has specific requirements for routing and the types of traffic that must be allowed into and out of associated subnets. A service might require or create their own subnet. There must be enough unallocated space to meet the service requirements. Suppose you connect a virtual network to an on-premises network by using Azure VPN Gateway. The virtual network must have a dedicated subnet for the gateway.

* Consider network virtual appliances. Azure routes network traffic between all subnets in a virtual network, by default. You can override Azure's default routing to prevent Azure routing between subnets. You can also override the default to route traffic between subnets through a network virtual appliance. If you require traffic between resources in the same virtual network to flow through a network virtual appliance, deploy the resources to different subnets.

* Consider service endpoints. You can limit access to Azure resources like an Azure storage account or Azure SQL database to specific subnets with a virtual network service endpoint. You can also deny access to the resources from the internet. You might create multiple subnets, and then enable a service endpoint for some subnets, but not others.

* Consider network security groups. You can associate zero or one network security group to each subnet in a virtual network. You can associate the same or a different network security group to each subnet. Each network security group contains rules that allow or deny traffic to and from sources and destinations.

* Consider private links. Azure Private Link provides private connectivity from a virtual network to Azure platform as a service (PaaS), customer-owned, or Microsoft partner services. Private Link simplifies the network architecture and secures the connection between endpoints in Azure. The service eliminates data exposure to the public internet.

### Things to know about creating virtual networks
Review the following requirements for creating a virtual network.

* When you create a virtual network, you need to define the IP address space for the network.

* Plan to use an IP address space that's not already in use in your organization.

* The address space for the network can be either on-premises or in the cloud, but not both.

* Once you create the IP address space, it can't be changed. If you plan your address space for cloud-only virtual networks, you might later decide to connect an on-premises site.

* To create a virtual network, you need to define at least one subnet.

* Each subnet contains a range of IP addresses that fall within the virtual network address space.

* The address range for each subnet must be unique within the address space for the virtual network.

* The range for one subnet can't overlap with other subnet IP address ranges in the same virtual network.

* You can create a virtual network in the Azure portal. Provide the Azure subscription, resource group, virtual network name, and service region for the network.

### Things to know about IP addresses
Let's take a closer look at the characteristics of IP addresses.

* IP addresses can be statically assigned or dynamically assigned.

* You can separate dynamically and statically assigned IP resources into different subnets.

* Static IP addresses don't change and are best for certain situations, such as:

  * DNS name resolution, where a change in the IP address requires updating host records.
  * IP address-based security models that require apps or services to have a static IP address.
  * TLS/SSL certificates linked to an IP address.
  * Firewall rules that allow or deny traffic by using IP address ranges.
  * Role-based virtual machines such as Domain Controllers and DNS servers.

### Things to consider when creating a public IP address
To create a public IP address, configure the following settings:

* IP Version: Select to create an IPv4 or IPv6 address, or Both addresses.

* SKU: Select the SKU for the public IP address, including Basic or Standard. The value must match the SKU of the Azure load balancer with which the address is used.

* Name: Enter a name to identify the IP address. The name must be unique within the resource group you select.

* IP address assignment: Identify the type of IP address assignment to use.

  * Dynamic addresses are assigned after a public IP address is associated to an Azure resource and is started for the first time. Dynamic addresses can change if a resource such as a virtual machine is stopped (deallocated) and then restarted through Azure. The address remains the same if a virtual machine is rebooted or stopped from within the guest OS. When a public IP address resource is removed from a resource, the dynamic address is released.

  * Static addresses are assigned when a public IP address is created. Static addresses aren't released until a public IP address resource is deleted. If the address isn't associated to a resource, you can change the assignment method after the address is created. If the address is associated to a resource, you might not be able to change the assignment method.

### Public IP address SKUs

|Feature	|Basic SKU  |Standard SKU | 
|---|---|--|
|IP assignment	| Static or Dynamic	|Static|
|Security	|Open by default	|Secure by default, closed to inbound traffic|
|Resources	|Network interfaces, VPN gateways, Application gateways, and internet-facing load balancers	|Network interfaces or public standard load balancers|
|Redundancy	|Not zone redundant	|Zone redundant by default|

### Private Lins vs Service Endpoints

Private Link and Service Endpoints are two ways to connect to Azure services from a private network12. Private Link assigns a private IP to a specific instance of a service and makes it accessible only from the virtual network where the private endpoint is configured12. Service Endpoints provide a direct path to a whole service type, but they remain publicly routable IP addresses12.


### What is a subnet?
A subnet, or subnetwork, is a network inside a network. Subnets make networks more efficient. Through subnetting, network traffic can travel a shorter distance without passing through unnecessary routers to reach its destination.


Class A network
: Everything before the first period indicates the network, and everything after it specifies the device within that network. Using 203.0.113.112 as an example, the network is indicated by "203" and the device by "0.113.112."

Class B network
: Everything before the second period indicates the network. Again using 203.0.113.112 as an example, "203.0" indicates the network and "113.112" indicates the device within that network.

Class C network
: For Class C networks, everything before the third period indicates the network. Using the same example, "203.0.113" indicates the Class C network, and "112" indicates the device.

* /32 (255.255.255.255) is the subnet mask for a single machine, because every single bit in the subnet mask is set to 1. In other words, nothing can change. You would use this on the loopback interface of a router, or in a firewall rule that refers to a specific machine.
* /29 (255.255.255.248) gives you eight IPs. If you buy a corporate internet connection from an ISP, it’s common for you to get a block of eight public IPs with it.
* /24 (255.255.255.0) is very common on a LAN. It gives you 256 IPs. If you need more, you can jump up to /23, which is written as 255.255.254.0. This gives you 512 IPs. That might seem like a big jump, but in practice this is almost always done using private IP addresses, so it’s generally not as wasteful as it might seem.
* /16 (255.255.0.0) 65,536 
