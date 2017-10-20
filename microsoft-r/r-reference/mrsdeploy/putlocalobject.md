--- 
 
# required metadata 
title: "putLocalObject function (mrsdeploy) " 
description: " Copy an object from the workspace of the local R session to the workspace  of the remote R session. " 
keywords: "(mrsdeploy), putLocalObject" 
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
 
 
 
 
 #putLocalObject: Copy an object from the local R session to the remote R session. 
 ##Description
 
Copy an object from the workspace of the local R session to the workspace 
of the remote R session.
 
 
 ##Usage

```   
  putLocalObject(obj, name = NULL)
 
```
 
 ##Arguments

   
  
 ### `obj`
 A character vector containing the names of the R Objects in the local R session to load in the remote R session 
  
  
  
 ### `name`
 The name of an R list object (created if necessary) in the remote R session that will contain the R objects from the local R session.  If `name` is `NULL`, then R objects from the local R session will be loaded in the GlobalEnv of the remote R session. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
list of R objects or `NULL`
 
 ##See Also
 
[getRemoteWorkspace](getRemoteWorkspace.md)

[getRemoteObject](getRemoteObject.md)

[putLocalWorkspace](putLocalWorkspace.md)
   
 ##Examples

 ```
   
  ## Not run:
 
aa<-rnorm(100)
bb<-rnorm(100)
putLocalObject(c("aa","bb"))
putLocalObject(c("aa","bb"), name="myObjsFromLocal")
 ## End(Not run) 
  
 
```
 
