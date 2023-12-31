#AZ-204 Implement user authentication and authorization

### Explore the Microsoft Identity Platform

There are several components that make up the Microsoft identity platform:

* OAuth 2.0 and OpenID Connect standard-compliant authentication service enabling developers to authenticate several identity types, including:
  * Work or school accounts, provisioned through Microsoft Entra ID
  * Personal Microsoft account, like Skype, Xbox, and Outlook.com
  * Social or local accounts, by using Azure Active Directory B2C
  * Social or local customer accounts, by using Microsoft Entra External ID
* Open-source libraries: Microsoft Authentication Libraries (MSAL) and support for other standards-compliant libraries
* Microsoft identity platform endpoint: Works with the Microsoft Authentication Libraries (MSAL) or any other standards-compliant library. It implements human readable scopes, in accordance with industry standards.
* Application management portal: A registration and configuration experience in the Azure portal, along with the other Azure management capabilities.
* Application configuration API and PowerShell: Programmatic configuration of your applications through the Microsoft Graph API and PowerShell so you can automate your DevOps tasks.

**Explore service principals**

The **application object** is the **global representation** of your application for use across all tenants, and the **service principal** is the **local representation for use in a specific tenant**. The application object serves as the template from which common and default properties are derived for use in creating corresponding service principal objects.

An application object has:
* A one to one relationship with the software application, and
* A one to many relationships with its corresponding service principal object(s).

A service principal must be created in each tenant where the application is used to enable it to establish an identity for sign-in and/or access to resources being secured by the tenant. A single-tenant application has only one service principal (in its home tenant), created and consented for use during application registration. A multitenant application also has a service principal created in each tenant where a user from that tenant consented to its use.

**Discover permissions and consent**

The Microsoft identity platform implements the OAuth 2.0 authorization protocol. OAuth 2.0 is a method through which a third-party app can access web-hosted resources on behalf of a user. Any web-hosted resource that integrates with the Microsoft identity platform has a resource identifier, or application ID URI.

In OAuth 2.0, these types of permission sets are called scopes. They're also often referred to as permissions. In the Microsoft identity platform, a permission is represented as a string value. An app requests the permissions it needs by specifying the permission in the scope query parameter. Identity platform supports several well-defined OpenID Connect scopes and resource-based permissions (each permission is indicated by appending the permission value to the resource's identifier or application ID URI). For example, the permission string ```https://graph.microsoft.com/Calendars.Read``` is used to request permission to read users calendars in Microsoft Graph.

**Permission types**
* **Delegated permissions** - used by apps that have a signed-in user present. The app is delegated with the permission to act as a signed-in user when it makes calls to the target resource.
* **App-only access permissions** - used by apps that run wihtout a signed-in user present, i.e. background services or daemons. Only administrator can consent to app-only access permissions.

**Consent Types**
* **Static User Consent** - All necessary permissions are in the app's configuration in the Azure portal. Challenges:
  * All permissions ever needed are requested at user's first sign in. Long list can discourage user from approving app's access on initial sign-in.
  * Nees to know all the resources the app would ever need ahead of time. Difficult to know.
* **Incremental and dynamic user consent** - You can ask for a minimum set of permissions upfront and request more over time as the customer uses more app features.
  * Add new scopes to the ```scope``` parameter when requesting an access token.
  * If the user hasn't consented, they are prompted to consent to new permissions.
  * Only applies to delegated permissions and not to app-only access permissions.
  * Any admin privileged permission my be registered in the Azure portal. This enables tenant admins to consent on behalf of all their users.
* **Admin consent** -  Admin consent is required when your app needs access to certain high-privilege permissions. Admin consent ensures that administrators have some other controls before authorizing apps or users to access highly privileged data from the organization.

