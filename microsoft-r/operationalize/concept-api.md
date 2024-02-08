---

# required metadata
title: "APIs for operationalizing your models and analytics - Machine Learning Server "
description: "Operationalization APIs for authenticating, publishing, managing, and consuming web services with Machine Learning Server  or Microsoft R Server."
keywords: 
author: "chuckheinzelman"
ms.author: "charlhe"
manager: "cgronlun"
ms.date: 2/16/2018
ms.topic: "how-to"
ms.service: mlserver

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# APIs for operationalizing your models and analytics with Machine Learning Server  

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

**Applies to: Machine Learning Server, Microsoft R Server 9.x**

The Machine Learning Server (and Microsoft R Server) <a href="https://microsoft.github.io/deployr-api-docs/" target="_blank">REST APIs</a> are exposed by the operationalization server, a standards-based server technology [capable of scaling to meet the needs of enterprise-grade deployments](configure-machine-learning-server-enterprise.md). With the Machine Learning Server configured to operationalize, the full statistics, analytics, and visualization capabilities of R and Python can now be directly leveraged inside Web, desktop and mobile applications.

The APIs available with Machine Learning Server can be categorized into two groups: Core APIs and the Service Consumption APIs.

><big>Looking for a specific core API call? <a href="https://microsoft.github.io/deployr-api-docs/" target="_blank">Look in this online API reference</a>.</big>


<a name="core"></a>

## Core APIs for Operationalization 

These core REST APIs expose the R or Python platform as a service allowing the integration of Python models and R statistics, analytics, and visualizations inside Web, desktop and mobile applications.  These APIs enable you to publish Machine Learning Server-hosted **R analytics web services**, making the full capabilities of R available to application developers on a simple yet powerful REST API. These core operationalization APIs can be grouped into several categories as shown in this table. 

Core API Type|Description|Reference Help
---------|-----------|:-----:
Authentication|These APIs provide authentication-related operations and access workflow features.|<a href="https://microsoft.github.io/deployr-api-docs/#authentication-apis" target="_blank">Help</a>
Web Services|These APIs facilitate the publishing and management of user-defined analytic web services (create, delete, update, list, discover). Each web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. When a service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered and a [custom Swagger-based JSON file is generated](how-to-build-api-clients-from-swagger-for-app-integration.md).|<a href="https://microsoft.github.io/deployr-api-docs/#services-management-apis" target="_blank">Help</a>
Session|These APIs provide functionality for R session management (create, delete, update, list, console output, history, and workspace and working directory files)|<a href="https://microsoft.github.io/deployr-api-docs/#session-apis" target="_blank">Help</a>
Snapshot|These APIs provide different operations to access and manage workspace snapshots. A snapshot is a prepared environment image of an R or Python session saved to Machine Learning Server, which includes the session's packages, objects and data files. This snapshot can be loaded into any subsequent remote session for the user who created it. [Learn more about R session snapshots](../r/how-to-execute-code-remotely.md#snapshot) |<a href="https://microsoft.github.io/deployr-api-docs/#snapshot-apis" target="_blank">Help</a>
Status|This API returns a health report of the configuration, including the number of nodes, [pool size](configure-evaluate-capacity.md#pool), and other details. A [similar diagnostic report](configure-run-diagnostics.md) is available on the server.|<a href="https://microsoft.github.io/deployr-api-docs/#status-apis" target="_blank">Help</a>

<br>

The core APIs are accessible from and described in  `mlserver-swagger-<version>.json`, **a Swagger-based JSON document**. Download this file from `https://microsoft.github.io/deployr-api-docs/swagger/mlserver-swagger-\<version>.json`, where `<version>` is the 3-digit product version number. Swagger is a popular specification for a JSON file that describes REST APIs.  For R Server users, replace mlserver-swagger with rserver-swagger.

You can access all of these core APIs using a client library built from `rserver-swagger-<version>.json` using [these instructions and example](how-to-build-api-clients-from-swagger-for-app-integration.md).

Note: For client applications written in **R**, you can side-step the Swagger approach altogether and exploit [the mrsdeploy package](../r-reference/mrsdeploy/mrsdeploy-package.md) directly to list, discover, and consume services. [Learn more in this article](how-to-consume-web-service-interact-in-r.md).

## Service Consumption APIs

The service consumption REST APIs expose a wide range of Python and R analytics services to client application developers.   After Python or R code is published and exposed by the server as a web service, an application can make API calls to pass inputs to the service, execute the service, and retrieve outputs from the service.  

Whenever a web service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom Swagger-based JSON file. This swagger file describes each API needed to interact with that service. While the service consumption Swagger files are all named `swagger.json`, you can find them each in a unique location by calling:
```
GET /api/{service-name}/{service-version}/swagger.json
``` 

To make it easier to integrate R and Python web services using these APIs, build and use an API client library stub from the `swagger.json` file for each web service using [these instructions and example](how-to-build-api-clients-from-swagger-for-app-integration.md).
