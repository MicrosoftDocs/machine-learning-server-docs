---

# required metadata
title: "Operationalization APIs | Microsoft R Server Docs"
description: "Operationalization APIs for Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
ms.topic: "article"
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

# R Server Operationalization API Overview

**Applies to:  Microsoft R Server 9.x**

The Microsoft R Server <a href="https://microsoft.github.io/deployr-api-docs/" target="_blank">operationalization REST APIs</a> are exposed by R Server's operationalization server, a standards-based server technology [capable of scaling to meet the needs of enterprise-grade deployments](configure-enterprise.md). With the operationalization feature configured, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific core API call? <a href="https://microsoft.github.io/deployr-api-docs/" target="_blank">Look in this online API reference</a>.</big>

The APIs available for operationalization with R Server can be categorized into two groups: Core APIs and the Service Consumption APIs.

<a name="core"></a>

## Core Operationalization APIs

These core REST APIs expose the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications.  These APIs enable you to publish Microsoft R Server-hosted **R analytics web services**, making the full capabilities of R available to application developers on a simple yet powerful REST API. The core R Server operationalization APIs can be grouped into several categories as shown in this table. 

Core API Type|Description|Reference Help
---------|-----------|:-----:
Authentication|These APIs provide authentication related operations and access workflow features.|<a href="https://microsoft.github.io/deployr-api-docs/#authentication-apis" target="_blank">Help</a>
Web Services|These APIs facilitate the publishing and management of user-defined analytic web services (create, delete, update, list, discover). Each web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. When a service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered and a [custom Swagger-based JSON file is generated](app-developer-get-started.md).|<a href="https://microsoft.github.io/deployr-api-docs/#services-management-apis" target="_blank">Help</a>
Session|These APIs provide functionality for R session management (create, delete, update, list, console output, history, and workspace and working directory files)|<a href="https://microsoft.github.io/deployr-api-docs/#session-apis" target="_blank">Help</a>
Snapshot|These APIs provide different operations to access and manage workspace snapshots. A snapshot is a prepared environment image of a R session saved to Microsoft R Server, which includes the session's R packages, R objects and data files. This snapshot can be loaded into any subsequent remote R session for the user who created it. |<a href="https://microsoft.github.io/deployr-api-docs/#snapshot-apis" target="_blank">Help</a>
Status|This API returns a health report of the configuration, including the number of nodes, pool size, and other details. A [similar diagnostic report](admin-diagnostics.md) is available on the server.|<a href="https://microsoft.github.io/deployr-api-docs/#status-apis" target="_blank">Help</a>

<br>

The core APIs are accessible from and described in  `rserver-swagger-<version>.json`, **a Swagger-based JSON document**. Download this file from https://microsoft.github.io/deployr-api-docs/swagger/rserver-swagger-<version>.json, where `<version>` is the 3-digit R Server version number. Swagger is a popular specification for a JSON file that describes REST APIs.  

You can access all of these core APIs using a client library built from `rserver-swagger-<version>.json` using [these instructions and example](app-developer-get-started.md).

Note: For client applications written in **R**, you can side-step the Swagger approach altogether and exploit [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) directly to list, discover, and consume services. [Learn more in this vignette](../operationalize/data-scientist-manage-services.md).

## Service Consumption APIs

The service consumption REST APIs expose a wide range of R analytics services to client application developers.   After R code is published and exposed by R Server as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve R session outputs (R objects, graphics, files, ...) from the service.  

Whenever a web service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom Swagger-based JSON file. This swagger file describes each API needed to interact with that service. While the service consumption Swagger files are all named `swagger.json`, you'll find them each in a unique location by calling:
```
GET /api/{service-name}/{service-version}/swagger.json
``` 

To simplify the integration of R analytics web services using these APIs, build and use an API client library stub from the `swagger.json` file for each web service using [these instructions and example](app-developer-get-started.md).
