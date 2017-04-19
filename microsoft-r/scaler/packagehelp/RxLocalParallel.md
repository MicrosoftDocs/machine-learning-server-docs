--- 
 
# required metadata 
title: "Generate Local Parallel Compute Context" 
description: " Creates a local compute context object that uses the doParallel back-end for HPC computations  performed using rxExec.  This compute context can be used only to distribute computations via the [rxExec](rxExec.md) function; it is ignored by Revolution HPA functions. This is the main generator for S4 class RxLocalParallel. " 
keywords: "RevoScaleR, RxLocalParallel, IO" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 #`RxLocalParallel`: Generate Local Parallel Compute Context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Creates a local compute context object that uses the doParallel back-end for HPC computations 
performed using rxExec.  This compute context can be used only to distribute computations
via the [rxExec](rxExec.md) function; it is ignored by Revolution HPA functions. This is the main generator for S4 class RxLocalParallel.
 
 
 ##Usage

```   
  RxLocalParallel( object, dataPath = NULL, outDataPath = NULL  )
 
```
 
 
 ##Arguments

   
    
 ### `object`
 a compute context object. If `object` has slots for   `dataPath` and/or `outDataPath`, they will be copied to the  equivalent slots for the new `RxLocalParallel` object. Explicit specifications  of the `dataPath` and/or outDataPath arguments will override this.  
  
   
    
 ### `dataPath`
 `NULL` or character vector defining the search path(s) for the input data source(s).  If not `NULL`, it overrides any specification for `dataPath` in [rxOptions](rxOptions.md) 
   
  
    
 ### `outDataPath`
 `NULL` or character vector defining the search path(s) for   new output data file(s).  If not `NULL`, this overrides any specification for `dataPath` in [rxOptions](rxOptions.md)  
   
 
 
 
 ##Details
 
A job is associated with the compute context in effect at the time the job
was submitted. If the compute context subsequently changes, the compute context of the
job is not affected.

Note that `dataPath` and `outDataPath` are only utiltized by
data sources used in **RevoScaleR** analyses. They do not alter the
working directory for other R functions that read from or write to files.
 
 
 
 ##Value
 
object of class RxLocalParallel.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxSetComputeContext](rxSetComputeContext.md),
[rxExec](rxExec.md),
[rxOptions](rxOptions.md),
[RxComputeContext](RxComputeContext.md),
[RxLocalSeq](RxLocalSeq.md),
[RxForeachDoPar](RxForeachDoPar.md),
[RxLocalParallel-class](RxLocalParallel-class.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
  
# Create a RxLocalParallel object to use with rxExec  
parallelContext <- RxLocalParallel()

# Tell RevoScaleR to use parallelContext compute context
rxSetComputeContext( parallelContext )

# Subsequent calls to rxExec will use the doParallel package 
# behind the scenes.

 ## End(Not run) 
  
 
```
 
 
 
