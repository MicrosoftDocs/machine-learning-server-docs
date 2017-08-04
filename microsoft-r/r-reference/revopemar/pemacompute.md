--- 
 
# required metadata 
title: " Comute PEMA " 
description: " Use a PemaBaseClass reference class object to perform a Parallel External Memory Algorithm computation. " 
keywords: "RevoPemaR, pemaCompute, models" 
author: "richcalaway"
ms.author: "richcala" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #pemaCompute:  Comute PEMA 

 Applies to version 8.0.3 of package RevoPemaR.
 
 ##Description
 
Use a PemaBaseClass reference class object to perform a Parallel External Memory Algorithm computation.
 
 
 ##Usage

```   
  pemaCompute(pemaObj, data = NULL, outData = NULL, overwrite = FALSE, append = "none",
      computeContext = NULL, initPema = TRUE, ...)
 
```
 
 ##Arguments

   
    
 ### pemaObj
  A [PemaBaseClass](pemabaseclass.md) reference class object containing the methods for the analysis.  
  
    
 ### data
  A data frame or **RevoScaleR** data source object.  
  
    
 ### outData
  An **RevoScaleR** data source object that has write capabilities, such as an .xdf file. Not used by all [PemaBaseClass](pemabaseclass.md) reference class objects.  
  
  
    
 ### overwrite
 logical value. If `TRUE`, an existing `outFile` will be overwritten. 
  
  
    
 ### append
 either `"none"` to create a new files, `"rows"` to append rows to an existing file, or `"cols"` to append columns to an existing file. If `outFile` exists and `append` is `"none"`,  the `overwrite` argument must be set to `TRUE`. Ignored when `outData` is not specified or not relevant. You cannot append to `RxTextData` or `RxTeradata` data sources,  and appending is not supported for composite .xdf files or when using the `RxHadoopMR` compute context. 
  
  
    
 ### computeContext
  `NULL` or a **RevoScaleR** compute context object.  
  
  
    
 ### initPema
  logical.  If `TRUE` the `initialize` method for the `pemaObj` object will be called before performing computations.  
  
  
    
 ###  ...
  Other fields in the `PemaBaseClass` class to be utilized in the analysis.  
  
 
 
 ##Details
 
The `pemaCompute` function provides a framework for writing parallel, external memory
algorithms (PEMAs) that can be run serially on a single computer, and will be automatically
parallelized when run on cluster supported by **RevoScaleR**.
 
 
 ##Value
 
The value returned that returned by the `PemaBaseClass` `processResults` method.
Note that the reference class `PemaBaseClass` will be reinitalized at the beginning
of the analysis unless `initPema` is set to `TRUE`, and will contain updated values upon completion.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[PemaBaseClass](pemabaseclass.md),
[PemaMean](pemamean.md),
[setPemaClass](setpemaclass.md)
   
 ##Examples

 ```
   
  
  # Instantiate an PemaMean reference class
  meanPemaObj <- PemaMean()
  meanPemaObj # See the initialized values of the fields
  
  # Compute the mean of Petal.Length from the iris data set
  # Call pemaCompute, specifying the custom analyis object, the data, and additional arg
  pemaCompute(pemaObj = meanPemaObj, data = iris, varName = "Petal.Length")
  
  meanPemaObj # Note that the reference class object has been updated
  
 
```
 
 
 
 
