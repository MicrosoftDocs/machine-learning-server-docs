--- 
 
# required metadata 
title: "Put an object from the local R session and load it into the remote R session." 
description: " Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)  " 
keywords: "mrsdeploy, putLocalObject" 
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
 
 
 
 
 #`putLocalObject`: Put an object from the local R session and load it into the remote R session.

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Usage

```   
  putLocalObject(obj, name = NULL)
 
```
 
 ##Arguments

   
  
 ### `name`
 The name of an R list object (created if necessary) in the remote R session that will contain the R objects from the local R session.  If `name` is `NULL`, then R objects from the local R session will be loaded in the GlobalEnv of the remote R session. 
  
  
  
 ### `x`
 A character vector containing the names of the R Objects in the local R session to load in the remote R session 
  
 
 
 ##Details
 
Put an object from the workspace of the local R session and load it into the workspace 
of the remote R session.
 
 
 ##Value
 
list of R objects or `NULL`
 
 ##See Also
 
[getRemoteWorkspace](getRemoteWorkspace.md)

[getRemoteObject](getRemoteObject.md)

[putLocalWorkspace](putLocalWorkspace.md)
   
 ##Examples

 ```
   
  ## Not run:
 
put_local_objet("x")
aa<-rnorm(100)
bb<-rnorm(100)
putLocalObject(c("aa","bb"))
putLocalObject(c("aa","bb"), name="myObjsFromLocal")
 ## End(Not run) 
  
 
```
 
 
