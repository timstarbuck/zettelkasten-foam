# AZ-204 Implement Azure Functions

[Learn path](https://learn.microsoft.com/en-us/training/paths/implement-azure-functions/){:target:="_blank"}

### Explore Azure Functions

* Explain functional differences between Azure Functions, Azure Logic Apps, and WebJobs
* Describe Azure Functions hosting plan options
* Describe how Azure Functions scale to meet business needs

For Azure Functions
: you develop orchestrations by writing code and using the Durable Functions extension

For Logic Apps
: you create orchestrations by using a GUI or editing configuration files.

WebJobs/WebJobs SDK (vs Azure Functions)
  * No scaling
  * No Dev/Test in browser
  * No integration with Logic Apps
  * No http or Azure Event Grid triggers

| Plan | Benefits | Max # Instances |
| ---- | ---- | --- |
|Consumption plan |	This is the default hosting plan. It scales automatically and you only pay for compute resources when your functions are running. Instances of the Functions host are dynamically added and removed based on the number of incoming events. | Event Driven. Windows: 200, Linux: 100 |
| Premium plan | Automatically scales based on demand using pre-warmed workers, which run applications with no delay after being idle, runs on more powerful instances, and connects to virtual networks. | Event Driven. Windows : 100, Linux: 20 - 100|
| Dedicated plan | Run your functions within an App Service plan at regular App Service plan rates. Best for long-running scenarios where Durable Functions can't be used. | Manual/Autoscale 10 - 20 |
|ASE	App Service Environment | (ASE) is an App Service feature that provides a fully isolated and dedicated environment for securely running App Service apps at high scale. | manual/Autoscale 100 |
| Kubernetes (Direct or Azure Arc) |Kubernetes provides a fully isolated and dedicated environment running on top of the Kubernetes platform. | Varies by cluster |

**Timeout Duration**
| Plan| Default | Maximum |
| --- | --- | --- |
|Consumption plan |5|10|
|Premium plan| 30| Unlimited |
|Dedicated plan|30|Unlimited |

On any plan, a function app requires a general *Azure Storage account*, which supports Azure Blob, Queue, Files, and Table storage.

**Scaling behaviors**

* *Maximum instances*: A single function app only scales out to a maximum of 200 instances. A single instance may process more than one message or request at a time though, so there isn't a set limit on number of concurrent executions.
* *New instance rate*: For HTTP triggers, new instances are allocated, at most, once per second. For non-HTTP triggers, new instances are allocated, at most, once every 30 seconds.

Limit Scale-Out
: `functionAppScaleLimit` set o 0 or null for unrestricted, or 1 and max to limit.

Which of the following Azure Functions hosting plans is best when predictive scaling and costs are required? 
Dedicated plan: Dedicated plans run in App service which supports setting autoscaling rules based on predictive usage.


### Develop Azure Functions

* Explain the key components of a function and how they are structured
* Create triggers and bindings to control when a function runs and where the output is directed
* Connect a function to services in Azure
* Create a function by using Visual Studio Code and the Azure Functions Core Tools

A function contains two important pieces - your code, which can be written in various languages, and some config, the function.json file.

The function.json file defines the function's trigger, bindings, and other configuration settings. Every function has one and only one trigger. The runtime uses this config file to determine the events to monitor and how to pass data into and return data from a function execution.
```
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

The `bindings`` property is where you configure both triggers and bindings. Each binding shares a few common settings and some settings that are specific to a particular type of binding. Every binding requires the following settings:

| Property |Types|Comments|
| --- | --- | ---|
|type | string | Name of binding. For example, queueTrigger. |
| direction | string | Indicates whether the binding is for receiving data into the function or sending data from the function. For example, in or out. |
| name| string| The name that is used for the bound data in the function. For example, myQueue. |

A function app is composed of one or more individual functions that are managed, deployed, and scaled together. All of the functions in a function app share the same pricing plan, deployment method, and runtime version. Think of a function app as a way to organize and collectively manage your functions.

The host.json file contains runtime-specific configurations and is in the root folder of the function app.

Which of the following choices is required for a function to run?
A trigger defines how a function is invoked and a function must have exactly one trigger.


