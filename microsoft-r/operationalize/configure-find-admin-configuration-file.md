---

# required metadata
title: "Finding the appsettings.json configuration file for R Server - Machine Learning Server | Microsoft Docs"
description: "Where to find appsettings.json for R Server, web node, compute node"
keywords: "R Server configuration file, appsettings.json"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "6/21/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""
---

# Default install paths for compute and web nodes

**Applies to:  Machine Learning Server, Microsoft R Server 9.x**

## Machine Learning Server 9.2.1

|Node type|Node install path|
|----|------------|
|Web|\<server-home>/Microsoft.MLServer.WebNode|
|Compute|\<server-home>/Microsoft.MLServer.ComputeNode|

Where \<server-home> is:
+ **Windows**, either:
  + C:\Program Files\Microsoft\ML\_Server\PYTHON\_SERVER\o16n  
  + C:\Program Files\Microsoft\ML\_Server\R\_SERVER\o16n 

+ **Linux**: &nbsp;&nbsp;/usr/lib64/microsoft-r/rserver/o16n/9.2.1  


## Microsoft R Server 9.1.0

|Node type|Node install path|
|----|------------|
|Web|\<server-home>/Microsoft.RServer.WebNode|
|Compute|\<server-home>/Microsoft.RServer.ComputeNode|

Where \<server-home> is:
+ **Windows**: \<r-home>\deployr  
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Run 'normalizePath(R.home())' in the R console for R home._

+ **Linux**: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/usr/lib64/microsoft-r/rserver/o16n/9.1.0  


## Microsoft R Server 9.0.1

|Node type|Node install path|
|----|------------|
|Web|\<server-home>/Microsoft.DeployR.Server.WebAPI|
|Compute|\<server-home>/Microsoft.DeployR.Server.BackEnd|

Where \<server-home> is:
+ **Windows**: \<r-home>\deployr  
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Run 'normalizePath(R.home())' in the R console for R home._

+ **Linux**: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/usr/lib64/microsoft-r/rserver/o16n/9.0.1  