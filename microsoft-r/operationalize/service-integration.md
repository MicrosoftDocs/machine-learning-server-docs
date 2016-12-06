---

# required metadata
title: "Integrate R Analytics Web Services into Applications | Microsoft R Server Docs"
description: "Integrate R Analytics Web Services into Applications with Microsoft R Server"
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

# Integrate Web Services into Applications 

**Applies to:  Microsoft R Server 9.0.1**

@@
 + Integration Workflow

 + Tutorial: Using a simple example, walk them through process of integrating the web services produced by a data scientist into their apps.

 + How application dev integrates R analytics through the Web services. How- to use those web services. I have to authenticate with the R Server deployment server to interact with web services. Who will tell which APIs were used? What input/output can I expect? what APIs would I use? swagger

 + How to integrate it via API or  mrsdeploy functions. You can create a service using the APIs or the package to publish and consume web services. 


An R Server web service is a stateless execution in an R shell on the compute node. Each web service is uniquely defined by a `name` and `version`. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis) are available for web services and provide programmatic access to a service's lifecycle. Similarly, you can use the functions in [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) to do the same directly from an R script.

We highly recommend you use a meaningful and standard versioning convention such as [semantic versioning](http://semver.org/). Versioning improves the release of services for service authors and makes it easier for consumers to identify the services they are calling. In the event that a web service is published without a version (<code>GET /services/{name}</code>), a Globally Unique Identifier (GUID) is automatically used as a version. Since the GUID is not a meaningful version number, the intention is allow an author to treat the service as a <i>development</i> version (seen only by the author) until it is ready to be promoted and shared. From there, a more meaningful version number can be chosen during service publishing to represent a stable release.

Whenever a web service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom <a href="http://swagger.io/">Swagger</a>-based JSON file. 

Please note that while only the web service author can update, publish, or delete a service, any authenticated user can retrieve a list of services or consume a service. 
 
 
<a name="list"></a>

## Getting a List of Available Web Services

Use the List Services API to return the list of all available services, including those by other users. The code of a web service will only be visible to the web service owner who created it.

@@Get will service meta-data and then returns a new service-api instance for consumption.

Example:

+ Using APIs directly:
  ```
  GET /services/<service-name>/<service-version> 
  ```

+ Using `mrsdeploy` package functions:
  ```
  getService(<service-name>, <service-version>)
  ```


<a name="consume"></a>

## Consuming Web Services

@@

<a name="clientlib"></a>

## Creating API Client Libraries to Consume Web Services

A web service can be published using the APIs directly (`/services/{name}/{version}`) or using the [`mrsdeploy` package functions](../mrsdeploy/mrsdeploy.md). Whenever a service is published (`POST /services/{name}/{version}`), an endpoint is registered (`/api/{name}/{version}`), which in turn triggers the generation of a custom [Swagger](http://swagger.io/)-based JSON file.  

This Swagger-based JSON file contains the definitions of every API specific to that service version, including the [authentication APIs](api.md#authentication), needed to interact with that specific version. 

Using a Swagger code generation tool such as  [Azure AutoRest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen), you can convert the Swagger-based file into an API client. The client simplifies the way in which you consume this published web service, simplifing the making of calls, encoding of data, and markup response handling on the API.  

**To build your client libraries:**

1. Find the Swagger-based JSON file, `swagger.json`, available under `/api/{name}/{version}/swagger.json`. Using the core APIs, you can use:
   ```
   GET /api/{name}/{version}/swagger.json
   ```

1. Install a Swagger code generator, such as [Azure AutoRest](https://github.com/Azure/autorest).

1. Run the file through the code generator, and specify the output language you want. 

1. Review the resulting API client stub that was generated. You can provide some custom headers and make other changes.

Now, you are ready to call the APIs needed to authenticate and consume the service.






<br>

## Core APIs for Operationalization 

The core R Server operationalization APIs can be grouped into several categories, including: [Authentication](#authentication), [Web Services](#services), [Sessions](#sessions), [Snapshots](#snapshots), and [Status](#status). 

<a name="authentication"></a>
### Authentication APIs

All operationalization API calls must be authenticated using the `POST /login` API or through Azure Active Directory. 

If you use the `POST /login` API, R Server will issue you a [bearer/access token](security-access-tokens.md). This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's core operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. If your organization uses Azure Active Directory, then AAD will issue the token.

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

