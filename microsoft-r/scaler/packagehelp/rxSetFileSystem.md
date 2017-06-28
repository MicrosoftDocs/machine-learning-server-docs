--- 
 
# required metadata 
title: "Set and Get RevoScaleR File System" 
description: " Set and get the default file system for RevoScaleR operations. " 
keywords: "RevoScaleR, rxSetFileSystem, rxGetFileSystem, file, connection" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 #`rxSetFileSystem`: Set and Get RevoScaleR File System

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Set and get the default file system for RevoScaleR operations.
 
 
 ##Usage

```   
  	rxSetFileSystem( fileSystem, ...)
  	rxGetFileSystem(x = NULL)
 
```
 
 ##Arguments

   
    
 ### `fileSystem`
 character string specifying class name, file system type, or  existing `RxFileSystem` object.  Choices include: "RxNativeFileSystem" or "native", or "RxHdfsFileSystem" or "hdfs". Optional arguments `hostName` and `port` may be specified for HDFS file systems.  
  
    
 ### ` ...`
 other arguments are passed to the underlying class generator.  
  
  
    
 ### `x`
 optional [RxXdfData](RxXdfData.md) or [RxTextData](RxTextData.md) object from which to retrieve the file system object.  
  
 
 
 
 ##Value
 
If `x` is a file system or is a data source object that contains one, that RxFileSystem object is returned.
Next, if the compute context contains a file system, that RxFileSystem object is returned.
If no file system object has been found, the previously set RxFileSystem file system object is returned.
The file system object is returned invisibly.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxFileSystem](../../r-reference/revoscaler/rxfilesystem.md),
[RxNativeFileSystem](../../r-reference/revoscaler/rxnativefilesystem.md),
[RxHdfsFileSystem](../../r-reference/revoscaler/rxhdfsfilesystem.md),
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
myHdfsFileSystem2 <- RxHdfsFileSystem( hostName = "default", port = 0)
rxSetFileSystem( fileSystem = myHdfsFileSystem2)

# Example 3
rxSetFileSystem(fileSystem = "hdfs", hostName = "myHost", port = 8020)

 ## End(Not run) 
  
  # Specify a file system in a text data source object
  hdfsFS1 <- RxHdfsFileSystem()
  textDS <- RxTextData(file = "myfile.txt", fileSystem = hdfsFS1)
  
  # Retrieve the file system information
  rxGetFileSystem(textDS)
 
```
 
 
 
