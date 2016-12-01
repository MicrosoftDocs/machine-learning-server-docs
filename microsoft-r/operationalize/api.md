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

## Introduction
The core Microsoft R Server [operationalization APIs](https://microsoft.github.io/deployr-api-docs/9.0.1/) expose the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications. This REST API is exposed by the R Server's operationalization server, a standards-based server technology capable of scaling to meet the needs of enterprise-grade deployments. With this server, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

><big>Looking for a specific API call? [Look in this online API reference.](https://microsoft.github.io/deployr-api-docs/9.0.1/)</big>

While data scientists can work with R directly in an R console window or R IDE, application developers need a different set of tools to leverage R inside applications. The API exposes Microsoft R Server-hosted **R analytics web services**, making the full capabilities of R available to application developers on a simple yet powerful Web services API.

As an application developer integrating with these web services, typically your interest is in executing R code, not writing it. Data scientists with the R programming skills write the R code. Then, using some core APIs, this R code can be published as a Microsoft R Server-hosted analytics Web service. 

Once R code is exposed by R Server as a web service, an application can make API calls to pass inputs to the service, execute the service and retrieve outputs from the service. Those outputs can include R object data, R graphics output such as plots and charts, and any file data written to the working directory associated the current R session.

To simplify the integration of R analytics web services using these APIs, you can build and use [an API client library stub](#swagger).

<br>

## Core Operationalization APIs

The core R Server operationalization APIs can be grouped by the following areas:
+ [Authentication & Status](#authentication)
+ [Web Services management & consumption](#services)
+ [Session management](#sessions)
+ [Snapshot management](#snapshots)
+ [Status](#status)

These REST APIs are described in a [Swagger-based JSON document](#swagger) delivered with the product. 

<a name="authentication"></a>
### User Authentication APIs

All operationalization API calls must be authenticated using the `POST /login` API or [through Azure Active Directory or Active Directory/LDAP](security-authentication.md). 

Once your use the `POST /login` API, you'll get the [bearer/access token](security-access-tokens.md). 
This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's core operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about bearer tokens and refreshTokens](security-access-tokens.md) for operationalization.

>For the full documentation for each authentication API, check out [this API Reference section](https://microsoft.github.io/deployr-api-docs/9.0.1/#authentication-apis).

|Authentication API|Description|
|----|-----------|
|`POST /login`|Logs the user in|
|`POST /login/refreshToken`|The user renews the access token and refresh token|
|`DELETE /login/refreshToken/{refreshToken}`|The user revokes a refresh token|
|`GET /status`|Gets the current health of the system|

<br>

<a name="services"></a>
### Web Services APIs

The core Web Services APIs facilitate the publishing and management of user-defined analytic web services. An R Server web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. The APIs are straightforward, easy-to-use, and when composed together, can form the foundation for more expressive operations.

Web services can be versioned to improve the release of services for service authors and make it easier for consumers to identify the services they are calling.  Service versioning is an important part of the publishing process and is designed to be flexible without naming restrictions. We highly recommend the adoption of meaningful and standard versioning conventions such as [semantic versioning](http://semver.org/).

>For the full documentation for each web service API, check out [this API reference section](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis).

The Service Management APIs are RESTful APIs that provide programmatic access to a services management’s lifecycle:

<table><thead>
<tr>
<th width="325">Web Services API</th>
<th>Description</th>
</tr>
</thead><tbody>
<tr>
<td><code>GET /services</code></td>
<td>Lists all published web services for the authenticated user.</td>
</tr>
<tr>
<td><code>GET /services/{name}</code></td>
<td>Lists all published web services by the specified name for the authenticated user.</td>
</tr>
<tr>
<td><code>POST /services/{name}</code></td>
<td>Publish web service for the authenticated user with given name. Since no `version` was specified a `GUID` (Globally Unique Identifier) will be created as a unique reference number to represent a version. <br><br>Since the GUID is not a meaningful version number, the intention is allow an author to treat the service as a <i>development</i> version (seen only by the author) until it is ready to be promoted and shared. From there, a more meaningful version number can be chosen during service publishing to represent a stable release.</td>
</tr>
<tr>
<td><code>GET /services/{name}/{version}</code></td>
<td>Lists all the published web services having given name and version for the logged in user.</td>
</tr>
<tr>
<td><code>POST /services/{name}/{version}</code></td>
<td>Publish the web service for the authenticated user with given name and version.</td>
</tr>
<tr>
<td><code>DELETE /services/{name}/{version}</code></td>
<td>Deletes the published web service for the logged in user.</td>
</tr>
<tr>
<td><code>PATCH /services/{name}/{version}</code></td>
<td>Patches the published web service for the logged in user.</td>
</tr>
<tr>
<td><code>GET /api/{name}/{version}/swagger.json</code></td>
<td>Get Swagger capability JSON for the published web service. Whenever a service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom <a href="http://swagger.io/">Swagger</a>-based JSON file. <a href="service-integration.md#consume">Learn more about consuming web services</a>.</td>
</tr>
</tbody></table>

<a name="sessions"></a>
### Session APIs

Each time a service is executed on the API, the service makes use of an R session that is managed by R Server on behalf of the application. 

The session APIs can be divided into the following groups:
+ Session lifecycle APIs help manage the R session lifecycle.
+ Session workspace APIs help manage the objects in your workspace.
+ Session working directory APIs help manage the files in your workspace.
+ Session snapshot APIs help create and manage session snapshots.

>For the full documentation for each session API, check out [this API Reference section](https://microsoft.github.io/deployr-api-docs/9.0.1/#session-apis).

<table>
    <thead>
        <tr>
            <th width="370">Session Lifecycle API</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>POST /sessions</code></td>
            <td>Creates a session</td>
        </tr>
        <tr>
            <td><code>GET /sessions</code></td>
            <td>Lists or existing sessions</td>
        </tr>
        <tr>
            <td><code>DELETE /sessions/{id}</code></td>
            <td>Closes a session and deletes all its associated resources</td>
        </tr>
        <tr>
            <td><code>DELETE /sessions/{id}/force</code></td>
            <td>Attempts to forcefully close a session and deletes all its associated resources</td>
        </tr>
        <tr>
            <td><code>POST /sessions/{id}/execute</code></td>
            <td>Executes code in the context of a specific session</td>
        </tr>
        <tr>
            <td><code>POST /sessions/{id}/cancel</code></td>
            <td>Cancel a session by aborting the current execution operation</td>
        </tr>
        <tr>
            <td><code>GET /sessions/{id}/console-output</code></td>
            <td>Returns the console output for the current or last execution</td>
        </tr>
        <tr>
            <td><code>GET /sessions/{id}/history</code></td>
            <td>Lists all history for a specific session</td>
        </tr>
    </tbody>
</table>

<p><br></p>

<table>
    <thead>
        <tr>
            <th width="370">Session Workspace API</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>GET /sessions/{id}/workspace</code></td>
            <td>Lists all object names of a specific session</td>
        </tr>
        <tr>
            <td><code>GET /sessions/{id}/workspace/{objName}</code></td>
            <td>Returns named R object from a session as RData file stream</td>
        </tr>
        <tr>
            <td><code>POST /sessions/{id}/workspace/{objName}</code></td>
            <td>Upload named serialized R object into R session</td>
        </tr>
        <tr>
            <td><code>DELETE /sessions/{id}/workspace/{objName}</code></td>
            <td>Delete named object from a session</td>
        </tr>
    </tbody>
</table>

<p><br></p>

<table>
    <thead>
        <tr>
            <th width="370">Session Working Directory API</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>GET /sessions/{id}/files</code></td>
            <td>Lists all files of a specific session</td>
        </tr>
        <tr>
            <td><code>POST /sessions/{id}/files</code></td>
            <td>Loads a file into the R session working directory</td>
        </tr>
        <tr>
            <td><code>GET /sessions/{id}/files/{fileName}</code></td>
            <td>Downloads a file from a session as stream</td>
        </tr>
        <tr>
            <td><code>DELETE /sessions/{id}/files/{fileName}</code></td>
            <td>Delete a file from a session working directory</td>
        </tr>
    </tbody>
</table>

<p>
    <a name="sessionsnapshots"></a>
    <br></p>

Note: Other snapshot APIs are covered in the section below.

<table>
    <thead>
        <tr>
            <th width="370">Session Snapshot API</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>POST /sessions/{id}/snapshot</code></td>
            <td>Create snapshot from specific R session.</td>
        </tr>
        <tr>
            <td><code>POST /sessions/{id}/loadsnapshot/{snapshotId}</code></td>
            <td>Loads a certain snapshot into specific R session.</td>
        </tr>
    </tbody>
</table>

<br>

<a name="snapshots"></a>
### Snapshot APIs


If you need a prepared environment for remote script execution that includes any of the following: R packages, R objects and data files, consider creating a **snapshot**. A snapshot is an image of a R session saved to Microsoft R Server, which includes:
+ The session's workspace along with the installed R packages
+ Any files and artifacts in the working directory

A snapshot can be loaded into any subsequent remote R session for the user who created it.  For example, suppose you want to execute a script that needs three R packages, a reference data file, and a model object.   Instead of loading these items each time you want to execute the script, create a snapshot of an R session containing them. Then, you can save time later by loading this snapshot using its ID to get the session contents exactly as they were at the time the snapshot was created. 

Snapshots are only accessible to the user that creates them and cannot be shared across users.

The following functions are available for working with snapshots:  
`listSnapshots()`, `createSnapshot()`, `loadSnapshot()`, `downloadSnapshot()` and `deleteSnapshot()`.


Snapshots can be used for and by both sessions and web service APIs. You can create a snapshot to freeze a session in time and then use it when consuming a web service.

>[!Important]
>For optimal performance, consider the size of the snapshot carefully especially when publishing a service. Before creating a snapshot, ensure that keep only those workspace objects you need and purge the rest.  And, in the event that you only need a single object, consider passing that object alone itself instead of using a snapshot.

These APIs allow you to create and manage session snapshots. There are additional snapshot APIs in the [session group](#sessionsnapshots).


>For the full documentation for each snapshot API, check out [this API Reference section](https://microsoft.github.io/deployr-api-docs/9.0.1/#snapshot-apis).


|Snapshot APIs|Description|
|----|-----------|
|`GET /snapshots`|List all snapshots for the current user.|
|`GET /snapshots/{id}`|Get the snapshot content as a zip file containing the working directory and workspace.|
|`DELETE /snapshots/{id}`|Delete specified snapshot.|


<br>


<a name="status"></a>
### Status API

You can retrieve the 'raw details' on the health of the system.

>For the full documentation for the status API, check out [this API Reference section](https://microsoft.github.io/deployr-api-docs/9.0.1/#status-apis).

|Status API|Description|
|----|-----------|
|`GET /status`|Gets the current health of the system|

<br>


<a name=swagger></a>

## Creating API Client Libraries from Swagger-based JSON File 

To simplify the integration of R analytics web services using the [R Server operationalization APIs](https://microsoft.github.io/deployr-api-docs/), we provide a core [Swagger template](http://swagger.io/) that defines each API.  In a nutshell, Swagger is a popular specification for a JSON file that describes REST APIs. 

Using a Swagger code generation tool such as  [Azure AutoRest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen), you can generate an API client that can be used in various stacks, such as .NET, Java, Javascript, to access the RESTful web services. The API client simplifies the making of calls, encoding of data, and markup response handling on the API.    

**To build your client libraries:**

1. Download the Swagger-based JSON file, `rserver-swagger-9.0.1.json`, containing the definitions for the core operationalization APIs. This file can be found on the [main API documentation page](https://microsoft.github.io/deployr-api-docs/9.0.1).

1. Install a Swagger code generator such as [Azure autorest](https://github.com/Azure/autorest).

1. Run the file through the code generator, and specify the language you want. 

1. Review the resulting API client stub that was generated. You can provide some custom headers and make other changes.

You can now use the generated client library stub to call the core APIs.
