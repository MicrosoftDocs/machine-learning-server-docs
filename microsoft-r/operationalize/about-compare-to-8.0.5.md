---

# required metadata
title: "Operationalizing: Compare 8.x to 9.x | Microsoft R Server Docs"
description: "Operationalizing Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "4/19/2017"
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

# Operationalizing with R Server: compare 9.x to DeployR 8.x

The following table presents some of the main differences between Microsoft R Server 9.x configured to operationalize analytics and the add-on DeployR 8.0.5 which was available in R Server 8.0.5.

>[!Important]
>R Server configured to operationalize analytics is **not backwards compatible** with DeployR 8.x. There is no migration path as the APIs are completely new and the data stored in the database is structured differently. 

Release|Microsoft R Server 8.0.5|Microsoft R Server 9.x
----|-----|------
Name of feature|DeployR|_integrated in R Server_
Install|Installer available separately from R Server|Integrated with R Server. Use the Administration Utility to [configure R Server to operationalize analytics](configuration-initial.md) and enable R Server to deploy and host web services
Deployment<br><small>(Turn R analytics into web services)</small>|Involves multiple steps, beginning with the upload of R analytics to the repository DB.|Publish R analytics directly from the R console using [new `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) or from a REST API.
Application Integration|Use client libraries and RBroker framework|[Swagger-based API for quicker exploration and integration](app-developer-get-started.md)
Architecture|Apache Tomcat|ASP .Net Core
Authentication|Authentication options:<br>-Basic<br>-Active Directory/LDAP<br>-PAM|[Authentication options](security-authentication.md):<br>-Active Directory/LDAP<br>-Azure Active Directory<br>-Local Administrator Account
High Availability|Active-Active recovery not supported|Active-Active recovery supported
Remote Execution|Use DeployR APIs to build your custom approach to remote execution|Use the [built-in remote execution functions](remote-execution.md) in the `mrsdeploy` package.
Web UI|Login, Admin Console, Repository Manager, API Explorer, Event Console|Coming with new design in future release
APIs|Over 100 RESTful APIs|[About 40 RESTful APIs](api.md)<br> (not backwards compatible)


Some term equilavents in the R Server 9.x:

|Version 8|Version 9|
|------------|---------------|
|Projects|Sessions|
|RBroker pool|Now part of web services implementation with an internal pool of R shells|
