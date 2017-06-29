--- 
 
# required metadata 
title: "Get the content of a file from remote R session." 
description: " Get the content of a file from the working directory of the remote R session. " 
keywords: "mrsdeploy, getRemoteFile" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 
 
 #`getRemoteFile`: Get the content of a file from remote R session.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Get the content of a file from the working directory of the remote R session.
 
 
 ##Usage

```   
  getRemoteFile(filename, as = "text")
 
```
 
 ##Arguments

   
  
 ### `filename`
 Name of the file in the remote working directory. 
  
  
  
 ### `as`
 The content type of the file ("text" or "raw").  For binary files use 'raw'. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
raw vector of bytes if `as="raw"`. Otherwise, the text content of the file, 
or `NULL` if the file content cannot be retrieved.
 
 ##See Also
 
[deleteRemoteFile](deleteremotefile.md)

[listRemoteFiles](listremotefiles.md)

[putLocalFile](putlocalfile.md)
   
 ##Examples

 ```
   
  ## Not run:
 
content<-getRemoteFile("test.csv")

raw_content<-getRemoteFile("test.png", as="raw")
fh = file("test.png", "wb")
writeBin(raw_content, fh)
close(fh)
 ## End(Not run) 
  
 
```
 
 
