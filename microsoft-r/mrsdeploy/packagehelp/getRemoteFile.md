--- 
 
# required metadata 
title: "Copy a file from the working directory of the remote R session." 
description: " Downloads the file from the working directory of the remote R session into the working direcoty of the local R session. " 
keywords: "mrsdeploy, getRemoteFile" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 
 #`getRemoteFile`: Copy a file from the working directory of the remote R session.

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Downloads the file from the working directory of the remote R session into the working
direcoty of the local R session.
 
 
 ##Usage

```   
  getRemoteFile(filename, as = "text")
 
```
 
 ##Arguments

   
  
 ### `filename`
 Name of the file to copy. 
  
  
  
 ### `as`
 The content type of the file ("text" or "raw").  For binary files use 'raw'. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
`TRUE` if successful.
 
 ##See Also
 
[deleteRemoteFile](deleteRemoteFile.md)

[listRemoteFiles](listRemoteFiles.md)

[putLocalFile](putLocalFile.md)
   
 ##Examples

 ```
   
  ## Not run:
 
getRemoteFile("c:/data/test.csv")
getRemoteFile("c:/data/test.png", as="raw")
 ## End(Not run) 
  
 
```
 
 
