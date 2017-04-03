--- 
 
# required metadata 
title: "RevoScaleR Native File System Object Generator" 
description: " This is the main generator for RxNativeFileSystem S3 class. " 
keywords: "RevoScaleR, RxNativeFileSystem, file, connection" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 #`RxNativeFileSystem`: RevoScaleR Native File System Object Generator

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
This is the main generator for RxNativeFileSystem S3 class.
 
 
 ##Usage

```   
  	RxNativeFileSystem()
 
```
 
 
 ##Value
 
An RxNativeFileSystem file system object. This object may be used in
[rxSetFileSystem](rxSetFileSystem.md), [rxOptions](rxOptions.md), [RxTextData](RxTextData.md), or
[RxXdfData](RxXdfData.md) to set the file system.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxFileSystem](RxFileSystem.md),
[RxHdfsFileSystem](RxHdfsFileSystem.md),
[rxSetFileSystem](rxSetFileSystem.md),
[rxOptions](rxOptions.md),
[RxXdfData](RxXdfData.md),
[RxTextData](RxTextData.md).
   
 ##Examples

 ```
   
  # Setup to run analyses to use HDFS file system, then native
  ## Not run:
 
myHdfsFileSystem <- RxHdfsFileSystem(hostName = "myHost", port = 8020)
rxSetFileSystem(fileSystem = myHdfsFileSystem )
# Perform other tasks
# Reset to native file system
rxSetFileSystem(fileSystem = RxNativeFileSystem())
 ## End(Not run) 
  
 
```
 
 
 
