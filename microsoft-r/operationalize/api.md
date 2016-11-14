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

Applies to:  Microsoft R Server 9.0.1

The Microsoft R Server [operationalization APIs](https://microsoft.github.io/deployr-api-docs/) exposes the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications. This API is exposed by the R Server's operationalization server, a standards-based server technology capable of scaling to meet the needs of enterprise-grade deployments. With this server, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific API call? [Look in this online API reference.](https://microsoft.github.io/deployr-api-docs)</big>

<br>


## R for Application Developers

While data scientists can work with R directly in an R console window or R IDE, application developers need a different set of tools to leverage R inside applications. The API exposes **R analytics Web services**, making the full capabilities of R available to application developers on a simple yet powerful Web services API.

As an application developer integrating with Microsoft R Server hosted **Web services**, typically your interest is in executing R code, not writing it. Data scientists with R programming skills write R code. Using some core APIs, this R code can be published as a Microsoft R Server hosted analytics Web service. Once R code is exposed by R Server as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve outputs from the service. Those outputs can include R object data, R graphics output such as plots and charts, and any file data written to the working directory associated the current R session.

Each time a service is executed on the API, the service makes use of an R session that is managed by R Server on behalf of the application. 

To simplify integration of R analytics Web services using the R Server operationalization API, we provide a swagger template so that you can build your own client libraries. A major benefit of using these client libraries is that they simplify making calls, encoding data, and handling response markup on the API.

<br>


## Key Terms

+ **Web service**: 

+ **End point**: 

+ **Core APIs**: 

+ **Web service consumption APIs**: 

+ **Swagger template**: 

<br>


## Swagger-Defined APIs

In Microsoft R Server v9.0.1, the [R Server operationalization APIs](https://microsoft.github.io/deployr-api-docs/) are described by a [Swagger](http://swagger.io/) document.  Swagger is a popular API interface written in a JSON format, you can use to build client library stubs based on our core and web service management APIs. 

|Type|Description|
|---------|----------------|
|Core APIs|The master Swagger delivered with Microsoft R Server that defines the APIs used to: <br>- authenticate <br>- create R sessions and snapshots <br>- execute code <br>- upload artifacts <br>- manage Web services (publish, update, list, and delete Web services)<br>- and so on...<br><br>Run the swagger.json file through your Swagger code generator* to get a custom client library stub you can use to call these APIs.
|Web Service Consumption APIs|When a Web service is published, a unique Swagger document is generated to define the Web service such as `/services/<service-name>/v<service-version>/swagger.json`. <br><br>When a service is published using a call such as `/services/sean/v1.0.0`, a Web service endpoint such as `/api/sean/v1.0.0/` is created for the consumption of that service. Additionally, a service-specific Swagger document that defines the service is also generated, such as `/api/sean/v1.0.0/swagger.json`. You will have one Swagger for each version of the Web service. <br><br>You can run this swagger.json file through your Swagger code generator* to get a custom client library you can use to consume that specific version of the Web service.
|
_*Build your client library stubs using a Swagger code generator such as [Azure autorest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen)._

<br>

## OPEN QUESTIONS

1. where do users find the swagger doc? app developers need access to it.
1. Who is writing the swagger - how to use doc?  Sean to write, Josee to polish?? what needs to be filled out in the template?  And what needs to be fleshed out in the resulting client library stubs? 