--- 
 
# required metadata 
title: "listRemoteFiles function (mrsdeploy) | Microsoft Docs" 
description: " Get a list of files in the working directory of the remote R session. " 
keywords: "(mrsdeploy), listRemoteFiles" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/18/2017" 
ms.topic: "reference" 
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
ms.technology: "r-server" 
ms.custom: "" 
 
--- 
 
 
 
 
 #listRemoteFiles: Get a list of files from the remote R session. 
 ##Description
 
Get a list of files in the working directory of the remote R session.
 
 
 ##Usage

```   
  listRemoteFiles()
 
```
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
A character vector containing the file names in the working directory of the remote session.
 
 ##See Also
 
[getRemoteFile](getRemoteFile.md)

[deleteRemoteFile](deleteRemoteFile.md)

[putLocalFile](putLocalFile.md)
   
 ##Examples

 ```
   
  ## Not run:
 
files<-listRemoteFiles()
 ## End(Not run) 
  
 
```
 
