--- 
 
# required metadata 
title: " Finds where in a given path a file is. " 
description: " Sequentially checks the entries in a delimited path string for a provided file name. " 
keywords: "RevoScaleR, rxFindFileInPath, IO" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #rxFindFileInPath:  Finds where in a given path a file is. 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Sequentially checks the entries in a delimited path string for a provided file name.
 
 
 
 ##Usage

```   
  rxFindFileInPath( path, fileName )
 
```
 
 
 ##Arguments

   
  
  
 ### path
 character vector, compute context or job object.  This is a required parameter.   If a compute context is provided, the `dataPath` from that context (assuming it has one ) will be used;  if a job object is provided, that job object's compute context's `dataPath` will be used. If a character vector is provided, each element of the vector should contain one directory path.   If a character scalar is supplied, multiple directory paths can be contained by using the standard system  delimiters (":" for Linux and ";") to separate the entries within the path.  This allows system PATH's to  be parsed using this function. 
  
  
  
 ### fileName
 logical scalar.  This is a required parameter.  The name of the file being sought along the `path`. 
  
  
 
 
 
 ##Details
 
This function will sequentially check the locations (directories) provided in the `path`.  Thus, if there exists more than one instance of a file
with the same `fileName`, the first instance of a directory in the path containing the file will be the one returned.
 
 
 
 ##Value
 
Returns NULL if the target file is not found in any of the possible locations, or the path to the file (not including the file name) if it is found.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##Examples

 ```
   
  ## Not run:
 
# using a Linux environment PATH
rxFindFileInPath(path=Sys.getenv("PATH"), fileName="Revoscript")

# using a compute context
rxFindFileInPath(path=myComputeContext, fileName="myData" )

# for Windows
rxFindFileInPath(path="C:\data\remember\to\escape\backslashes;D:\other\data", fileName="myData" )

# for Linux
rxFindFileInPath(path="/mnt/data:/home/myName/data", fileName="myData" )

# using a vector of paths
rxFindFileInPath( path=c("/dir1","/dir2",/dir3/dir4"), fileName="myData" )
 ## End(Not run) 
  
 
```
 
 
