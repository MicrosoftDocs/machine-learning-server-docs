---

# required metadata
title: "Roles and Groups | Microsoft R Server Docs"
description: "Enterprise-Grade Security: Authentication Roles for Operationalization with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "02/14/2017"
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

# R Server configuration file (appsettings.json)

**Applies to:  Microsoft R Server 9.x**

The external configuration file, `appsettings.json`, defines a number of policies used when deploying and operationalizing web services with R Server. There is one `appsettings.json` file on each web node and on each compute node. This file contains a wide range of policy configuration options for that node.

+ On the web node, this configuration file governs authentication, SSL, CORS support, service logging, database connections, token signing, compute node declarations, and more.

+ On the compute node, this configuration file governs SSL, logging, R shell pool size, R execution ports, and more.

The location of this file depends on the R Server version and operating system you have. 

### Windows path to config file

The logs can be found as follows where `<MRS_home>` is the path to the Microsoft R Server installation directory. Type `normalizePath(R.home())` in your R console to find the path to `<MRS_home>`.

|Node|Path on version 9.1|
|----|------------|
|Web|<MRS_home>\o16n\Microsoft.RServer.WebNode|
|Compute|<MRS_home>\o16n\Microsoft.RServer.ComputeNode|

<br>

|Node|Path on version 9.0|
|----|------------|
|Web|<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI|
|Compute|<MRS_home>\deployr\Microsoft.DeployR.Server.BackEnd|


### Linux path to config file

The logs can be found here: 


|Node|Path on version 9.1|
|----|------------|
|Web|/usr/lib64/microsoft-r/rserver/o16n/9.1/Microsoft.RServer.WebNode|
|Compute|/usr/lib64/microsoft-r/rserver/o16n/9.1/Microsoft.RServer.ComputeNode|

<br>

|Node|Path on version 9.0|
|----|------------|
|Web|/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI|
|Compute|/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.BackEnd|
