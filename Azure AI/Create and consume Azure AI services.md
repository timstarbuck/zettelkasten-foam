# Azure AI/Create and consume Azure AI services

To consume a service via its endpoint:

* **The endpoint URI.** This is the HTTP address at which the REST interface for the service can be accessed. Most AI services software development kits (SDKs) use the endpoint URI to initiate a connection to the endpoint.
* **A subscription key.** Access to the endpoint is restricted based on a subscription key. Client applications must provide a valid key to consume the service. When you provision an AI services resource, two keys are created - applications can use either key. You can also regenerate the keys as required to control access to your resource.
* **The resource location.** When you provision a resource in Azure, you generally assign it to a location, which determines the Azure data center in which the resource is defined. While most SDKs use the endpoint URI to connect to the service, some require the location.

You can use a REST API, or an SDK in common languages:
* Microsoft C# (.NET Core)
* Python
* JavaScript (Node.js)
* Go
* Java
