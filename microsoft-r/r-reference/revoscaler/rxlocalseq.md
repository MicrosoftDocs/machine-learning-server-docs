--- 

# required metadata 
title: "RxLocalSeq function (revoAnalytics) | Microsoft Docs" 
description: " Creates a local compute context object.   This is the main generator for S4 class RxLocalSeq. Computations using rxExec will be processed sequentially. This is the default compute context. " 
keywords: "(revoAnalytics), RxLocalSeq, IO" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019 
ms.topic: "reference" 
ms.prod: "sql-non-specified"
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
ms.technology: machine-learning-server

--- 


 # RxLocalSeq: Generate Local Compute Context 
 ## Description

Creates a local compute context object.  
This is the main generator for S4 class RxLocalSeq. Computations using rxExec
will be processed sequentially. This is the default compute context.


 ## Usage

```   
  RxLocalSeq( object, dataPath = NULL, outDataPath = NULL )

```


 ## Arguments



 ### `object`
 a compute context object. If `object` has slots for   `dataPath` and/or `outDataPath`, they will be copied to the  equivalent slots for the new `RxLocalSeq` object. Explicit specifications  of the `dataPath` and/or outDataPath arguments will override this.  


 ### `dataPath`
 `NULL` or character vector defining the search path(s) for the input data source(s). If not `NULL`, it overrides any specification for `dataPath` in [rxOptions](rxOptions.md) 


 ### `outDataPath`
 `NULL` or character vector defining the search path(s) for  new output data file(s).  If not `NULL`, this overrides any specification for `dataPath`in [rxOptions](rxOptions.md)  




 ## Details

A job is associated with the compute context in effect at the time the job
was submitted. If the compute context subsequently changes, the compute context of the
job is not affected.

Note that `dataPath` and `outDataPath` are only utiltized by
data sources used in **RevoScaleR** analyses. They do not alter the
working directory for other R functions that read from or write to files.



 ## Value

object of class RxLocalSeq.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[rxSetComputeContext](rxSetComputeContext.md),
[rxExec](rxExec.md),
[rxOptions](rxOptions.md),
[RxComputeContext](RxComputeContext.md),
[RxLocalParallel](RxLocalParallel.md),
[RxLocalSeq-class](RxLocalSeq-class.md).


 ## Examples

 ```

  ## Not run:


# Create a local compute context object  
localContext <- RxLocalSeq()

# Tell RevoScaleR to use localContext compute context
rxSetComputeContext( localContext )

 ## End(Not run) 
```



