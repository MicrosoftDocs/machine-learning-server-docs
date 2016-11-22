---

# required metadata
title: "Operationalization with R Server"
description: "Operationalization of R Analytics with Microsoft R Server"
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

# Comparing to 8.x Releases

The following table presents some of the main differences between the operationalization feature in Microsoft R Server 9.0 and the DeployR feature available in R Server 8.0.5.

Release|Microsoft R Server 8.0.5|Microsoft R Server 9.0
----|-----|------|
Name of feature|DeployR|operationalization
Install|Installer available separately from R Server|Integrated with R Server. Configure via script to enable R Server to deploy and host web services
Deployment<br><small>(Turn R analytics into web services)</small>|Involves multiple steps, beginning with the upload of R analytics to the repository DB.|Publsih R analytics directly from the R console using new `mrsdeploy` package.
Application Integration|Use client libraries and RBroker framework|Swagger-based API for quicker exploration and integration
Architecture|Apache Tomcat|ASP .Net Core
Authentication|Authentication options:<br>-Basic<br>-Active Directory/LDAP<br>-PAM|Authentication options:<br>-Active Directory/LDAP<br>-Azure Active Directory
High Availability|Active-Active recovery not supported|Active-Active recovery supported
Remote Execution|Use DeployR APIs to build your custom approach to remote execution|Use the built-in remote execution functions in the `mrsdeploy` package.
Web UI|Login, Admin Console, Repository Manager, <br>API Explorer, Event Console|Coming in 2017 with new design 
APIs|Over 100 RESTful APIs|About 40 RESTful APIs<br> (not backwards compatible)

