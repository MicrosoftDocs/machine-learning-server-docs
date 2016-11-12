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
# R Server's Operationalization API Overview

Applies to:  DeployR 9.0.1

The DeployR API exposes the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications. This API is exposed by the DeployR server, a standards-based server technology capable of scaling to meet the needs of enterprise-grade deployments.

With the advent of DeployR, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific API call? [Look in this online API reference.](https://microsoft.github.io/deployr-api-docs)</big>

##R for Application Developers

While data scientists can work with R directly in an R console window or R IDE, application developers need a different set of tools to leverage R inside applications. The DeployR API exposes **R analytics Web services**, making the full capabilities of R available to application developers on a simple yet powerful Web services API.

As an application developer integrating with **DeployR-managed analytics Web services**, typically your interest is in executing R code, not writing it. Data scientists with R programming skills write R code. Using some core APIs, this R code can be published as a DeployR-managed analytics Web service. Once R code is exposed by DeployR as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve outputs from the service. Those outputs can include R object data, R graphics output such as plots and charts, and any file data written to the working directory associated the current R session.

Each time a service is executed on the API, the service makes use of an R session that is managed by DeployR on behalf of the application. 

To simplify integration of R analytics Web services using the DeployR API, we provide a swagger template so that you can build your own client libraries. A major benefit of using these client libraries is that they simplify making calls, encoding data, and handling response markup on the API.


##Swagger SOMETHING

In DeployR 9.0, we've introduced a [Swagger](http://swagger.io/)-enabled API. Swagger is a popular API interface you can use to build client libraries for:
+ The core APIs
+ The web service consumption APIs

(aka endpoint) 


API INTRO
Our client libraries are replaced by swagger.
Swagger used: Build client libraries for the core APIs and to build client libraries for the web service (aka endpoint) consumption apis.
Swagger editor tools such as: 
 + code-gen https://github.com/swagger-api/swagger-codegen
 +  https://github.com/Azure/autorest


API DOCS ARE STILL HERE:
https://microsoft.github.io/deployr-api-docs/


publish a service like this: /services/sean/v1.0.0   (a swagger will be created - auto built for you)

GET swagger —> /api/sean/v1.0.0/swagger.json

run the swagger through your tool to get a custom client library specific to a given web service version.

then someone comes along and consumes it with this: /api/sean/v1.0.0   and creates an endpoint for your consumption



OVERVIEW;

THE APIS:
	core apis 

	Service consumption apis

SWAGGER:

	