**Requesting individual user consent**
In an OpenID Connect or OAuth 2.0 authorization request, an app can request the permissions it needs by using the scope query parameter (space delimited).
```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

After the user enters their credentials, the Microsoft identity platform checks for a matching record of user consent. If the user hasn't consented to any of the requested permissions in the past, and if the administrator hasn't consented to these permissions on behalf of the entire organization, the Microsoft identity platform asks the user to grant the requested permissions.

**Discover conditional access**

* Multifactor authentication
* Allowing only Intune enrolled devices to access specific services
* Restricting user locations and IP ranges

For the most common cases no app changes are required for Conditional Access. The following scenarios require code to handle Conditional Access challenges:
* Apps performing the on-behalf-of flow
* Apps accessing multiple services/resources
* Single-page apps using MSAL.js
* Web apps calling a resource

**Conditional Access examples**
Some scenarios require code changes to handle Conditional Access whereas others work as is. Here are a few scenarios using Conditional Access to do multifactor authentication that gives some insight into the difference.

* You're building a single-tenant iOS app and apply a Conditional Access policy. The app signs in a user and doesn't request access to an API. When the user signs in, the policy is automatically invoked and the user needs to perform multifactor authentication.
* You're building an app that uses a middle tier service to access a downstream API. An enterprise customer at the company using this app applies a policy to the downstream API. When an end user signs in, the app requests access to the middle tier and sends the token. The middle tier performs on-behalf-of flow to request access to the downstream API. At this point, a claims "challenge" is presented to the middle tier. The middle tier sends the challenge back to the app, which needs to comply with the Conditional Access policy.

### Implement Authentication by using Microsoft Authentication library

**Explore the Microsoft Authentication Library**

MSAL Benefits
* No need to directly use the OAuth libraries or code against the protocol in your application.
* Acquires tokens on behalf of a user or on behalf of an application (when applicable to the platform).
* Maintains a token cache and refreshes tokens for you when they're close to expire. You don't need to handle token expiration on your own.
* Helps you specify which audience you want your application to sign in.
* Helps you set up your application from configuration files.
* Helps you troubleshoot your app by exposing actionable exceptions, logging, and telemetry.

**Authentication Flows**
|Flow|	Description|
|--|--|
|Authorization code|	Native and web apps securely obtain tokens in the name of the user|
|Client credentials|	Service applications run without user interaction|
|On-behalf-of|	The application calls a service/web API, which in turns calls Microsoft Graph|
|Implicit|	Used in browser-based applications|
|Device code|	Enables sign-in to a device by using another device that has a browser|
|Integrated Windows|	Windows computers silently acquire an access token when they're domain joined|
|Interactive|	Mobile and desktops applications call Microsoft Graph in the name of a user|
|Username/password|	The application signs in a user by using their username and password|

**Public client, and confidential client applications**

Public client applications
: Are apps that run on devices or desktop computers or in a web browser. They're not trusted to safely keep application secrets, so they only access web APIs on behalf of the user. (They support only public client flows.) Public clients can't hold configuration-time secrets, so they don't have client secrets.

Confidential client applications
: Are apps that run on servers (web apps, web API apps, or even service/daemon apps). They're considered difficult to access, and for that reason capable of keeping an application secret. Confidential clients can hold configuration-time secrets. Each instance of the client has a distinct configuration (including client ID and client secret).


### Implement shared access signatures

A shared access signature (SAS)
:  is a URI that grants restricted access rights to Azure Storage resources. You can provide a shared access signature to clients that you want to grant delegate access to certain storage account resources.

* Identify the three types of shared access signatures.
* Explain when to implement shared access signatures.
* Create a stored access policy.

A shared access signature (SAS) 
: is a signed URI that points to one or more storage resources and includes a token that contains a special set of query parameters. The token indicates how the resources may be accessed by the client. One of the query parameters, the signature, is constructed from the SAS parameters and signed with the key that was used to create the SAS. This signature is used by Azure Storage to authorize access to the storage resource.

Azure Storage supports three types of shared access signatures:

* **User delegation SAS**: A user delegation SAS is secured with Microsoft Entra credentials and also by the permissions specified for the SAS. A user delegation SAS applies to Blob storage only.

* **Service SAS**: A service SAS is secured with the storage account key. A service SAS delegates access to a resource in the following Azure Storage services: Blob storage, Queue storage, Table storage, or Azure Files.

* **Account SAS**: An account SAS is secured with the storage account key. An account SAS delegates access to resources in one or more of the storage services. All of the operations available via a service or user delegation SAS are also available via an account SAS.

In a single URI, such as https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D, you can separate the URI from the SAS token as follows:

* URI: https://medicalrecords.blob.core.windows.net/patient-images/patient-116139-nq8z7f.jpg?
* SAS token: sp=r&st=2020-01-20T11:42:32Z&se=2020-01-20T19:42:32Z&spr=https&sv=2019-02-02&sr=b&sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D
The SAS token itself is made up of several components.

|Component|	Description|
|--|--|
|```sp=r```	|Controls the access rights. The values can be a for add, c for create, d for delete, l for list, r for read, or w for write. This example is read only. The example ```sp=acdlrw``` grants all the available rights.|
|```st=2020-01-20T11:42:32Z```|	The date and time when access starts.|
|```se=2020-01-20T19:42:32Z```|	The date and time when access ends. This example grants eight hours of access.|
|```sv=2019-02-02```|	The version of the storage API to use.|
|```sr=b``````	|The kind of storage being accessed. In this example, b is for blob.|
|```sig=SrW1HZ5Nb6MbRzTbXCaPm%2BJiSEn15tC91Y4umMPwVZs%3D```|	The cryptographic signature.|

