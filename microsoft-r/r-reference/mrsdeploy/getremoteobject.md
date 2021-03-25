--- 

# required metadata 
title: "getRemoteObject function (mrsdeploy) | Microsoft Docs" 
description: " Get an object from the workspace of the remote R session and load it into the workspace  of the local R session. " 
keywords: "(mrsdeploy), getRemoteObject" 
author: "dphansen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver"  
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "" 
ms.custom: "" 

--- 




 # getRemoteObject: Get an object from the remote R session. 
 ## Description

Get an object from the workspace of the remote R session and load it into the workspace 
of the local R session.


 ## Usage

```   
  getRemoteObject(obj, name = NULL)

```

 ## Arguments



 ### `obj`
 A character vector containing the names of the R objects in the remote R session  to load in the local R session. 



 ### `name`
 The name of an R list object (created if necessary) in the local R session that  will contain the R objects from the remote R session.  If `name` is `NULL`,  then R objects from the remote R session will be loaded in the GlobalEnv of the local R session. 



 ## Details

Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)



 ## Value

list of R objects or `NULL`

 ## See Also

[getRemoteWorkspace](getRemoteWorkspace.md)

[putLocalObject](putLocalObject.md)

[putLocalWorkspace](putLocalWorkspace.md)

 ## Examples

 ```

  ## Not run:

remoteExecute("x<-rnorm(10);y<-rnorm(10);model<-lm(x~y)", writePlots=TRUE)
getRemoteObject("model")
getRemoteObject("model", name="myObjsFromRemote")
 ## End(Not run) 
```

