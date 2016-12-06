---

# required metadata
title: "DeployR API"
description: "DeployR API"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
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
The core Microsoft R Server [operationalization APIs](https://microsoft.github.io/deployr-api-docs/9.0.1/) expose the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications. This REST API is exposed by the R Server's operationalization server, a standards-based server technology capable of scaling to meet the needs of enterprise-grade deployments. With this server, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific API call? [Look in this online API reference.](https://microsoft.github.io/deployr-api-docs/9.0.1/)</big>

The core REST APIs expose a wide range of R analytics services to client application developers. These APIs are described in a Swagger-based JSON document, `rserver-swagger-9.0.1.json`, which is delivered with the product. You can get access to all the core APIs for operationalization using a client library built from this Swagger-based JSON document. 

## R for Application Developers
While data scientists can work with R directly in an R console window or R IDE, application developers often need a different set of tools to leverage R inside applications. The API exposes Microsoft R Server-hosted **R analytics web services**, making the full capabilities of R available to application developers on a simple yet powerful Web services API.

As an application developer integrating with these web services, typically your interest is in executing R code, not writing it. Data scientists with the R programming skills write the R code. Then, using some core APIs, this R code can be published as a Microsoft R Server-hosted analytics Web service. 

### Accessing APIs Outside of R with Client Libraries

To simplify the integration of your R analytics web services, R Server provides a core [Swagger template](http://swagger.io/) that defines each [core operationalization API](https://microsoft.github.io/deployr-api-docs/9.0.1). In a nutshell, Swagger is a popular specification for a JSON file that describes REST APIs. 

In addition to the core Swagger template, another custom Swagger-based JSON file for that service. each time a web service version is published, an endpoint is registered, which in turn triggers the generation of another custom [Swagger](http://swagger.io/)-based JSON file for that service.  There is one unique file for each service version published. This Swagger document specifies the list of resources that are available in the REST API and the operations that can be called on those resources.

To access these core RESTful APIs and published web services outside of R, use a Swagger code generation tool to generate an API client library that can be used in any programming language, such as .NET, C#, Java, Javascript, Python, or node.js. The API client simplifies the making of calls, encoding of data, and markup response handling on the API.    

Some popular Swagger code generation tools are [Azure AutoRest](https://github.com/Azure/autorest) and [code-gen](https://github.com/swagger-api/swagger-codegen).

#### Create Client Library for Core Operationalization APIs

1. Install and familiarize yourself with a Swagger code generator.  

1. [Download the Swagger-based JSON file](https://microsoft.github.io/deployr-api-docs/9.0.1), `rserver-swagger-9.0.1.json`, containing the definitions for the core operationalization APIs. 

1. Run the file through the code generator, and specify the language you want.  For example, if using AutoRest, it might look like this:
   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input rserver-9.0.1.json -Namespace MyNamespace
   ```

1. Create the client library.
   ```
   DeployRClient deployRClient = new DeployRClient(new Uri("https://dhost:port"));
   ```

Review the resulting API client stub that was generated. You can provide some custom headers and make other changes before using the generated client library stub to authenticate and call the core APIs.


#### Configure Authentication 

Once your client library has been generated, you can begin to interact with the core operationalization APIs. 

Since all APIs require authentication, begin by obtaining a bearer access token using [the authentication method](security-authentication.md) your administrator configured for operationalization.

+ **For Azure Active Directory (Cloud)**
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

+ **For Active Directory LDAP or Local Administrator (On-premise)** 

  For these authentication methods, you must call the `POST /login` API in order to authenticate. You'll need to pass in the  `username` and `password` for the local administrator, or if Active Directory is enabled, pass the LDAP account information.

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



#### Client Library for Web Service Consumption

1. Install and familiarize yourself with a Swagger code generator. 

1. Retrieve the Swagger-based JSON file, `swagger.json`, available under `/api/{name}/{version}/swagger.json`. Using the core APIs, you can use:
   ```
   GET /api/{service-name}/{service-name}/swagger.json
   ```

   For example, to retrieve the JSON file for wersion `1.0.0` of the service named `transmission`:
   ```
   GET https://r-server-host/api/transmission/1.0.0/swagger.json
   ```

1. Run the file through the code generator, and specify the language you want.  For example, if using AutoRest to generate a C# client library to consume the `transmission` service version `1.0.0`, you might run the following:
   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input swagger.json -Namespace MyNamespace
   ```
   
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

Review the resulting API client stub that was generated. You can provide some custom headers and make other changes before using the generated client library stub to call the core APIs.

### Access APIs from R

For client applications written in **R**, you can side-step the Swagger approach altogether and exploit [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) directly to list, discover, and consume services. [Learn more in this vignette](../mrsdeploy/mrsdeploy-websrv-vignette.md).



<br>

## Core APIs for Operationalization 

The core R Server operationalization APIs can be grouped into several categories, including: [Authentication](#authentication), [Web Services](#services), [Sessions](#sessions), [Snapshots](#snapshots), and [Status](#status). 

<a name="authentication"></a>
### Authentication APIs

All operationalization API calls must be authenticated using the `POST /login` API or [through Azure Active Directory or Active Directory/LDAP](security-authentication.md). 

If you use the `POST /login` API, you'll get the [bearer/access token](security-access-tokens.md). This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's core operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. 

Other authentication APIs enable you to refresh or revoke the token. [Learn about each authentication API](https://microsoft.github.io/deployr-api-docs/9.0.1/#authentication-apis).

<a name="services"></a>
### Web Services APIs

The core Web Services APIs facilitate the publishing and management of user-defined analytic web services. An R Server web service is a stateless execution in an R shell on the compute node. Each web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. 

These RESTful APIs for web services provide programmatic access to a service's lifecycle. The APIs are straightforward, easy-to-use, and when composed together, can form the foundation for more expressive operations. [Learn about each web service API](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis).

Web services can be versioned to improve the release of services for service authors and make it easier for consumers to identify the services they are calling.  Service versioning is an important part of the publishing process and is designed to be flexible without naming restrictions. We highly recommend the adoption of meaningful and standard versioning conventions such as [semantic versioning](http://semver.org/).

If a web service is published without a version (<code>GET /services/{name}</code>), a Globally Unique Identifier (GUID) will be created as a unique reference number to represent a version. Since the GUID is not a meaningful version number, the intention is allow an author to treat the service as a <i>development</i> version (seen only by the author) until it is ready to be promoted and shared. From there, a more meaningful version number can be chosen during service publishing to represent a stable release.

Whenever a web service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom <a href="http://swagger.io/">Swagger</a>-based JSON file. <a href="service-integration.md#clientlib">Learn more about consuming web services</a>.

If you use a snapshot when developing your web service, the session snapshot information will be baked into the service API when published and available to any authenticated user consuming that web service later.

Please note that while only the web service author can update, publish, or delete a service, any authenticated user can retrieve a list of services or consume a service. 


<a name="sessions"></a>
### Session APIs

A session is an interactive stateful R session that is run in an R shell on the compute node. A session is managed by R Server on behalf of the application. 

The session APIs can be divided into the following groups:
+ Session lifecycle APIs help manage the R session lifecycle.
+ Session workspace APIs help manage the objects in your workspace.
+ Session working directory APIs help manage the files in your workspace.
+ Session snapshot APIs help create and manage session snapshots. More snapshot APIs are covered in the next section.
[Learn about each of these session API](https://microsoft.github.io/deployr-api-docs/9.0.1/#session-apis).


<a name="snapshots"></a>
### Snapshot APIs

If you need a prepared environment for remote script execution that includes any of the following: R packages, R objects and data files, consider creating a **snapshot**. A snapshot is an image of a R session saved to Microsoft R Server, which includes:
+ The session's workspace along with the installed R packages
+ Any files and artifacts in the working directory

>For optimal performance, consider the size of the snapshot carefully especially when publishing a service. Before creating a snapshot, ensure that you keep only those workspace objects you need and purge the rest.  And, in the event that you only need a single object, consider passing that object alone itself instead of using a snapshot.

A snapshot can be loaded into any subsequent remote R session for the user who created it.  For example, suppose you want to execute a script that needs three R packages, a reference data file, and a model object.   Instead of loading these items each time you want to execute the script, create a snapshot of an R session containing them. Then, you can save time later by loading this snapshot using its ID to get the session contents exactly as they were at the time the snapshot was created. 

[Learn about each snapshot API](https://microsoft.github.io/deployr-api-docs/9.0.1/#snapshot-apis) used to manage to create and manage session snapshots. There are a few more snapshot APIs under the [session group](#sessions).


<a name="status"></a>
### Status API

You can retrieve the 'raw details' on the health of the system using the `GET /status` API call. [Learn about this API](https://microsoft.github.io/deployr-api-docs/9.0.1/#status-apis).



## Service Consumption APIs for Operationalization
Once R code is exposed by R Server as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve outputs from the service. Those outputs can include R object data, R graphics output such as plots and charts, and any file data written to the working directory associated the current R session.

To simplify the integration of R analytics web services using these APIs, you can build and use [an API client library stub](#swagger).
