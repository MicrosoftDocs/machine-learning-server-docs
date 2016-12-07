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

The Microsoft R Server [operationalization REST APIs](https://microsoft.github.io/deployr-api-docs/9.0.1/) are exposed by R Server's operationalization server, a standards-based server technology [capable of scaling to meet the needs of enterprise-grade deployments](configuration-initial.md#enterprise). With the operationalization feature configured, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific core API call? [Look in this online API reference.](https://microsoft.github.io/deployr-api-docs/9.0.1/)</big>

The APIs available for operationalization with R Server can be categorized into two groups: Core APIs and the Service Consumption APIs.

## Core Operationalization APIs

These core REST APIs expose the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications.  These APIs enable you to publish Microsoft R Server-hosted **R analytics web services**, making the full capabilities of R available to application developers on a simple yet powerful Web services API. The core R Server operationalization APIs can be grouped into several categories as shown in this table. 

Core API Type|Description|Reference Help
---------|-----------|---------------
Authentication|These APIs provide authentication related operations and access workflow features.|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#authentication-apis)
Web Services|These APIs facilitate the publishing and management of user-defined analytic web services (create, delete, update, list, discover). Each web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. When a service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered and a [custom Swagger-based JSON file is generated](#clientlib-service).|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis)
Session|These APIs provide functionality for R session management (create, delete, update, list, console output, history, and workspace and working directory files)|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#session-apis)
Snapshot|These APIs provide different operations to access and manage workspace snapshots. A snapshot is a prepared environment image of a R session saved to Microsoft R Server, which includes the session's R packages, R objects and data files. This snapshot can be loaded into any subsequent remote R session for the user who created it. | [Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#snapshot-apis)
Status|This API allows you to retrieve the 'raw details' on the health of the system.|[Help](https://microsoft.github.io/deployr-api-docs/9.0.1/#status-apis)

<br>

The core APIs are accessible from and described in  `rserver-swagger-9.0.1.json`, **a Swagger-based JSON document**. This document can be downloaded directly from the [core API reference](https://microsoft.github.io/deployr-api-docs/9.0.1)  page. [Swagger](http://swagger.io/) is a popular specification for a JSON file that describes REST APIs.  

You access all these core APIs using a client library built from `rserver-swagger-9.0.1.json` using [these instructions and example](api-client-libraries.md).

Note: For client applications written in **R**, you can side-step the Swagger approach altogether and exploit [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) directly to list, discover, and consume services. [Learn more in this vignette](../mrsdeploy/mrsdeploy-websrv-vignette.md).

## Service Consumption APIs

The service consumption REST APIs expose a wide range of R analytics services to client application developers.   Once R code is published and exposed by R Server as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve R session outputs (R objects, graphics, files, ...) from the service.  

Whenever a web service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom Swagger-based JSON file. This swagger file describes each API needed to interact with that service. 

While the service consumption Swagger files are all named `swagger.json`, you'll find them each in a unique location by calling `GET /api/{service-name}/{service-name}/swagger.json`.  

To simplify the integration of R analytics web services using these APIs, build and use an API client library stub from the `swagger.json` file for each web service using [these instructions and example](api-client-libraries.md).
