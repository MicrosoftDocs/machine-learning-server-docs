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


<a name="consume"></a>

## Creating API Client Libraries to Consume Web Services

A web service can be published using the APIs directly (`/services/{name}/{version}`) or using the `mrsdeploy` package functions. Whenever a service is published (`POST /services/{name}/{version}`), an endpoint is registered (`/api/{name}/{version}`), which in turn triggers the generation of a custom [Swagger](http://swagger.io/)-based JSON file.  

This Swagger-based JSON file contains the definitions of every API specific to that service version, including the [authentication APIs](api.md#authentication), needed to interact with that specific version. 

Using a Swagger code generation tool such as  [Azure AutoRest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen), you can convert the Swagger-based file into an API client. The client  simplifies the way in which you consume this published web service, simplifing the making of calls, encoding of data, and markup response handling on the API.  

**To build your client libraries:**

1. Find the Swagger-based JSON file, `swagger.json`, available under `/api/{name}/{version}/swagger.json`. Using the core APIs, you can use `GET /api/{name}/{version}/swagger.json`.

1. Install a Swagger code generator such as [Azure autorest](https://github.com/Azure/autorest).

1. Run the file through the code generator, and specify the language you want. 

1. Review the resulting API client stub that was generated. You can provide some custom headers and make other changes.

Now, you are ready to call the APIs needed to authenticate and consume the service.