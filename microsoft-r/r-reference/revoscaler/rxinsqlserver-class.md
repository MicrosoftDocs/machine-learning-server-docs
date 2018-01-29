--- 
 
# required metadata 
title: "RxInSqlServer-class class (revoAnalytics) | Microsoft Docs" 
description: " Creates a compute context for running Microsoft R Server analyses inside Microsoft SQL Server.  Currently only supported in Windows. " 
keywords: "(revoAnalytics), RxInSqlServer-class, doPreJobValidation,RxInSqlServer-method, initialize,RxInSqlServer-method, show,RxInSqlServer-method, classes" 
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
 
 
 
 
 
 
 #RxInSqlServer-class: Class RxInSqlServer 
 ##Description
 
Creates a compute context for running Microsoft R Server analyses inside Microsoft SQL Server.

Currently only supported in Windows.
 
 
 ## Objects from the Class 

 
Objects can be created by calls of the form .

 
 ## Slots 

 


###`connectionString`:
Object of class `"character"` ~~ 



###`wait`:
Object of class `"logical"` ~~ 


###`consoleOutput`:
Object of class `"logical"` ~~ 


###`autoCleanup`:
Object of class `"logical"` ~~ 




###`dataPath`:
Object of class `"characterORNULL"` ~~ 



###`packagesToLoad`:
Object of class `"characterORNULL"` ~~ 



###`executionTimeout`:
Object of class `"numeric"` ~~ 


###`description`:
Object of class `"character"` ~~ 


###`version`:
Object of class `"character"` ~~ 



 
 ## Extends 

 
Class RxDistributedHpa, directly.
Class RxComputeContext, by class "RxDistributedHpa", distance 2.
Class RxComputeContext, by class "RxLocalSeq", distance 1.
 
 ## Methods 

 


###`doPreJobValidation`
`signature(object = "RxInSqlServer")`: ... 


###`initialize`
`signature(.Object = "RxInSqlServer")`: ... 


###`show`
`signature(object = "RxInSqlServer")`: ... 



 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxInSqlServer](RxInSqlServer.md),
[RxSqlServerData](RxSqlServerData.md)
   
 ##Examples

 ```
   
  showClass("RxInSqlServer")
 
```
 
 
