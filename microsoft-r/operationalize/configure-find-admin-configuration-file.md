---

# required metadata
title: "Default install paths for compute and web nodes - Machine Learning Server "
description: "Where to find appsettings.json for Machine Learning Server, web node, compute node"
keywords: "Machine Learning Server configuration file, appsettings.json"
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "9/25/2017"
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

## Machine Learning Server 9.3

The installation path depends on the operating system and the type of node (web or compute).

**Find your server directory path**
|OS|\<server-directory>|
|----|------------|
|Windows|C:\Program Files\Microsoft\ML Server\PYTHON\_SERVER\o16n<br>C:\Program Files\Microsoft\ML Server\R\_SERVER\o16n|
|Linux|/opt/microsoft/mlserver/9.3/o16n||

**Find your node paths**
|File(s)|Filepath by node|
|----|------------|
|**Log files**|**Web node:** \<server-directory>\Microsoft.MLServer.WebNode\logs\*.* <br/>**Compute node:** \<server-directory>\Microsoft.MLServer.ComputeNode\logs\*.*|
|**appsettings.json**|**Web node:** \<server-directory>\Microsoft.MLServer.WebNode\appsettings.json<br/>**Compute node:** \<server-directory>\Microsoft.MLServer.ComputeNode\appsettings.json|

  
## Machine Learning Server 9.2.1

The installation path depends on the operating system and the type of node (web or compute).

|OS|Path|
|----|------------|
|Windows|C:\Program Files\Microsoft\ML Server\PYTHON\_SERVER\o16n<br>C:\Program Files\Microsoft\ML Server\R\_SERVER\o16n|
|Linux|/opt/microsoft/mlserver/9.2.1/o16n||


+ **Administration utility** is under: \<server-directory>\Microsoft.MLServer.Utils.AdminUtil\

+ **Log files** are under: \<server-directory>\\\<node-directory>\logs\*.*

+ **appsettings.json** is under: \<server-directory>\\\<node-directory>\appsettings.json

  Where the \<node-directory> name: 

  |Type|Node directory name|
  |----|------------|
  |Web node|Microsoft.MLServer.WebNode|
  |Compute node|Microsoft.MLServer.ComputeNode|

## Microsoft R Server 9.1.0

The installation path depends on the operating system and the type of node (web or compute).

|OS|Path|
|----|------------|
|Windows|\<r-home>\deployr<br>(_Run 'normalizePath(R.home())' in the R console for R home._)|
|Linux|/usr/lib64/microsoft-r/rserver/o16n/9.1.0/\<node-directory>||

Where the \<node-directory> name: 

|Type|Node Directory Name|
|----|------------|
|Web node|Microsoft.RServer.WebNode|
|Compute node|Microsoft.RServer.ComputeNode|

## Microsoft R Server 9.0.1

The installation path depends on the operating system and the type of node (web or compute).

|OS|Path|
|----|------------|
|Windows|\<r-home>\deployr<br>(_Run 'normalizePath(R.home())' in the R console for R home._)|
|Linux|/usr/lib64/microsoft-r/rserver/o16n/9.0.1/\<node-directory>||

Where the \<node-directory> name: 

|Type|Node Directory Name|
|----|------------|
|Web node|Microsoft.DeployR.Server.WebAPI|
|Compute node|Microsoft.DeployR.Server.BackEnd|