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

When a web service is published (`/services/{name}/{version}`), not only is a endpoint is created (`/api/{name}/{version}`) for the consumption of that specific version of the service, but a custom Swagger document defining that service version is also dynamically generated (`/api/{name}/{version}/swagger.json`).  This Swagger document contains all of the APIs specific to the service version and the [authentication APIs](api.md#authentication) so that an app dev will have everything they need to interact with the published web service. 

You can run this `swagger.json`@@ DOES THIS HAVE A VERSION SPECIFIC NAME OR A GENERIC NAME file through your Swagger code generator* to produce a custom client library stub for the consumption of that version of that Web service. 

## Swagger & Client Libraries

To simplify the integration of R analytics Web services using the [R Server operationalization APIs](https://microsoft.github.io/deployr-api-docs/), we provide a core [Swagger template](http://swagger.io/) that defines each API. 

You can use this template to build the client libraries that will simplify making calls, encoding data, and handling response markup on the API.  

**To build your client libraries:**

1. Download the core Swagger template, `swagger.json`, is available here@@.
1. Get a Swagger code generator such as  [Azure autorest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen).
1. Run the Swagger file through the code generator specifying the language you want. 

You can now use the generated client library stub to call the core APIs.

