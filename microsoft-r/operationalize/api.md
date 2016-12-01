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

All operationalization API calls must be authenticated using the `POST /login` API or [through Azure Active Directory or Active Directory/LDAP](security-authentication.md). If you use the `POST /login` API, you'll get the [bearer/access token](security-access-tokens.md). This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's core operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. Other authentication APIs enable you to refresh or revoke the token. [Learn about each authentication API](https://microsoft.github.io/deployr-api-docs/9.0.1/#authentication-apis)...

<a name="services"></a>
### Web Services APIs

The core Web Services APIs facilitate the publishing and management of user-defined analytic web services. An R Server web service is a stateless execution in an R shell on the compute node. Each web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. 

These RESTful APIs for web services provide programmatic access to a service's lifecycle. The APIs are straightforward, easy-to-use, and when composed together, can form the foundation for more expressive operations. [Learn about each web service API](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis)...

Web services can be versioned to improve the release of services for service authors and make it easier for consumers to identify the services they are calling.  Service versioning is an important part of the publishing process and is designed to be flexible without naming restrictions. We highly recommend the adoption of meaningful and standard versioning conventions such as [semantic versioning](http://semver.org/).

If a web service is published without a version (<code>GET /services/{name}</code>), a Globally Unique Identifier (GUID) will be created as a unique reference number to represent a version. Since the GUID is not a meaningful version number, the intention is allow an author to treat the service as a <i>development</i> version (seen only by the author) until it is ready to be promoted and shared. From there, a more meaningful version number can be chosen during service publishing to represent a stable release.

Whenever a web service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom <a href="http://swagger.io/">Swagger</a>-based JSON file. <a href="service-integration.md#consume">Learn more about consuming web services</a>.

<a name="sessions"></a>
### Session APIs

A session is an interactive stateful R session that is run in an R shell on the compute node. A session is managed by R Server on behalf of the application. 

The session APIs can be divided into the following groups:
+ Session lifecycle APIs help manage the R session lifecycle.
+ Session workspace APIs help manage the objects in your workspace.
+ Session working directory APIs help manage the files in your workspace.
+ Session snapshot APIs help create and manage session snapshots. More snapshot APIs are covered in the next section.

[Learn about each session API](https://microsoft.github.io/deployr-api-docs/9.0.1/#session-apis)...


<a name="snapshots"></a>
### Snapshot APIs

If you need a prepared environment for remote script execution that includes any of the following: R packages, R objects and data files, consider creating a **snapshot**. A snapshot is an image of a R session saved to Microsoft R Server, which includes:
+ The session's workspace along with the installed R packages
+ Any files and artifacts in the working directory

A snapshot can be loaded into any subsequent remote R session for the user who created it.  For example, suppose you want to execute a script that needs three R packages, a reference data file, and a model object.   Instead of loading these items each time you want to execute the script, create a snapshot of an R session containing them. Then, you can save time later by loading this snapshot using its ID to get the session contents exactly as they were at the time the snapshot was created. 

Snapshots are only accessible to the user that creates them and cannot be shared across users.

Snapshots can be used for and by sessions and web service APIs. You can create a snapshot to freeze a session in time and then use it when consuming a web service.

>For optimal performance, consider the size of the snapshot carefully especially when publishing a service. Before creating a snapshot, ensure that keep only those workspace objects you need and purge the rest.  And, in the event that you only need a single object, consider passing that object alone itself instead of using a snapshot.

[Learn about each snapshot API](https://microsoft.github.io/deployr-api-docs/9.0.1/#snapshot-apis) used to manage to create and manage session snapshots. There are a few more snapshot APIs under the [session group](#sessions).


<a name="status"></a>
### Status API

You can retrieve the 'raw details' on the health of the system using the `GET /status` API call. [Learn about this API](https://microsoft.github.io/deployr-api-docs/9.0.1/#status-apis).


<br>


<a name=swagger></a>

## Creating API Client Libraries from Swagger-based JSON File 

To simplify the integration of R analytics web services using the [R Server operationalization APIs](https://microsoft.github.io/deployr-api-docs/9.0.1), we provide a core [Swagger template](http://swagger.io/) that defines each API.  In a nutshell, Swagger is a popular specification for a JSON file that describes REST APIs. 

Using a Swagger code generation tool such as  [Azure AutoRest](https://github.com/Azure/autorest) or [code-gen](https://github.com/swagger-api/swagger-codegen), you can generate an API client that can be used in various stacks, such as .NET, Java, Javascript, to access the RESTful web services. The API client simplifies the making of calls, encoding of data, and markup response handling on the API.    

**To build your client libraries:**

1. Download the Swagger-based JSON file, `rserver-swagger-9.0.1.json`, containing the definitions for the core operationalization APIs. This file can be found on the [main API documentation page](https://microsoft.github.io/deployr-api-docs/9.0.1).

1. Install a Swagger code generator such as [Azure autorest](https://github.com/Azure/autorest).

1. Run the file through the code generator, and specify the language you want. 

1. Review the resulting API client stub that was generated. You can provide some custom headers and make other changes.

You can now use the generated client library stub to call the core APIs.
