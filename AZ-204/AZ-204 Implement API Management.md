# AZ-204/AZ-204 Implement API Management

[Learn Path](https://learn.microsoft.com/en-us/training/paths/az-204-implement-api-management/){:target="_blank"}


Learn how the API Management service functions, how to transform and secure APIs, and how to create a backend API.

### API Management components

* API Gateway
  * This is the enpoint that:
    * Accepts API calls and routes them to appropriate backends
    * Verifies API keys and other credentials presented with requests
    * Enforces usage quotas and rate limits
    * Transforms requests and responses specified in policy statements
    * Caches responses to improve response latency and minimize the load on backend services
    * Emits logs, metrics, and traces for monitoring, reporting, and troubleshooting
* Management plane
  * The admin interface where you set up for API program. Use it for:
    * Provision and configure API Management service settings
    * Define or import API schema
    * Package APIs into products
    * Set up policies like quotas or transformations on the APIs
    * Get insights from analytics
    * Manage users
* Developer Portal
  * Automatically generated, fully customizable website with documentation of APIs. Developers use it for:
    * Read API documentation
    * Call an API via the interactive console
    * Create an account and subscribe to get API keys
    * Access analytics on their own usage
    * Download API definitions
    * Manage API keys

Products
: Products are how APIs are surfaced to developers. Products in API Management have one or more APIs, and are configured with a title, description, and terms of use. Products can be Open or Protected. Protected products must be subscribed to before they can be used, while open products can be used without a subscription. Subscription approval is configured at the product level and can either require administrator approval, or be autoapproved.

Groups
:  Groups are used to manage the visibility of products to developers. API Management has the following immutable system groups:
  * Administrators - Manage API Management service instances and create the APIs, operations, and products that are used by developers. Azure subscription administrators are members of this group.
  * Developers - Authenticated developer portal users that build applications using your APIs. Developers are granted access to the developer portal and build applications that call the operations of an API.
  * Guests - Unauthenticated developer portal users. They can be granted certain read-only access, like the ability to view APIs but not call them.
  
  In addition to these system groups, administrators can create custom groups or use external groups in associated Microsoft Entra tenants.

Developers
: Developers represent the user accounts in an API Management service instance. Developers can be created or invited to join by administrators, or they can sign up from the Developer portal. Each developer is a member of one or more groups, and can subscribe to the products that grant visibility to those groups.

Policies
:  Policies are a collection of statements that are executed sequentially on the request or response of an API. Popular statements include format conversion from XML to JSON and call rate limiting to restrict the number of incoming calls from a developer, and many other policies are available.

  Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise. Some policies such as the Control flow and Set variable policies are based on policy expressions.

  Policies can be applied at different scopes, depending on your needs: global (all APIs), a product, a specific API, or an API operation.

**Explore API Gateways**
An API gateway sits between clients and services. It acts as a reverse proxy, routing requests from clients to services. It may also perform various cross-cutting tasks such as authentication, SSL termination, and rate limiting. If you don't deploy a gateway, clients must send requests directly to back-end services. However, there are some potential problems with exposing services directly to clients:

It can result in complex client code. The client must keep track of multiple endpoints, and handle failures in a resilient way.
It creates coupling between the client and the backend. The client needs to know how the individual services are decomposed. That makes it harder to maintain the client and also harder to refactor services.
A single operation might require calls to multiple services.
Each public-facing service must handle concerns such as authentication, SSL, and client rate limiting.
Services must expose a client-friendly protocol such as HTTP or WebSocket. This limits the choice of communication protocols.
Services with public endpoints are a potential attack surface, and must be hardened.

**Managed and self-hosted**
API Management offers both managed and self-hosted gateways:

Managed - The managed gateway is the default gateway component that is deployed in Azure for every API Management instance in every service tier. With the managed gateway, all API traffic flows through Azure regardless of where backends implementing the APIs are hosted.

Self-hosted - The self-hosted gateway is an optional, containerized version of the default managed gateway. It's useful for hybrid and multicloud scenarios where there's a requirement to run the gateways off of Azure in the same environments where API backends are hosted. The self-hosted gateway enables customers with hybrid IT infrastructure to manage APIs hosted on-premises and across clouds from a single API Management service in Azure.

**API Management Policies**
Policies are a collection of Statements that are executed sequentially on the request or response of an API.
Policies are applied inside the gateway that sits between the API consumer and the managed API. Policy can apply changes to both the request and response (but might not). 
Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.

Policy definition is a XML document. 
```
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies>
```
Any errors cause further execution to be skipped and execution jumps to on-error. 

Policy Expressions

Unless the policy specifies otherwise, policy expressions can be used as attribute values or text values in any of the API Management policies. A policy expression is either:
* a single C# statement enclosed in @(expression), or
* a multi-statement C# code block, enclosed in @{expression}, that returns a value

Example setting header to add user ID to request. 
```
<policies>
    <inbound>
        <base />
        <set-header name="x-request-context-data" exists-action="override">
            <value>@(context.User.Id)</value>
            <value>@(context.Deployment.Region)</value>
      </set-header>
    </inbound>
</policies>
```

Example showing policies applied at different scopes, ordering by using the ```<base />``` tag. Global and policy level are both applies.
```
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

Example filtering response content.
```
<policies>
  <inbound>
    <base />
  </inbound>
  <backend>
    <base />
  </backend>
  <outbound>
    <base />
    <choose>
      <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">
        <!-- NOTE that we are not using preserveContent=true when deserializing response body stream into a JSON object since we don't intend to access it again. See details on https://learn.microsoft.com/azure/api-management/api-management-transformation-policies#SetBody -->
        <set-body>
          @{
            var response = context.Response.Body.As<JObject>();
            foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {
            response.Property (key).Remove ();
           }
          return response.ToString();
          }
    </set-body>
      </when>
    </choose>    
  </outbound>
  <on-error>
    <base />
  </on-error>
</policies>
```

**Advanced Policies**

Control Flow
: ```choose``` policy is similar to if-then-else or switch. At least one ```<when />``` element is required, ```<otherwise />``` is optional. 
```
<choose>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <otherwise>
        <!— one or more policy statements to be applied if none of the above conditions are true  -->
</otherwise>
</choose>
```

Forward Request
: The ```forward-request``` policy forwards the incoming request to the backend service specified in the request context. The backend service URL is specified in the API settings and can be changed using the set backend service policy.
  Removing this policy results in the request not being forwarded to the backend service and the policies in the outbound section are evaluated immediately upon the successful completion of the policies in the inbound section.
  ```
  <forward-request timeout="time in seconds" follow-redirects="true | false"/>
  ```

Limit Concurrency
: The ```limit-concurrency``` policy prevents enclosed policies from executing by more than the specified number of requests at any time. Upon exceeding that number, new requests will fail immediately with a *429 Too Many Requests* status code.
  ```
  <limit-concurrency key="expression" max-count="number">
        <!— nested policy statements -->
  </limit-concurrency>
  ```

Log to Event hub
: The ```log-to-eventhub``` policy sends messages to an event hub defined by a logger entity. Used for saving selected request or response information for further analysis.
  ``` 
  <log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">
    Expression returning a string to be logged
  </log-to-eventhub>
  ```

Mock Response
: The ```mock-response``` is used to mock APIs and operations. Aborts normal pipeline execution. Prefers response content examples when available. Generates sample responses from schemas when provided and examples if not. Without either no content is returned.
  ```
  <mock-response status-code="code" content-type="media type"/>
  ```

Retry 
: Executes child policies once and retries until the ```condition``` becomes false or ```count``` is exhausted.
  ```
    <retry
        condition="boolean expression or literal"
        count="number of retry attempts"
        interval="retry interval in seconds"
        max-interval="maximum retry interval in seconds"
        delta="retry interval delta in seconds"
        first-fast-retry="boolean expression or literal">
        <!-- One or more child policies. No restrictions -->
    </retry>
  ```

Return Response
: Aborts normal execution and returns either default or custom response. Default is ```200 OK``` with no body. Custom response can be specified via a context variable or policy statements.
  ```
  <return-response response-variable-name="existing context variable">
    <set-header/>
    <set-body/>
    <set-status/>
  </return-response>
  ```

#### Secure APIs using Subscriptions

Subscription scopes:
* App APIs: every API in the gateway.
* Single API - single imported API and its endpoints.
* Product - A product is a collection of one or more APIs you configure. You can assign APIs to more than one product. Products can have different access rules, usage quotas, and terms of use.

Subscription Key: unique auto-generated key that can be passed through in header or query string. Key is directly related to a subscription. Every subscription has primary and secondary keys, can be changed if needed (aka if its leaked), clients can use secondary key while primary is changed to avoid downtime.

The default header name is **Ocp-Apim-Subscription-Key**, and the default query string is **subscription-key**.

To test out your API calls, you can use the developer portal, or command-line tools, such as curl.

curl examples.
```
# header
curl --header "Ocp-Apim-Subscription-Key: <key string>" https://<apim gateway>.azure-api.net/api/path

# query string
curl https://<apim gateway>.azure-api.net/api/path?subscription-key=<key string>
```

#### Secure APIs using certificates
Certificates can be used to provide Transport Layer Security (TLS) mutual authentication between the client and the API gateway. You can configure the API Management gateway to allow only requests with certificates containing a specific thumbprint. The authorization at the gateway level is handled through inbound policies.

With TLS client authentication, the API Management gateway can inspect the certificate contained within the client request and check for properties like:
* Certificate Authority (CA) - Only allow certs signed by a given CA
* Thumbprint - Only allow certs with a given thumbprint
* Subject - Only allow certs with a given Subject
* Expiration Date - Don't allow expired certs
Not mutually exclusive - properties can be mixed.

Client certificates are signed to ensure that they are not tampered with. When a partner sends you a certificate, verify that it comes from them and not an imposter. There are two common ways to verify a certificate:
* Check who issued the certificate. If the issuer was a certificate authority that you trust, you can use the certificate. You can configure the trusted certificate authorities in the Azure portal to automate this process.
* If the certificate is issued by the partner, verify that it came from them. For example, if they deliver the certificate in person, you can be sure of its authenticity. These are known as self-signed certificates.

**Consumption Tier**
The Consumption tier in API Management is designed to conform with serverless design principals. If you build your APIs from serverless technologies, such as Azure Functions, this tier is a good fit. In the Consumption tier, you must explicitly enable the use of client certificates, which you can do on the **Custom domains** page. This step is not necessary in other tiers.

Certificate Authorization policies are created in the inbound processing policy file within the API Management gateway.

Examples checking for Specific Thumbprint and against certs uploaded to API Mangement:
```
<!-- specific thumbprint >
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

<!-- uploaded certs >
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```
