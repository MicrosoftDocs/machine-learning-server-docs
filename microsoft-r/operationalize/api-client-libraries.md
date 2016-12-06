---

# required metadata
title: "Operationalization APIs | Microsoft R Server Docs"
description: "Operationalization APIs for Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
ms.custom: ""
---

# Using Client Libraries to Access APIs

While data scientists can work with R directly in an R console window or R IDE, application developers often need a different set of tools to leverage R inside applications. As an application developer integrating with these web services, typically your interest is in executing R code, not writing it. Data scientists with the R programming skills write the R code. Then, using some core APIs, this R code can be published as a Microsoft R Server-hosted analytics Web service. 

To simplify the integration of your R analytics web services, R Server provides a Swagger template that defines each [core operationalization API](https://microsoft.github.io/deployr-api-docs/9.0.1).  Additionally, another unique Swagger-based JSON file is generated each time a web service version is published to define the list of resources that are available in the REST API and the operations that can be called on those resources.

To access these RESTful APIs outside of R, use a Swagger code tool to generate an API client library that can be used in any programming language, such as .NET, C#, Java, Javascript, Python, or node.js. The API client simplifies the making of calls, encoding of data, and markup response handling on the API.    

![Swagger Workflow](../media/o16n/api-swagger-workflow.png)

### Get a Swagger Generation Tool

1. Install a Swagger code generator on your local machine. Some popular Swagger code generation tools are [Azure AutoRest](https://github.com/Azure/autorest) and [code-gen](https://github.com/swagger-api/swagger-codegen). 

1. Familiarize yourself with the tool so you can generate the API client libraries in your preferred programming language. 

<a name=clientlib-core></a>

### Get the Swagger File and Build the Core Client Library

To build a client library for the core operationalization APIs, follow these steps.

1. Get the Swagger-based JSON file. [Download `rserver-swagger-9.0.1.json`](https://microsoft.github.io/deployr-api-docs/9.0.1) to get the core operationalization API definitions.

1. Run the file through the Swagger code generator, and specify the language you want. If you were using AutoRest to generate a C# client library, it might look like this:
   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input rserver-9.0.1.json -Namespace MyNamespace
   ```

You can now provide some custom headers and make other changes before using the generated client library stub.

<a name="authentication"></a>

### Add Authentication Workflow Logic to Your Application

Keep in mind that all APIs require authentication; therefore, all users must authenticate when making an API call using the `POST /login` API or through Azure Active Directory (AAD). 

To simplify this process, bearer access tokens are issued so that users need not provide their credentials for every since call.  This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about managing these tokens.](security-access-tokens.md) 

Before you interact with the core APIs, first authenticate and get the bearer access token using [the authentication method](security-authentication.md) your administrator configured for operationalization:

+ Azure Active Directory (AAD): You'll need to pass the AAD credentials, authority, and client ID. In turn, AAD will issue the token.

+ Active Directory LDAP or Local Admin:  For these authentication methods, you must call the `POST /login` API in order to authenticate. You'll need to pass in the  `username` and `password` for the local administrator, or if Active Directory is enabled, pass the LDAP account information. In turn, R Server will issue you a [bearer/access token](security-access-tokens.md).   

Once authenticated, the user will not need to provide credentials again as long as the token is still valid.

#### CSharp Example of Azure Active Directory Authentication

```cs
DeployRClient client = new DeployRClient(new Uri("https://host:port"));

// -------------------------------------------------------------------------
// Note - Update these with your appropriate values
// -------------------------------------------------------------------------

// Address of the authority to issue token.
const string tenantId = "microsoft.com";
const string authority = "https://login.windows.net/" + tenantId;

// Identifier of the client requesting the token
const string clientId = "00000000-0000-0000-0000-000000000000";

// Secret of the client requesting the token
const string clientKey = "00000000-0000-0000-0000-00000000000";

var authenticationContext = new AuthenticationContext(authority);
var authenticationResult = await authenticationContext.AcquireTokenAsync(
       clientId, new ClientCredential(clientId, clientKey));

// Set Authorization header with `Bearer` and access-token
var headers = client.HttpClient.DefaultRequestHeaders;
var accessToken = authenticationResult.AccessToken;

headers.Remove("Authorization");
headers.Add("Authorization", $"Bearer {accessToken}");
```

#### CSharp Example of Active Directory/LDAP  Authentication

```cs
DeployRClient client = new DeployRClient(new Uri("https://host:port"));

// Authenticate using username/password
var loginRequest = new LoginRequest("LDAP_USER_NAME", "LDAP_PASSWORD");
var loginResponse = await client.LoginAsync(loginRequest);

// Set Authorization header with `Bearer` and access-token
var headers = deployRClient.HttpClient.DefaultRequestHeaders;
var accessToken = loginResponse.AccessToken;

headers.Remove("Authorization");
headers.Add("Authorization", $"Bearer {accessToken}");
```


### Interact with the APIs

Once your client library has been generated and you've build the authentication logic into your application, you can begin to interact with the core operationalization APIs. 

1. Create the client library.
   ```
   DeployRClient deployRClient = new DeployRClient(new Uri("https://dhost:port"));
   ```

Review the resulting API client stub that was generated. You can provide some custom headers and make other changes before using the generated client library stub to [authenticate and call the core APIs](#authentication).



<a name=clientlib-service></a>

### Build the Client Library for Service Consumption APIs

To build a client library used to integrate and consume a specific web service, follow these steps.

1. Retrieve the Swagger-based JSON file, `swagger.json`, for that service, which is available under `/api/{name}/{version}/swagger.json`. Using the core APIs, you can use:
   ```
   GET /api/{service-name}/{service-name}/swagger.json
   ```

   For example, to retrieve the JSON file for wersion `1.0.0` of the service named `transmission`:
   ```
   GET https://r-server-host/api/transmission/1.0.0/swagger.json
   ```

1. Run the file through the Swagger code generator, and specify the language you want.  For example, if using AutoRest to generate a C# client library to consume the `transmission` service version `1.0.0`, you might run the following:
   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input swagger.json -Namespace MyNamespace
   ```

   @@WHAT IS THIS NEXT BIT OF CODE?
   
   ```java
   using System;
   using MyNamespace;
   using MyNamespace.Models;
   
   namespace TransmissionApiExample
   {
       public class Program
       {
           public static void Main(string[] args)
           {
               var api = new Transmission(new Uri("https://r-server.contoso.com"));
               var accessToken = "{{YOUR_JWT_TOKEN}}";
               
               var headers = client.HttpClient.DefaultRequestHeaders;
               headers.Remove("Authorization");
               headers.Add("Authorization", $"Bearer {accessToken}");   
            
               InputParameters inputs = new InputParameters() { hp = 120, wt = 2.8 };
               var serviceResult = api.Manual.TransmissionAsync(inputs).Result;
          
               Console.Out.WriteLine(serviceResult.OutputParameters);
           }
       }
   }
   ```

1. Create the client library. @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
   ```
   DeployRClient deployRClient = new DeployRClient(new Uri("https://dhost:port"));
   ```

Review the resulting API client stub that was generated. You can provide some custom headers and make other changes before using the generated client library stub to [authenticate and call the service consumption APIs](#authentication).




## CSharp Client Library Example

1. Statically generate CSharp client libraries from the `rserver-9.0.1.json` swagger. [Download `rserver-swagger-9.0.1.json`](https://microsoft.github.io/deployr-api-docs/9.0.1).

   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input rserver-9.0.1.json -Namespace IO.Swagger.Client
   ```

   Notice the namespace is `IO.Swagger.Client`.

1. Use the statically-generated client library to call the operationalization APIs. 

   1. Get the following `NuGet` package dependencies and add them to your Visual Studio project. 
      + `Microsoft.Rest.ClientRuntime`
      + `Microsoft.IdentityModel.Clients.ActiveDirectory`

      Open the Package Manager Console for NuGet and add them with this command:

      ```
      PM> Install-Package Microsoft.Rest.ClientRuntime
      PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
      ```

### Import required namespace types in your source code

```csharp
// The namespace used during `AutoRest.exe -Namespace IO.Swagger.Client`
using IO.Swagger.Client;
using IO.Swagger.Client.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest
```

### Create a Client Library

```csharp
DeployRClient client = new DeployRClient(new Uri("https://host:port"));
```

### Authentication ADDING THE AUTHENTICATION WORKFLOW TO YOUR APP

Once your client library has been generated, you can begin to interact with the core operationalization APIs. Keep in mind that all APIs require authentication and therefore all users must authenticate when making an API call. To simplify this process, bearer access tokens are issues so that users need not provide their credentials for every since call. Learn more about managing these tokens.
Before you interact with the core APIs, first authenticate and get the bearer access token using the authentication method your administrator configured for operationalization.
Since all APIs require authentication, we first need to obtain our `Bearer` access token such that it can be included in ever request header:

```
GET /resource HTTP/1.1
Host: server.example.com
Authorization: Bearer mFfl_978_.G5p-4.94gM-
```

There are two ways to achieve this depending on your authentication requirements 
and workflow:

1. Authenticating with Azure Active Directory (Cloud)

```cs
DeployRClient client = new DeployRClient(new Uri("https://host:port"));

// -------------------------------------------------------------------------
// Note - Update these with your appropriate values
// -------------------------------------------------------------------------

// Address of the authority to issue token.
const string tenantId = "microsoft.com";
const string authority = "https://login.windows.net/" + tenantId;

// Identifier of the client requesting the token
const string clientId = "00000000-0000-0000-0000-000000000000";

// Secret of the client requesting the token
const string clientKey = "00000000-0000-0000-0000-00000000000";

var authenticationContext = new AuthenticationContext(authority);
var authenticationResult = await authenticationContext.AcquireTokenAsync(
       clientId, new ClientCredential(clientId, clientKey));

// Set Authorization header with `Bearer` and access-token
var headers = client.HttpClient.DefaultRequestHeaders;
var accessToken = authenticationResult.AccessToken;

headers.Remove("Authorization");
headers.Add("Authorization", $"Bearer {accessToken}");
```

2. Authenticating with Active Directory LDAP or Local Admin (On-premise)
For these authentication methods, you must call the POST /login API in order to authenticate. You'll need to pass in the username and password for the local administrator, or if Active Directory is enabled, pass the LDAP account information.


Authentication via On-prem AD requires you to call the `POST /login` API 
passing in your AD `username` and `password`.

```cs
DeployRClient client = new DeployRClient(new Uri("https://host:port"));

// Authenticate using username/password
var loginRequest = new LoginRequest("LDAP_USER_NAME", "LDAP_PASSWORD");
var loginResponse = await client.LoginAsync(loginRequest);

// Set Authorization header with `Bearer` and access-token
var headers = deployRClient.HttpClient.DefaultRequestHeaders;
var accessToken = loginResponse.AccessToken;

headers.Remove("Authorization");
headers.Add("Authorization", $"Bearer {accessToken}");
```

### Consume Core Op APIs

Once authenticated, the user will not need to provide credentials again as long as the token is still valid. You can now begin to interact with the core Op APIs

``cs
// --- Create Client ----------------------------------------------------------
DeployRClient client = new DeployRClient(new Uri("https://host:port"));

// --- Authenticate using AAD [or] on-prem AD as defined above ----------------
// ...
// ...

// --- Ready to use APIS ------------------------------------------------------

// Try creating an R Session `POST /sessions`
var createSessionResponse = client.CreateSession(
      new CreateSessionRequest("Session One"));

Console.WriteLine("Session ID: " + createSessionResponse.SessionId);
```

 