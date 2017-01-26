---

# required metadata
title: "Operationalization of R Analytics | Microsoft R Server Docs"
description: "Operationalization of R Analytics with Microsoft R Server"
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
# Managing Web Services

<!--



> Learn about consuming the web services [here](app-developer-get-started.md)
>
> Learn more about@@


## Snapshots

If you need a prepared environment for remote script execution that includes any of the following: R packages, R objects and data files, consider creating a **snapshot**. A snapshot is an image of a R session saved to Microsoft R Server, which includes:
+ The session's workspace along with the installed R packages
+ Any files and artifacts in the working directory

>For optimal performance, consider the size of the snapshot carefully especially when publishing a service. Before creating a snapshot, ensure that you keep only those workspace objects you need and purge the rest.  And, in the event that you only need a single object, consider passing that object alone itself instead of using a snapshot.

A snapshot can be loaded into any subsequent remote R session for the user who created it.  For example, suppose you want to execute a script that needs three R packages, a reference data file, and a model object.   Instead of loading these items each time you want to execute the script, create a snapshot of an R session containing them. Then, you can save time later by loading this snapshot using its ID to get the session contents exactly as they were at the time the snapshot was created. 

[Learn about each snapshot API](https://microsoft.github.io/deployr-api-docs/9.0.1/#snapshot-apis) used to manage to create and manage session snapshots.


## Publishing and Managing Web Services

The core Web Services APIs facilitate the publishing and management of user-defined analytic web services. An R Server web service is a stateless execution in an R shell on the compute node. Each web service is uniquely defined by a `name` and `version` for easy service consumption and meaningful machine-readable discovery approaches. 

These RESTful APIs for web services provide programmatic access to a service's lifecycle. The APIs are straightforward, easy-to-use, and when composed together, can form the foundation for more expressive operations. [Learn about each web service API](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis).

Web services can be versioned to improve the release of services for service authors and make it easier for consumers to identify the services they are calling.  Service versioning is an important part of the publishing process and is designed to be flexible without naming restrictions. We highly recommend the adoption of meaningful and standard versioning conventions such as [semantic versioning](http://semver.org/).

If a web service is published without a version (<code>GET /services/{name}</code>), a Globally Unique Identifier (GUID) will be created as a unique reference number to represent a version. Since the GUID is not a meaningful version number, the intention is allow an author to treat the service as a <i>development</i> version (seen only by the author) until it is ready to be promoted and shared. From there, a more meaningful version number can be chosen during service publishing to represent a stable release.

Whenever a web service is published (<code>POST /services/{name}/{version}</code>), an endpoint is registered (<code>/api/{name}/{version}</code>), which in turn triggers the generation of a custom <a href="http://swagger.io/">Swagger</a>-based JSON file. <a href="app-developer-get-started.md">Learn more about consuming web services</a>.

If you use a snapshot when developing your web service, the session snapshot information will be baked into the service API when published and available to any authenticated user consuming that web service later.

Please note that while only the web service author can update, publish, or delete a service, any authenticated user can retrieve a list of services or consume a service. 
-->