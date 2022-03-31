--- 

# required metadata 
title: "RxForeachDoPar function (revoAnalytics) | Microsoft Docs" 
description: " Creates a compute context object using the registered foreach parallel back end. This compute context can be used only to distribute computations via the [rxExec](rxExec.md) function; it is ignored by Revolution HPA functions. This is the main generator for S4 class RxForeachDoPar. " 
keywords: "(revoAnalytics), RxForeachDoPar, IO" 
author: "chuckheinzelman"
ms.author: "charlhe" 
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
#ms.technology: "" 
ms.custom: "" 

--- 


 # RxForeachDoPar: Generate RxForeachDoPar Compute Context 
 ## Description

Creates a compute context object using the registered `foreach`
parallel back end. This compute context can be used only to distribute computations
via the [rxExec](rxExec.md) function; it is ignored by Revolution HPA functions.
This is the main generator for S4 class RxForeachDoPar.


 ## Usage

```   
  RxForeachDoPar( object, dataPath = NULL, outDataPath = NULL )

```


 ## Arguments




 ### `object`
 a compute context object. If `object` has slots for   `dataPath` and/or `outDataPath`, they will be copied to the  equivalent slots for the new `RxForeachDoPar` object. Explicit specifications  of the `dataPath` and/or outDataPath arguments will override this.   



 ### `dataPath`
 `NULL` or character vector defining the search path(s) for the input data source(s).  If not `NULL`, it overrides any specification for `dataPath` in [rxOptions](rxOptions.md) 



 ### `outDataPath`
 `NULL` or character vector defining the search path(s) for   new output data file(s).  If not `NULL`, this overrides any specification for `dataPath` in [rxOptions](rxOptions.md)  




 ## Details

A job is associated with the compute context in effect at the time the job
was submitted. If the compute context subsequently changes, the compute context of the
job is not affected.

Available parallel backends are system-dependent, but include `doRSR` and `doParallel`. 
These are separate packages that must be
loaded and registered to accept `foreach` input.

Note that `dataPath` and `outDataPath` are only used by
data sources used in **RevoScaleR** analyses. They do not alter the
working directory for other R functions that read from or write to files. 



 ## Value

object of class RxForeachDoPar.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

foreach,
doParallel-package,
registerDoParallel,
doRSR-package,
registerDoRSR,
[rxSetComputeContext](rxSetComputeContext.md),
[rxOptions](rxOptions.md),
[rxExec](rxExec.md),
[RxComputeContext](RxComputeContext.md),
[RxLocalSeq](RxLocalSeq.md),
[RxLocalParallel](RxLocalParallel.md),
[RxForeachDoPar-class](RxForeachDoPar-class.md).


 ## Examples

 ```

  ## Not run:


# Create a compute context using your registered foreach backend
doparContext <- RxForeachDoPar()

# Tell RevoScaleR to use the RxForeachDoPar compute context
rxSetComputeContext(doparContext)

 ## End(Not run) 
```



