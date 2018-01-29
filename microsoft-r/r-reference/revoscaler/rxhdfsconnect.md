--- 
 
# required metadata 
title: "rxHdfsConnect function (revoAnalytics) | Microsoft Docs" 
description: " Establishes a connection from RevoScaleR to the Hadoop Distributed File System (HDFS).  " 
keywords: "(revoAnalytics), rxHdfsConnect, file, connection" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 #rxHdfsConnect:  Establish a Connection to the Hadoop Distributed File System  
 ##Description
 
Establishes a connection from RevoScaleR to the Hadoop Distributed
File System (HDFS). 
 
 
 ##Usage

```   
  rxHdfsConnect(hostName, portNumber)
 
```
 
 ##Arguments

   
    
 ### `hostName`
  character string specifying the host name of your Hadoop name node.  
  
    
 ### `portNumber`
  integer scalar specifying the port number of your Hadoop name node.  
  
 
 
 ##Details
 
If you accept the install time option to specify a default
HDFS connection, you are prompted for a host name and port number
for your Hadoop name node, which are stored as environment variables
`REVOHADOOPHOST` and `REVOHADOOPPORT`. If these environment
variables are set, this function is called by Rprofile.site on 
startup.

After establishing the connection, this function sets the
`rxOptions` for `hdfsHost` and `hdfsPort`, 
and subsequent calls to `RxHdfsFileSystem` should be done
using the default values of `hostName` and `port`.

It is important that this function be called 
before any functions that call into **rJava**, in particular
before initializing **rhdfs**.
 
 
 ##Value
 
invisibly, the return value of `rxOptions`.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[rxOptions](rxOptions.md), `link{RxHdfsFileSystem}`
   
 ##Examples

 ```
   
  ## Not run:
 
rxHdfsConnect(hostName = "sandbox-01", port = 8020)

myHDFS <- RxHdfsFileSystem()
 ## End(Not run) 
  
 
```
 
 
 
