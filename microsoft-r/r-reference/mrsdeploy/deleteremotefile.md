--- 
 
# required metadata 
title: "deleteRemoteFile function (mrsdeploy) | Microsoft Docs" 
description: " Delete a file from the working directory of the remote R session. " 
keywords: "(mrsdeploy), deleteRemoteFile" 
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
 
 
 
 
 #deleteRemoteFile: Delete a file from the remote R session. 
 ##Description
 
Delete a file from the working directory of the remote R session.
 
 
 ##Usage

```   
  deleteRemoteFile(filename)
 
```
 
 ##Arguments

   
  
 ### `filename`
 Name of the file to delete. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
`TRUE` if successful.
 
 ##See Also
 
[getRemoteFile](getRemoteFile.md)

[listRemoteFiles](listRemoteFiles.md)

[putLocalFile](putLocalFile.md)
   
 ##Examples

 ```
   
  ## Not run:
 
deleteRemoteFile("c:/data/test.csv")
 ## End(Not run) 
  
 
```
 
