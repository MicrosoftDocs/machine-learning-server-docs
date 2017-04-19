--- 
 
# required metadata 
title: "Upload a file from the local machine to the remote R session." 
description: " Uploads a file from the local machine and writes it to the working directory of the remote R session. This function is often used if a 'data' file needs to be accessed by a script running on the remote R session. " 
keywords: "mrsdeploy, putLocalFile" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 
 
 #`putLocalFile`: Upload a file from the local machine to the remote R session.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Uploads a file from the local machine and writes it to the working directory of
the remote R session. This function is often used if a 'data' file needs to be accessed
by a script running on the remote R session.
 
 
 ##Usage

```   
  putLocalFile(filename)
 
```
 
 ##Arguments

   
  
 ### `filename`
 Name of the file to copy. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
`TRUE` if successful.
 
 ##See Also
 
[getRemoteFile](getRemoteFile.md)

[deleteRemoteFile](deleteRemoteFile.md)

[listRemoteFiles](listRemoteFiles.md)
   
 ##Examples

 ```
   
  ## Not run:
 
putLocalFile("c:/data/test.csv")
putLocalFile("c:/data/test.png", as="raw")
 ## End(Not run) 
  
 
```
 
 
