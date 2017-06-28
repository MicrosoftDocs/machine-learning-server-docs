--- 
 
# required metadata 
title: "Get and Set the compute context" 
description: " Get or set the active compute context for RevoScaleR computations " 
keywords: "RevoScaleR, rxSetComputeContext, rxGetComputeContext, IO" 
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
 
 
 
 #`rxSetComputeContext`: Get and Set the compute context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Get or set the active compute context for RevoScaleR computations
 
 
 ##Usage

```   
  
  rxSetComputeContext(computeContext, ...) 
               
  rxGetComputeContext() 
 
```
 
 ##Arguments

   
    
 ### `computeContext`
 character string specifying class name or description of the specific  class to instantiate, or an existing `RxComputeContext` object.  Choices include: `"RxLocalSeq"` or `"local"`, `"RxLocalParallel"` or `"localpar"`,  `"RxSpark"` or `"spark"`,  `"RxHadoopMR"` or `"hadoopmr"`,   `"RxInTeradata"` or `"teradata"`,  and `"RxForeachDoPar"` or `"dopar"`. 
  
  
    
 ### ` ...`
 any other arguments are passed to the class generator determined from `computeContext`. 
  
 
 
 
 ##Value
 
`rxSetComputeContext` returns the previously active compute context invisibly.
`rxGetComputeContext` returns the active compute context.
 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxComputeContext](rxcomputecontext.md),
[rxOptions](rxoptions.md),
[rxGetOption](rxoptions.md)
[rxExec](rxexec.md).
   
 ##Examples

 ```
   
  ## Not run:
 
origComputeContext <- rxSetComputeContext("localpar")
x <- 1:10
rxExec(print, x, elemType = "cores", timesToRun = 10)
rxSetComputeContext( origComputeContext )
 ## End(Not run) 
  
  
 
```
 
 
 
