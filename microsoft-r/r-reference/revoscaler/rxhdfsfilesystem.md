--- 
 
# required metadata 
title: "RxHdfsFileSystem function (RevoScaleR) " 
description: " This is the main generator for RxHdfsFileSystem S3 class. " 
keywords: "(RevoScaleR), RxHdfsFileSystem, print.RxHdfsFileSystem, file, connection" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 
 #RxHdfsFileSystem: RevoScaleR HDFS File System object generator 
 ##Description
 
This is the main generator for RxHdfsFileSystem S3 class.
 
 
 ##Usage

```   
  	RxHdfsFileSystem( object, hostName = rxGetOption("hdfsHost"), port = rxGetOption("hdfsPort"), useWebHdfs = FALSE, oAuthParameters = NULL, verbose = FALSE )
  	
  	 ## S3 method for class `RxHdfsFileSystem':
print  ( x, ... )
 
```
 
 ##Arguments

   
    
 ### `object`
 object of class RxHdfsFileSystem. This argument is optional. If supplied, the values of  the other arguments are used to replace those of `object` and the modified object is returned. If these arguments are not supplied, they will take their default values.  
  
    
 ### `hostName`
 character string specifying name of host for HDFS file system.  
  
    
 ### `port`
 integer specifying port number.  
  
    
 ### `useWebHdfs`
 NOT YET IMPLEMENTED Optional Flag indicating whether this is a HDFS or a WebHdfs interface - default FALSE  
  
    
 ### `oAuthParameters`
 NOT YET IMPLEMENTED Optional list of OAuth2 parameters created using rxOAuthParameters function  (valid only if useWebHdfs is TRUE) - default NULL  
  
    
 ### `verbose`
 Optional Flag indicating "verbose" mode for WebHdfs HTTP calls (valid only if useWebHdfs is TRUE) - default FALSE  
  
    
 ### `x`
 an RxHdfsFileSystem object.  
  
    
 ### ` ...`
 other arguments are passed to the underlying function.  
  
 
 
 ##Details
 
Writing to the HDFS file system can only be done using a [RxHadoopMR](RxHadoopMR.md)
compute context with an [RxXdfData](RxXdfData.md) data source. The 'rxHadoop' commands,
such as [rxHadoopCopy](rxHadoopCommand.md), can also be used to manipulate data sets in HDFS.
 
 
 
 ##Value
 
An RxHdfsFileSystem file system object. This object may be used to in
[rxSetFileSystem](rxSetFileSystem.md), [rxOptions](rxOptions.md), [RxTextData](RxTextData.md), or
[RxXdfData](RxXdfData.md) to set the file system.
 

 
 
 
 ##See Also
 
[RxFileSystem](RxFileSystem.md),
[RxNativeFileSystem](RxNativeFileSystem.md),
[rxSetFileSystem](rxSetFileSystem.md),
[rxOptions](rxOptions.md),
[RxXdfData](RxXdfData.md),
[RxTextData](RxTextData.md),
[rxOAuthParameters](rxOAuthParameters.md).
   
 ##Examples

 ```
   
  # Setup to run analyses to use HDFS file system
  ## Not run:
 
myHdfsFileSystem <- RxHdfsFileSystem(hostName = "myHost", port = 8020)
rxSetFileSystem(fileSystem = myHdfsFileSystem )
 ## End(Not run) 
  
 
```
 
 
 
