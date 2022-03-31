--- 

# required metadata 
title: "rxSetComputeContext function (revoAnalytics) | Microsoft Docs" 
description: " Get or set the active compute context for RevoScaleR computations " 
keywords: "(revoAnalytics), rxSetComputeContext, rxGetComputeContext, IO" 
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



 # rxSetComputeContext: Get and Set the compute context 
 ## Description

Get or set the active compute context for RevoScaleR computations


 ## Usage

```   

  rxSetComputeContext(computeContext, ...) 

  rxGetComputeContext() 

```

 ## Arguments



 ### `computeContext`
 character string specifying class name or description of the specific  class to instantiate, or an existing `RxComputeContext` object.  Choices include: `"RxLocalSeq"` or `"local"`, `"RxLocalParallel"` or `"localpar"`,  `"RxSpark"` or `"spark"`,  `"RxHadoopMR"` or `"hadoopmr"`,    and `"RxForeachDoPar"` or `"dopar"`. 



 ### ` ...`
 any other arguments are passed to the class generator determined from `computeContext`. 




 ## Value

`rxSetComputeContext` returns the previously active compute context invisibly.
`rxGetComputeContext` returns the active compute context.



 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[RxComputeContext](RxComputeContext.md),
[rxOptions](rxOptions.md),
[rxGetOption](rxOptions.md)
[rxExec](rxExec.md).

 ## Examples

 ```

  ## Not run:

origComputeContext <- rxSetComputeContext("localpar")
x <- 1:10
rxExec(print, x, elemType = "cores", timesToRun = 10)
rxSetComputeContext( origComputeContext )
 ## End(Not run) 
```



