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

# Build and Use API Client Libraries from Swagger 

While data scientists can work with R directly in an R console window or R IDE, application developers often need a different set of tools to leverage R inside applications. As an application developer integrating with these web services, typically your interest is in executing R code, not writing it. Data scientists with the R programming skills write the R code. Then, using some core APIs, this R code can be published as a Microsoft R Server-hosted analytics Web service. 

To simplify the integration of your R analytics web services, R Server provides a Swagger template that defines each [core operationalization API](https://microsoft.github.io/deployr-api-docs/9.0.1).  Additionally, another unique Swagger-based JSON file is generated each time a web service version is published to define the list of resources that are available in the REST API and the operations that can be called on those resources.

To access these RESTful APIs outside of R, use a Swagger code tool to generate an API client library that can be used in any programming language, such as .NET, C#, Java, Javascript, Python, or node.js. The API client simplifies the making of calls, encoding of data, and markup response handling on the API.    

## Swagger Workflow

![Swagger Workflow](../media/o16n/api-swagger-workflow.png)

### Get a Swagger Generation Tool

1. Install a Swagger code generator on your local machine. Some popular Swagger code generation tools are [Azure AutoRest](https://github.com/Azure/autorest) and [code-gen](https://github.com/swagger-api/swagger-codegen). 

1. Familiarize yourself with the tool so you can generate the API client libraries in your preferred programming language. 

### Get the Swagger File

Get the Swagger-based JSON file for the APIs you need.

   + For Core APIs: [Download `rserver-swagger-9.0.1.json`](https://microsoft.github.io/deployr-api-docs/9.0.1). 

   + For Service Consumption APIs: Get the `swagger.json` for the service you want to consume:
     ```
     GET /api/{service-name}/{service-name}/swagger.json
     ```

### Build the Core Client Library

To build a client library, run the file through the Swagger code generator, and specify the language you want. If you were using AutoRest to generate a C# client library, it might look like this:
   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input rserver-9.0.1.json -Namespace MyNamespace
   ```

You can now provide some custom headers and make other changes before using the generated client library stub.

<a name="authentication"></a>

### Add Authentication Workflow Logic to Your Application

Keep in mind that all APIs require authentication; therefore, all users must authenticate when making an API call using the `POST /login` API or through Azure Active Directory (AAD). 

To simplify this process, bearer access tokens are issued so that users need not provide their credentials for every since call.  This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about managing these tokens.](security-access-tokens.md) 

Before you interact with the core APIs, first authenticate and get the bearer access token using [the authentication method](security-authentication.md) your administrator configured for operationalization:


+ **Azure Active Directory (AAD)**

  You'll need to pass the AAD credentials, authority, and client ID. In turn, AAD will issue the token.

  Here's an example of Azure Active Directory authentication in CSharp:

  ```
  <swagger-client-title> client = new <swagger-client-title>(new Uri("https://<host>:<port>"));
  
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

+ **Active Directory LDAP or Local Admin** 

  For these authentication methods, you must call the `POST /login` API in order to authenticate. You'll need to pass in the  `username` and `password` for the local administrator, or if Active Directory is enabled, pass the LDAP account information. In turn, R Server will issue you a [bearer/access token](security-access-tokens.md). Once authenticated, the user will not need to provide credentials again as long as the token is still valid.

  Here's an example of Active Directory/LDAP authentication in CSharp:

  ```
  <swagger-client-title> client = new <swagger-client-title>(new Uri("https://<host>:<port>"));

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

```
<swagger-client-title> client = new <swagger-client-title>(new Uri("https://<host>:<port>"));
```

<a name="clientlib-core"></a>

## Example: Core Client Library from Swagger (CSharp)

This example shows how you can use the `rserver-9.0.1.json` swagger file to build a client library to interact with the core operationalization APIs from your application.  

Build and use a core client library from swagger in CSharp and Azure Active Directory authentication:

1. [Download `rserver-swagger-9.0.1.json`](https://microsoft.github.io/deployr-api-docs/9.0.1).

1. Build the statically generated client library files for CSharp from the `rserver-9.0.1.json` swagger. 

   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input rserver-9.0.1.json -Namespace IO.Swagger.Client
   ```

   Notice the language is `CSharp` and the namespace is `IO.Swagger.Client`.

1. Use the statically-generated client library files to call the operationalization APIs. 

   1. In Visual Studio, add the following `NuGet` package dependencies to your VS project. 
      + `Microsoft.Rest.ClientRuntime`
      + `Microsoft.IdentityModel.Clients.ActiveDirectory`

      Open the Package Manager Console for NuGet and add them with this command:

      ```
      PM> Install-Package Microsoft.Rest.ClientRuntime
      PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
      ```

    1. In your application code, import the required namespace types:

       ```
       // The namespace used during `AutoRest.exe -Namespace IO.Swagger.Client`
       using IO.Swagger.Client;
       using IO.Swagger.Client.Models;
       
       using Microsoft.IdentityModel.Clients.ActiveDirectory;
       using Microsoft.Rest
       ```

    1. In your application code, create an API client to manage the API calls: 

       ```
       // --- Create API Client ----------------------------------------------------------
       DeployRClient client = new DeployRClient(new Uri("https://rserver.contoso.com:12800"));
       ```

1. In your application code, add the authentication workflow to your application.  

   Since all APIs require authentication, we first need to obtain our `Bearer` access token such that it can be included in every request header:

   ```
   GET /resource HTTP/1.1
   Host: rserver.contoso.com
   Authorization: Bearer mFfl_978_.G5p-4.94gM-
   ```

   In our example, the organization has Azure Active Directory. Insert the following into your application code:

   ```
   // --- Create API Client ----------------------------------------------------------
   DeployRClient client = new DeployRClient(new Uri("https://rserver.contoso.com:12800"));
   

   // --- Authenticate using AAD  ----------------------------------------------------
   // Note - Update these with your appropriate values
   // Once authenticated, user won't provide credentials again until token is invalid. 
   // You can now begin to interact with the core Op APIs
   // --------------------------------------------------------------------------------

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

1. Begin consuming the core operationalization APIs.
   ```
   // --------------------------------------------------------------------------------
   // Once authenticated, no need for credentials again until token expires. 

   // --- Ready to use APIS ------------------------------------------------------

   // Try creating an R Session `POST /sessions`
   var createSessionResponse = client.CreateSession(
         new CreateSessionRequest("Session One"));
   
   Console.WriteLine("Session ID: " + createSessionResponse.SessionId);
   ```

<a name=clientlib-service></a>

## Example: Service Consumption Client Library from Swagger
