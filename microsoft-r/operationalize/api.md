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

**Applies to:  Microsoft R Server 9.0.1**

The Microsoft R Server [operationalization API](https://microsoft.github.io/deployr-api-docs/) exposes the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications. This API is exposed by the R Server's operationalization server, a standards-based server technology capable of scaling to meet the needs of enterprise-grade deployments. With this server, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific API call? [Look in this online API reference.](https://microsoft.github.io/deployr-api-docs)</big>

While data scientists can work with R directly in an R console window or R IDE, application developers need a different set of tools to leverage R inside applications. The API exposes **R analytics Web services**, making the full capabilities of R available to application developers on a simple yet powerful Web services API.

As an application developer integrating with Microsoft R Server-hosted **Web services**, typically your interest is in executing R code, not writing it. Data scientists with R programming skills write R code. Using some core APIs, this R code can be published as a Microsoft R Server-hosted analytics Web service. Once R code is exposed by R Server as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve outputs from the service. Those outputs can include R object data, R graphics output such as plots and charts, and any file data written to the working directory associated the current R session.

Each time a service is executed on the API, the service makes use of an R session that is managed by R Server on behalf of the application. 

## Client Libraries

To simplify the integration of R analytics Web services using the [R Server operationalization APIs](https://microsoft.github.io/deployr-api-docs/), we provide a core [Swagger template](http://swagger.io/) that defines each API. 

You can use this template to build the client libraries that will simplify making calls, encoding data, and handling response markup on the API.  

**Get the Core Swagger File**

The core Swagger template, `swagger.json`, is available here@@. 

**Build Your Client Libraries**

To build your client libraries, you'll need a Swagger code generator such as  [Azure autorest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen).

Then, run the Swagger file through the code generator to get a custom client library stub you can use to call these core APIs. 

<br>

## The Core APIs Introduced

the core R Server operationalization APIs include those used to authenticate, create R sessions and snapshots, execute R code, upload objects, and publish Web services. They can be grouped by the following areas. 

### User Authentication & Status

All operationalization API calls must be authenticated using the `/login` API or [through Azure Active Directory or Active Directory/LDAP](security-authenticaton.md). Once you use the `/login` API, you'll get the access and refresh tokens@@. 

|Common&nbsp;APIs|Description|
|----|-----------|
|`POST /login`|Logs the user in|
|`POST /login/refreshToken`|The user renews the access token and refresh token|
|`DELETE /login/refreshToken/{refreshToken}`|The user revokes a refresh token|
|`GET /status`|Gets the current health of the system|

Learn more about the set of authentication APIs, [here](?tags=User).

@@What about Access tokens LINK???

### Web Services

@@Small intro para

These are the APIs around the management and life cycle of Web services. For more information on these Web services, @@CHECKOUT THE VIGNETTE.

When a service is published (`/services/{name}/{version}`), not only is a Web service endpoint is created (`/api/{name}/{version}`) for the consumption of that service, but another Swagger document defining that service is also generated (`/api/{name}/{version}/swagger.json`). There is always one Swagger service template for each Web service version. You can run this swagger.json file through your Swagger code generator* to produce a custom client library stub for the consumption of that version of that Web service. 

|Common&nbsp;APIs|Description|
|----|-----------|
|`GET /services`|Lists all published web services for the authenticated user.|
|`GET /services/{name}`|Lists all published web services by the specified name for the authenticated user.|
|`POST /services/{name}`|Publish web service for the authenticated user with given name and a unique version.|
|`GET /services/{name}/{version}`|Lists all the published web services having given name and version for the logged in user.|
|`POST /services/{name}/{version}`|Publish the web service for the logged in user with given name and version.|
|`DELETE /services/{name}/{version}`|Deletes the published web service for the logged in user.|
|`PATCH /services/{name}/{version}`|Patches the published web service for the logged in user.|
|`GET /api/{name}/{version}/swagger.json`|Get Swagger capability json for the published web service.|

Learn more about the set of Web services APIs, [here](?tags=Services).

### Sessions

@@Small intro para

**Session Lifecycle APIs**

These calls manage the lifecycle of an R session.

|Common&nbsp;APIs|Description|
|----|-----------|
|`POST /sessions`|Creates a session|
|`GET /sessions`|Lists or existing sessions|
|`DELETE /sessions/{id}`|Closes a session and all its associated resources|
|`DELETE /sessions/{id}/force`|Attempts to forcefully close a session and all its associated resources|
|`POST /sessions/{id}/execute`|Executes code in the context of a specific session|
|`POST /sessions/{id}/cancel`|Cancel a session by aborting the current execution operation|
|`GET /sessions/{id}/console-output`|Returns the console output for the current or last execution|
|`GET /sessions/{id}/history`|Lists all history for a specific session|

**Session Workspace APIs**

These APIs allow you to manage the objects in your workspace.

|Common&nbsp;APIs|Description|
|----|-----------|
|`GET /sessions/{id}/workspace`|Lists all object names of a specific session|
|`GET /sessions/{id}/workspace/{objectName}`|Returns an R object from a session as RData file stream|
|`POST /sessions/{id}/workspace/{objectName}`|Upload a serialized R object into R session|
|`DELETE /sessions/{id}/workspace/{objectName}`|Delete an object from a session|

**Session Working Directory APIs**

These APIs allow you to manage the files in your workspace.

|Common&nbsp;APIs|Description|
|----|-----------|
|`GET /sessions/{id}/files`|Lists all files of a specific session|
|`POST /sessions/{id}/files`|Loads a file into the R session working directory| 
|`GET /sessions/{id}/files/{fileName}`|Downloads a file from a session as stream|
|`DELETE /sessions/{id}/files/{fileName}`|Delete a file from a session working directory|


**Session Snapshot APIs**

These APIs allow you to create and manage session snapshots.

|Common&nbsp;APIs|Description|
|----|-----------|
|`POST /sessions/{id}/snapshot `|Create snapshot from specific R session.|
|`POST /sessions/{id}/loadsnapshot/{snapshotId}`|Loads a certain snapshot into specific R session.|
|` `| |

@@ADD link to vignette


### Snapshots

You can prolong the lifespan of a session by saving the session's workspace and working directory into a **snapshot**. A snapshot includes:
+ The session's workspace along with the installed R packages
+ Any files and artifacts in the working directory

You can then retrieve this snapshot later using its ID to access the session contents exactly as they were at the time the snapshot was created.

Snapshots can be used for and by both sessions and web service APIs. You can create a snapshot to freeze a session in time and then use it when consuming a web service.

These APIs allow you to create and manage session snapshots.

|Common&nbsp;APIs|Description|
|----|-----------|
|`GET /snapshots`|List all snapshots for the current user.|
|`GET /snapshots/{id}`|Get the snapshot content as a zip file containing the working directory and workspace.|
|`DELETE /snapshots/{id}`|Delete specified snapshot.|


## OPEN QUESTIONS

1. where do users find the swagger doc? app developers need access to it.
1. Who is writing the swagger - how to use doc?  Sean to write, Josee to polish?? what needs to be filled out in the template?  And what needs to be fleshed out in the resulting client library stubs? 