--- 
 
# required metadata 
title: "Stop Distributed Computing Engine" 
description: " `rxStopEngine` stops the remote Spark application. " 
keywords: "RevoScaleR, rxStopEngine, rxStopEngine,RxSpark-method, rxStopEngine,RxDistributedHpa-method, rxStopEngine,RxHadoopMR-method, rxStopEngine,RxHpcServer-method, rxStopEngine,RxInSqlServer-method, rxStopEngine,RxInTeradata-method, rxStopEngine,RxLsfCluster-method, computecontext" 
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
 
 
 
 
 
 
 
 
 
 #`rxStopEngine`: Stop Distributed Computing Engine

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
`rxStopEngine` stops the remote Spark application.
 
 
 ##Usage

```   
  rxStopEngine(computeContext,   ...  )
  rxStopEngine(computeContext, scope = "session")
 
```
 
 ##Arguments

   
    
 ### `computeContext`
 a valid [RxDistributedHpa-class](RxDistributedHpa-class.md). Currently only [RxSpark](RxSpark.md) is supported.  
  
  
    
 ### `scope`
 only used in `rxStopEngine` for `RxSpark`; a single `character` that takes the value of either:  
*   `"session"`: stop engine applications running in the current R session. 
*   `"user"`: stop engine applications running by the current user. 
  
  
 
 
 
 ##Details
 
This function stops distributed computing engine applications with
scope set to either "session" or "user". Specifically, for the
[RxSpark](RxSpark.md) compute context it stops the remote Spark
application(s).
 
 
 ##Value
 
No useful return value.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[RxSpark](RxSpark.md)
   
 ##Examples

 ```
   
  ## Not run:
 
rxStopEngine( computeContext )
rxStopEngine( computeContext , scope = "user" )
 ## End(Not run) 
  
 
```
 
 
