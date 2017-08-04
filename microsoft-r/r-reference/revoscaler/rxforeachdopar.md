--- 
 
# required metadata 
title: "Generate RxForeachDoPar Compute Context" 
description: " Creates a compute context object using the registered `foreach` parallel back end. This compute context can be used only to distribute computations via the [rxExec](rxexec.md) function; it is ignored by Revolution HPA functions. This is the main generator for S4 class RxForeachDoPar. " 
keywords: "RevoScaleR, RxForeachDoPar, IO" 
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
 
 
 #RxForeachDoPar: Generate RxForeachDoPar Compute Context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Creates a compute context object using the registered `foreach`
parallel back end. This compute context can be used only to distribute computations
via the [rxExec](rxexec.md) function; it is ignored by Revolution HPA functions.
This is the main generator for S4 class RxForeachDoPar.
 
 
 ##Usage

```   
  RxForeachDoPar( object, dataPath = NULL, outDataPath = NULL )
 
```
 
 
 ##Arguments

   
  
    
 ### object
 a compute context object. If `object` has slots for   `dataPath` and/or `outDataPath`, they will be copied to the  equivalent slots for the new `RxForeachDoPar` object. Explicit specifications  of the `dataPath` and/or outDataPath arguments will override this.   
  
   
    
 ### dataPath
 `NULL` or character vector defining the search path(s) for the input data source(s).  If not `NULL`, it overrides any specification for `dataPath` in [rxOptions](rxoptions.md) 
   
  
    
 ### outDataPath
 `NULL` or character vector defining the search path(s) for   new output data file(s).  If not `NULL`, this overrides any specification for `dataPath` in [rxOptions](rxoptions.md)  
   
 
 
 
 ##Details
 
A job is associated with the compute context in effect at the time the job
was submitted. If the compute context subsequently changes, the compute context of the
job is not affected.

Available parallel backends are system-dependent, but include `doRSR` and `doParallel`. 
These are separate packages that must be
loaded and registered to accept `foreach` input.

Note that `dataPath` and `outDataPath` are only used by
data sources used in **RevoScaleR** analyses. They do not alter the
working directory for other R functions that read from or write to files. 
 
 
 
 ##Value
 
object of class RxForeachDoPar.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
foreach,
doParallel-package,
registerDoParallel,
doRSR-package,
registerDoRSR,
[rxSetComputeContext](rxsetcomputecontext.md),
[rxOptions](rxoptions.md),
[rxExec](rxexec.md),
[RxComputeContext](rxcomputecontext.md),
[RxLocalSeq](rxlocalseq.md),
[RxLocalParallel](rxlocalparallel.md),
[RxForeachDoPar-class](rxforeachdopar-class.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
  
# Create a compute context using your registered foreach backend
doparContext <- RxForeachDoPar()

# Tell RevoScaleR to use the RxForeachDoPar compute context
rxSetComputeContext(doparContext)

 ## End(Not run) 
  
 
```
 
 
 
