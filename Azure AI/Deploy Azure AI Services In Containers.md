# Azure AI/Deploy Azure AI Services In Containers

A container comprises an application or service and the runtime components needed to run it, while abstracting the underlying operating system and hardware. In practice, this abstraction results in two significant benefits:

* Containers are portable across hosts, which may be running different operating systems or use different hardware - making it easier to move an application and all its dependencies.
* A single container host can support multiple isolated containers, each with its own specific runtime configuration - making it easier to consolidate multiple applications that have different configuration requirement.

A container is encapsulated in a container image that defines the software and configuration it must support. Images can be stored in a central registry, such as Docker Hub, or you can maintain a set of images in your own registry.

Containers can be deployed to a host such as:
* A Docker* server.
* An Azure Container Instance (ACI).
* An Azure Kubernetes Service (AKS) cluster.

There are container images for Azure AI services in the Microsoft Container Registry that you can use to deploy a containerized service that encapsulates an individual Azure AI services service API.

Even when using a container, you must provision an Azure AI services resource in Azure for billing purposes. Client applications send their requests to the containerized service, meaning that potentially sensitive data is not sent to the Azure AI services endpoint in Azure; but the container must be able to connect to the Azure AI services resource in Azure periodically to send usage metrics for billing.

Container images are available for 
* Language  
  * Key Phrase Extraction
  * Language Detection
  * Sentiment Analysis
  * Named Entity Recognition
  * Text Analytics for health
  * Translator
  * Summarization
* Speech
  * Speech to Text
  * Custom speech to Text
  * Neural Tet to speech
  * Speech language Detection
* Vision
  * Read OCR
  * Spatial Analysis

Container configuration
* ApiKey - key from your service, used for billing
* Billing - Endpoint URI from your deployed Azure AI service - used for billing
* Elua - value of `accept` to state you accept the license for the container.
