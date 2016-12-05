---

# required metadata
title: "Operationalization with R Server"
description: "Operationalization of R Analytics with Microsoft R Server"
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

Using a Swagger code generation tool such as  [Azure AutoRest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen), you can convert the Swagger-based file into an API client. The client  simplifies the way in which you consume this published web service, simplifing the making of calls, encoding of data, and markup response handling on the API.  

**To build your client libraries:**

1. Find the Swagger-based JSON file, `swagger.json`, available under `/api/{name}/{version}/swagger.json`. Using the core APIs, you can use `GET /api/{name}/{version}/swagger.json`.

1. Install a Swagger code generator such as [Azure autorest](https://github.com/Azure/autorest).

1. Run the file through the code generator, and specify the language you want. 

1. Review the resulting API client stub that was generated. You can provide some custom headers and make other changes.

Now, you are ready to call the APIs needed to authenticate and consume the service.