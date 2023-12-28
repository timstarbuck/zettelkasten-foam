# AZ-204 Implement Azure App Service web apps

[Learn path](https://learn.microsoft.com/en-us/training/paths/create-azure-app-service-web-apps/){:target="_blank"}

## Implement Azure App Service web apps

### Explore Azure App Service

* Describe Azure App Service key components and value.
  * Baked into Azure App Service is the ability to scale up/down or scale out/in.
  * Continuous integration/deployment support
  * Deployment slots
  * App Service can also host web apps natively on Linux for supported application stacks.
  * Supported Languages: Supported languages and frameworks include: Node.js, Java (JRE 8 & JRE 11), PHP, Python, .NET, and Ruby.
  * Custom containers are also supported.
  * App Service Plans
    * Shared compute: Free and Shared
    * Dedicated Compute: Basic, Standard, Premium - V2 - V3 = Dedicated VMs
    * Isolated: Isolated - V2 - Dedicated VMs on dedicated networks
    * Isolate your app into a new App Service plan when:
      * The app is resource-intensive.
      * You want to scale the app independently from the other apps in the existing plan.
      * The app needs resource in a different geographical region.
* Explain how Azure App Service manages authentication and authorization.
* Identify methods to control inbound and outbound traffic to your web app.
* Deploy an app to App Service using Azure CLI commands.

Built in App Service Authentication
: The built-in authentication feature for App Service and Azure Functions can save you time and effort by providing out-of-the-box authentication with federated identity providers, allowing you to focus on the rest of your application.
  * Azure App Service allows you to integrate various auth capabilities into your web app or API without implementing them yourself.
  * It’s built directly into the platform and doesn’t require any particular language, SDK, security expertise, or code.
  * You can integrate with multiple login providers. For example, Microsoft Entra ID, Facebook, Google, Twitter.
  * Identity providers  
    * Microsoft Identity Platform `/.auth/login/aad`
    * Facebook `/.auth/login/facebook`
    * Google `/.auth/login/google`
    * Twitter `/.auth/login/twitter`
    * Any OpenID Connect `/.auth/login/<provider name>`
    * GitHub `/.auth/login/github`
  * How it works
    * Authenticates users and clients with the specified identity provider(s)
    * Validates, stores, and refreshes OAuth tokens issued by the configured identity provider(s)
    * Manages the authenticated session
    * Injects identity information into HTTP request headers
  * Authorization Behavior
    * Allow unauthenticated requests - i.e. have an aread of the site publically available
    * Require Auth - Rejects any unauthenticated traffic, i.e. `/.auth/login<provider`


### Configure web app settings

* Create application settings that are bound to deployment slots.
* Explain the options for installing SSL/TLS certificates for your app.
* Enable diagnostic logging for your app to aid in monitoring and debugging.
* Create virtual app to directory mappings.

App settings are always encrypted when stored (encrypted-at-rest).

==In a default, or custom, Linux container any nested JSON key structure in the app setting name like ApplicationInsights:InstrumentationKey needs to be configured in App Service as ApplicationInsights__InstrumentationKey for the key name. In other words, any : should be replaced by __ (double underscore).==

Path mappings
* Windows apps (uncontainerized)
  * Each app has the default root path (/) mapped to D:\home\site\wwwroot, where your code is deployed by default.
* Linux and containerized apps
  *  Select New Azure Storage Mount and configure your custom storage

| Option | Description |
| ---- | --------- |
| Create a free App Service managed certificate |	A private certificate that's free of charge and easy to use if you just need to secure your custom domain in App Service.|
|Purchase an App Service certificate |A private certificate that's managed by Azure. It combines the simplicity of automated certificate management and the flexibility of renewal and export options.|
| Import a certificate from Key Vault	| Useful if you use Azure Key Vault to manage your certificates.|
|Upload a private certificate	| If you already have a private certificate from a third-party provider, you can upload it.| 
| Upload a public certificate	| Public certificates aren't used to secure custom domains, but you can load them into your code if you need them to access remote resources.|

Private Cert requirements
* Exported as a password-protected PFX file, encrypted using triple DES.
* Contains private key at least 2048 bits long
* Contains all intermediate certificates in the certificate chain
To secure a custom domain in a TLS binding, the certificate has other requirements:
* Contains an Extended Key Usage for server authentication (OID = 1.3.6.1.5.5.7.3.1)
* Signed by a trusted certificate authority

Managed Certs
: a TLS/SSL server certificate that's fully managed by App Service and renewed continuously and automatically in six-month increments, 45 days before expiration.
  * Doesn't support wildcard certificates.
  * Doesn't support usage as a client certificate by using certificate thumbprint, which is planned for deprecation and removal.
  * Doesn't support private DNS.
  * Isn't exportable.
  * Isn't supported in an App Service Environment (ASE).
  * Only supports alphanumeric characters, dashes (-), and periods (.).

Which of the following types of application logging is supported on the Linux platform? 
Deployment Logging.


### Scale apps in Azure App Service

* Identify scenarios for which autoscaling is an appropriate solution.
* Create autoscaling rules for a web app.
* Monitor the effects of autoscaling.

Autoscaling 
: is a cloud system or process that adjusts available resources based on the current demand. Autoscaling performs scaling in and out, as opposed to scaling up and down.

Autoscaling 
: responds to changes in the environment by adding or removing web servers and balancing the load between them. Autoscaling doesn't have any effect on the CPU power, memory, or storage capacity of the web servers powering the app, it only changes the number of web servers.

Autoscale conditions
: * Scale based on a metric, such as the length of the disk queue, or the number of HTTP requests awaiting processing.
  * Scale to a specific instance count according to a schedule. For example, you can arrange to scale out at a particular time of day, or on a specific date or day of the week. You also specify an end date, and the system scales back in at this time.

Metrics for Autoscale rules
* CPU Percentage. 
  * This metric is an indication of the CPU utilization across all instances. A high value shows that instances are becoming CPU-bound, which could cause delays in processing client requests.
* Memory Percentage. 
  * This metric captures the memory occupancy of the application across all instances. A high value indicates that free memory could be running low, and could cause one or more instances to fail.
* Disk Queue Length. 
  * This metric is a measure of the number of outstanding I/O requests across all instances. A high value means that disk contention could be occurring.
* Http Queue Length. 
  * This metric shows how many client requests are waiting for processing by the web app. If this number is large, client requests might fail with HTTP 408 (Timeout) errors.
* Data In. 
  * This metric is the number of bytes received across all instances.
* Data Out. 
  * This metric is the number of bytes sent by all instances.

Autoscale Best Practices
* Ensure the maximum and minimum values are different and have an adequate margin between them
* Choose the appropriate statistic for your diagnostics metric
* Choose the thresholds carefully for all metric types
* Multiple rules
  * On scale-out, autoscale runs if any rule is met. 
  * On scale-in, autoscale require all rules to be met.
* Always select a safe default instance count
* Configure autoscale notifications - Use activity log alert


### Explore Azure App Service deployment slots
* Describe the benefits of using deployment slots.
* Understand how slot swapping operates in App Service.
* Perform manual swaps and enable auto swap.
* Route traffic manually and automatically.

Staging slot benefits
* You can validate app changes in a staging deployment slot before swapping it with the production slot.
* Deploying an app to a slot first and swapping it into production makes sure that all instances of the slot are warmed up before being swapped into production. This eliminates downtime when you deploy your app. The traffic redirection is seamless, and no requests are dropped because of swap operations. You can automate this entire workflow by configuring auto swap when pre-swap validation isn't needed.
* After a swap, the slot with previously staged app now has the previous production app. If the changes swapped into the production slot aren't as you expect, you can perform the same swap immediately to get your "last known good site" back.

When swapping slots
* Slot-specific settings, conn strings, continous deployment and auth settings are applied.
  * Any of these trigger restarts, if swap with preview is used the operation is paused.
* Waits for every instance to restart, if any fail to restart the swap operation reverts changes.
* If local cache is enabled, it is triggered by making http request to the root (/) on each instance and waits for http response, Causes another restart on each instance.
* If custom warm-up is enabled, triggers Application Initiation bu making HTTP request to each instance.
* If successful, swap the slots by switching the routing rules for the two slots.
* Source slot now has former target slot, same operations are performed by applying settings and restarting instances.

Specify Custom Warm Upload
web.config - `applicationInitialization`
```
<system.webServer>
    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostName="[app hostname]" />
    </applicationInitialization>
</system.webServer>
```
App Settings:
* `WEBSITE_SWAP_WARMUP_PING_PATH` Default is /
* `WEBSITE_SWAP_WARMUP_PING_STATUSES` comma-seperated list of http status codes, by default all responses are valid
* `WEBSITE_WARMUP_PATH`

Specify Percentage for slots
Client is pinned to a slot, http header `x-ms-routing-name=staging` or prod slot: `x-ms-routing-name=self`
Manually route traffic
* Use a link with x-ms-routing-name  `<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>`

Some configuration elements follow the content across a swap (not slot specific), whereas other configuration elements stay in the same slot after a swap (slot specific). Which of the following settings are swapped?
* WebJobs content
