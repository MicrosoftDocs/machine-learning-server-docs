--- 
 
# required metadata 
title: "rxStopEngine function (revoAnalytics) | Microsoft Docs" 
description: " rxStopEngine stops the remote Spark application. " 
keywords: "(revoAnalytics), rxStopEngine, rxStopEngine,RxSpark-method, rxStopEngine,RxDistributedHpa-method, rxStopEngine,RxHadoopMR-method, rxStopEngine,RxInSqlServer-method, rxStopEngine,RxLsfCluster-method, computecontext" 
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
 
 
 
 
 
 
 
 
 #rxStopEngine: Stop Distributed Computing Engine 
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
 
 
