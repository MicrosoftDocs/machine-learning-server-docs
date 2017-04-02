--- 
 
# required metadata 
title: "RevoScaleR File System object generator" 
description: " This is the main generator for RxFileSystem S3 classes. " 
keywords: "RevoScaleR, RxFileSystem, print.RxFileSystem, file, connection" 
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
 
 
 
 #`RxFileSystem`: RevoScaleR File System object generator

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
This is the main generator for RxFileSystem S3 classes.
 
 
 ##Usage

```   
  	RxFileSystem( fileSystem, ...)
  	
  	 ## S3 method for class `RxFileSystem':
print  ( x, ... )
 
```
 
 ##Arguments

   
    
 ### `fileSystem`
 character string specifying class name or file system type  existing `RxFileSystem` object.  Choices include: "RxNativeFileSystem" or "native", or "RxHdfsFileSystem" or "hdfs". Optional arguments `hostName` and `port` may be specified for HDFS file systems.  
  
    
 ### `x`
 an RxFileSystem object.  
  
    
 ### ` ...`
 other arguments are passed to the underlying function.  
  
 
 
 ##Details
 
This is a wrapper to specific generator functions for the
RevoScaleR file system classes. For example, the RxHdfsFileSystem class uses function
[RxHdfsFileSystem](RxHdfsFileSystem.md) as a generator. Therefore either `RxHdfsFileSystem()`
or `RxFileSystem("hdfs")` will create an RxHdfsFileSystem object.
 
 
 ##Value
 
A type of RxFileSystem file system object. This object may be used in
[rxSetFileSystem](rxSetFileSystem.md), [rxOptions](rxOptions.md), [RxTextData](RxTextData.md), or
[RxXdfData](RxXdfData.md) to set the file system.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxNativeFileSystem](RxNativeFileSystem.md),
[RxHdfsFileSystem](RxHdfsFileSystem.md),
[rxSetFileSystem](rxSetFileSystem.md),
[rxOptions](rxOptions.md),
[RxXdfData](RxXdfData.md),
[RxTextData](RxTextData.md).
   
 ##Examples

 ```
   
  # Setup to run analyses to use HDFS file system
  ## Not run:
 
# Example 1
myHdfsFileSystem1 <- RxFileSystem(fileSystem = "hdfs")
rxSetFileSystem(fileSystem = myHdfsFileSystem1 )

# Example 2
myHdfsFileSystem2 <- RxFileSystem(fileSystem = "hdfs", hostName = "myHost", port = 8020)
rxSetFileSystem(fileSystem = myHdfsFileSystem2 )
 ## End(Not run) 
  
 
```
 
 
 
