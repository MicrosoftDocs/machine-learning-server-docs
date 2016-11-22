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
# Consuming Web Services

**Applies to:  Microsoft R Server 9.0.1**

A web service can be published using the APIs directly (`/services/{name}/{version}`) or using the `mrsdeploy` package functions. When published, the following are generate for the consumption of the web service:

+ An endpoint (`/api/{name}/{version}`) for its consumption

+ A custom Swagger document containing all of the APIs specific to that service and the [authentication APIs](api.md#authentication) so that an application developer will have everything they need to interact with the published web service

## Swagger & Client Libraries

Each web service version has its own [Swagger template](http://swagger.io/) that defines the APIs needed to interact with it. 

You can use this template to build the client libraries that will simplify the making of calls, encoding of data, and markup response handling on the API.  

**To build your client libraries:**

1. Find the Swagger template, `swagger.json`, available under `/api/{name}/{version}/swagger.json`. Using the core APIs, you can use `GET /api/{name}/{version}/swagger.json`.

1. Install a Swagger code generator such as  [Azure autorest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen).

1. Run the Swagger file through the code generator specifying the language you want. 

You can now use the generated client library stub to call the APIs needed to authenticate and consume the service.