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
