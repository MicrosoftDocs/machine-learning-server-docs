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

# R Server Operationalization APIs

**Applies to:  Microsoft R Server 9.0.1**

## Introduction
The Microsoft R Server [operationalization REST APIs](https://microsoft.github.io/deployr-api-docs/9.0.1/) are exposed by R Server's operationalization server, a standards-based server technology [capable of scaling to meet the needs of enterprise-grade deployments](configuration-initial.md#enterprise). With the operationalization feature configured, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific API call? [Look in this online API reference.](https://microsoft.github.io/deployr-api-docs/9.0.1/)</big>

The APIs available for operationalization with R Server can be categorized into two groups:

### Core Operationalization APIs

These core REST APIs expose the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications.  These APIs enable you to publish Microsoft R Server-hosted **R analytics web services**, making the full capabilities of R available to application developers on a simple yet powerful Web services API. 

The core APIs are described in  `rserver-swagger-9.0.1.json`, a Swagger-based JSON document available from [the core API reference page](https://microsoft.github.io/deployr-api-docs/9.0.1). [Swagger](http://swagger.io/) is a popular specification for a JSON file that describes REST APIs.  You access all the core APIs for operationalization using a client library built from `rserver-swagger-9.0.1.json` using the [instructions](#clientlib-core) later in this article.

For client applications written in **R**, you can side-step the Swagger approach altogether and exploit [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) directly to list, discover, and consume services. [Learn more in this vignette](../mrsdeploy/mrsdeploy-websrv-vignette.md).

The core R Server operationalization APIs can be grouped into several categories as shown in this table. 


Core API Type|Description|Reference Help
---------|-----------|---------------
Authentication|These APIs provide authentication related operations and access workflow features.|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#authentication-apis)
Web Services|These APIs facilitate the publishing and management of user-defined analytic web services (create, delete, update, list, discover). Each web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. When a service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered and a [custom Swagger-based JSON file is generated](#clientlib-service).|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis)
Session|These APIs provide functionality for R session management (create, delete, update, list, console output, history, and workspace and working directory files)|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#session-apis)
Snapshot|These APIs provide different operations to access and manage workspace snapshots. A snapshot is a prepared environment image of a R session saved to Microsoft R Server, which includes the session's R packages, R objects and data files. This snapshot can be loaded into any subsequent remote R session for the user who created it. | [Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#snapshot-apis)
Status|This API allows you to retrieve the 'raw details' on the health of the system.|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#status-apis)



### Service Consumption APIs

The service consumption REST APIs expose a wide range of R analytics services to client application developers.   Once R code is published and exposed by R Server as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve R session outputs (R objects, graphics, files, ...) from the service.  

Each time a service is published, a unique Swagger-based JSON document is automatically generated to describe each API needed to interact with that service. While the service consumption Swagger files are all named `swagger.json`, you'll find them each in a unique location by calling `GET /api/{service-name}/{service-name}/swagger.json`.  To simplify the integration of R analytics web services using these APIs, build and use [an API client library stub](#clientlib-service).


## Using Client Libraries to Access APIs

While data scientists can work with R directly in an R console window or R IDE, application developers often need a different set of tools to leverage R inside applications. As an application developer integrating with these web services, typically your interest is in executing R code, not writing it. Data scientists with the R programming skills write the R code. Then, using some core APIs, this R code can be published as a Microsoft R Server-hosted analytics Web service. 

To simplify the integration of your R analytics web services, R Server provides a Swagger template that defines each [core operationalization API](https://microsoft.github.io/deployr-api-docs/9.0.1).  Additionally, another unique Swagger-based JSON file is generated each time a web service version is published to define the list of resources that are available in the REST API and the operations that can be called on those resources.

To access these RESTful APIs outside of R, use a Swagger code tool to generate an API client library that can be used in any programming language, such as .NET, C#, Java, Javascript, Python, or node.js. The API client simplifies the making of calls, encoding of data, and markup response handling on the API.    

![Swagger Workflow](../media/o16n/api-swagger-workflow.png)

### Get a Swagger Generation Tool

1. Install a Swagger code generator on your local machine. Some popular Swagger code generation tools are [Azure AutoRest](https://github.com/Azure/autorest) and [code-gen](https://github.com/swagger-api/swagger-codegen). 

1. Familiarize yourself with the tool so you can generate the API client libraries in your preferred programming language. 

<a name=clientlib-core></a>

### Build the Client Library for Core APIs

To build a client library for the core operationalization APIs, follow these steps.

1. Get the Swagger-based JSON file. [Download `rserver-swagger-9.0.1.json`](https://microsoft.github.io/deployr-api-docs/9.0.1) to get the core operationalization API definitions.

1. Run the file through the Swagger code generator, and specify the language you want.  if using AutoRest to generate a C# client library, it might look like this:
   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input rserver-9.0.1.json -Namespace MyNamespace
   ```

1. Create the client library.
   ```
   DeployRClient deployRClient = new DeployRClient(new Uri("https://dhost:port"));
   ```

Review the resulting API client stub that was generated. You can provide some custom headers and make other changes before using the generated client library stub to [authenticate and call the core APIs](#auth).

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

Review the resulting API client stub that was generated. You can provide some custom headers and make other changes before using the generated client library stub to [authenticate and call the service consumption APIs](#auth).

<a name=auth></a>

### Adding Authentication Logic to your Application  

Once your client library has been generated, you can begin to interact with the core operationalization APIs. Keep in mind that all APIs require authentication; therefore, all users must authenticate when making an API call using the `POST /login` API or through Azure Active Directory. 

To simplify this process, bearer access tokens are issued so that users need not provide their credentials for every since call.  This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about managing these tokens.](security-access-tokens.md) 

Before you interact with the core APIs, first authenticate and get the bearer access token using [the authentication method](security-authentication.md) your administrator configured for operationalization.

+ **Authenticating with Azure Active Directory (Cloud)**

  For Azure Active Directory (AAD), you'll need to pass the AAD credentials and, in turn, AAD will issue the token.

    ```
    csharp
    public static async Task<DeployRClient> HandleLoginAzureActiveDirectory(DeployRClient deployRClient)
    { 

    // Enter the URL to the AAD login as the authority. 
    // For example, if the AAD domain is `myMRServer.contoso.com`, 
    // then the `Authority` would be `https://login.windows.net/myMRSServer.contoso.com`
    const string authority = "https://login.windows.net/myMRSServer.contoso.com";

    // Enter the identifier of the client requesting the token.
    // This is the `CLIENT ID` value for the web app in the Azure management portal.
    // Ask your administrator for more information.
    const string clientId = "00000000-0000-0000-0000-000000000000";

    // Enter the secret of the client requesting the token.
    const string clientKey = "00000000-0000-0000-0000-00000000000";

    var authenticationContext = new AuthenticationContext(authority);
    var authenticationResult = authenticationContext.AcquireTokenAsync(
    clientId, new ClientCredential(clientId, clientKey));
           // Set Authorization header with `Bearer` and access-token
           var headers = deployRClient.HttpClient.DefaultRequestHeaders;
           var accessToken = authenticationResult.Result.AccessToken;

    headers.Remove("Authorization");
    headers.Add("Authorization", $"Bearer {accessToken}");

    return deployRClient;
    }
    ```

+ **Authenticating with Active Directory LDAP or Local Admin (On-premise)** 

  For these authentication methods, you must call the `POST /login` API in order to authenticate. You'll need to pass in the  `username` and `password` for the local administrator, or if Active Directory is enabled, pass the LDAP account information. In turn, R Server will issue you a [bearer/access token](security-access-tokens.md).   

    ```
    public static async Task<DeployRClient> HandleLoginOnPremisesActiveDirectory(DeployRClient deployRClient)
    {
        try
        {  
            // Authenticate using username/password
            var loginRequest = new LoginRequest("<USER_NAME>", "<PASSWORD>");
            var loginResponse = await deployRClient.LoginAsync(loginRequest);

       // Set Authorization header with `Bearer` and access-token
        var headers = deployRClient.HttpClient.DefaultRequestHeaders;
        var accessToken = loginResponse.AccessToken;

       headers.Remove("Authorization");
       headers.Add("Authorization", $"Bearer {accessToken}");

            return deployRClient;
        }
        catch (HttpOperationException httpOperationException)
        {
            httpOperationException.Response.ThrowIfInternalServerError();
        }

        throw new InvalidOperationException("Unable to authenticate with Active Directory.");
    }
    ``` 

Once authenticated, the user will not need to provide credentials again as long as the token is still valid. 