**Best practices**
To reduce the potential risks of using a SAS, Microsoft provides some guidance:

* To securely distribute a SAS and prevent man-in-the-middle attacks, always use HTTPS.
* The most secure SAS is a user delegation SAS. Use it wherever possible because it removes the need to store your storage account key in code. You must use Microsoft Entra ID to manage credentials. This option might not be possible for your solution.
* Try to set your expiration time to the smallest useful value. If a SAS key becomes compromised, it can be exploited for only a short time.
* Apply the rule of minimum-required privileges. Only grant the access that's required. For example, in your app, read-only access is sufficient.
* There are some situations where a SAS isn't the correct solution. When there's an unacceptable risk of using a SAS, create a middle-tier service to manage users and their access to storage.

A common scenario where a SAS is useful is a service where users read and write their own data to your storage account. In a scenario where a storage account stores user data, there are two typical design patterns:

**Choose when to use shared access signatures**
* Clients upload and download data via a front-end proxy service, which performs authentication. This front-end proxy service has the advantage of allowing validation of business rules, but for large amounts of data or high-volume transactions, creating a service that can scale to match demand may be expensive or difficult.
* A lightweight service authenticates the client as needed and then generates a SAS. Once the client application receives the SAS, they can access storage account resources directly with the permissions defined by the SAS and for the interval allowed by the SAS. The SAS mitigates the need for routing all data through the front-end proxy service.

**Stored access policies**

A stored access policy provides an extra level of control over service-level shared access signatures (SAS) on the server side. Establishing a stored access policy groups SAS and provides more restrictions for signatures that are bound by the policy. You can use a stored access policy to change the start time, expiry time, or permissions for a signature, or to revoke it after it has been issued.

The following storage resources support stored access policies:
* Blob containers
* File shares
* Queues
* Tables

The access policy for a SAS consists of the start time, expiry time, and permissions for the signature. You can specify all of these parameters on the signature URI and none within the stored access policy; all on the stored access policy and none on the URI; or some combination of the two. However, you can't specify a given parameter on both the SAS token and the stored access policy.

Example creating a access policy using the Azure Client
```
az storage container policy create 
  --name <stored access policy identifier> 
  --container-name <container name> 
  --start <start time UTC datetime> 
  --expiry <expiry time UTC datetime> 
  --permissions <(a)dd, (c)reate, (d)elete, (l)ist, (r)ead, or (w)rite> 
  --account-key <storage account key> 
  --account-name <storage account name> 
```
