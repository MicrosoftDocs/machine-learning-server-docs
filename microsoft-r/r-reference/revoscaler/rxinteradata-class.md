--- 
 
# required metadata 
title: "Class RxInTeradata" 
description: " Creates a compute context for running Microsoft R Server analyses inside a Teradata database cluster. " 
keywords: ", RxInTeradata-class, doPreJobValidation,RxInTeradata-method, initialize,RxInTeradata-method, show,RxInTeradata-method, classes" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "08/04/2017" 
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
 
 
 
 
 
 
 #RxInTeradata-class: Class RxInTeradata 
 ##Description
 
Creates a compute context for running Microsoft R Server analyses inside a Teradata database cluster.
 
 
 ## Objects from the Class 

 
Objects can be created by calls of the form .

 
 ## Slots 

 


###`remoteShareDir`:
Object of class `"character"` ~~ 


###`connectionString`:
Object of class `"character"` ~~ 


###`shareDir`:
Object of class `"character"` ~~ 


###`revoPath`:
Object of class `"characterORNULL"` ~~ 


###`wait`:
Object of class `"logical"` ~~ 


###`consoleOutput`:
Object of class `"logical"` ~~ 


###`autoCleanup`:
Object of class `"logical"` ~~ 


###`configFile`:
Object of class `"characterORNULL"` ~~ 


###`workingDir`:
Object of class `"characterORNULL"` ~~ 


###`dataPath`:
Object of class `"characterORNULL"` ~~ 


###`minElems`:
Object of class `"numeric"` ~~ 


###`maxElems`:
Object of class `"numeric"` ~~ 


###`exclusive`:
Object of class `"logical"` ~~ 


###`nodes`:
Object of class `"characterORNULL"` ~~ 


###`dataDistType`:
Object of class `"character"` ~~ 


###`packagesToLoad`:
Object of class `"characterORNULL"` ~~ 


###`email`:
Object of class `"characterORNULL"` ~~ 


###`resultsTimeout`:
Object of class `"numeric"` ~~ 


###`description`:
Object of class `"character"` ~~ 


###`version`:
Object of class `"character"` ~~ 



 
 ## Extends 

 
Class RxDistributedHpa, directly.
Class RxComputeContext, by class "RxDistributedHpa", distance 2.
 
 ## Methods 

 


###`doPreJobValidation`
`signature(object = "RxInTeradata")`: ... 


###`initialize`
`signature(.Object = "RxInTeradata")`: ... 


###`show`
`signature(object = "RxInTeradata")`: ... 



 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxInTeradata](RxInTeradata.md),
[RxTeradata](RxTeradata.md)
   
 ##Examples

 ```
   
  showClass("RxInTeradata")
 
```
 
 
