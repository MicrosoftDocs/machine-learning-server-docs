--- 
 
# required metadata 
title: "tlcOneClassSVM function (MicrosoftML) | Microsoft Docs" 
description: " Not Available! " 
keywords: "(MicrosoftML), tlcOneClassSVM, AnomalyDetectorTrainer, Trainer" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/13/2017" 
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
 
 
 
 
 #tlcOneClassSVM: AnomalyDetectorTrainer, Trainer: 'OneClassSVM' 
 ##Description
 
Not Available!
 
 
 ##Usage

```   
  tlcOneClassSVM(kernel = "RbfKernel", cacheSize = 100, epsilon = 0.001,
    nu = 0.1, shrink = TRUE)
 
```
 
 ##Arguments

   
  
 ### `kernel`
 list: <name><options>. The kernel used for computing inner products Default value:'RbfKernel' (short form ker) 
  
  
  
 ### `cacheSize`
 double: <double>. Cache size, specified in megabytes Default value:'100' (short form cache) 
  
  
  
 ### `epsilon`
 double: <double>. Stopping tolerance Default value:'0.001' (short form eps) 
  
  
  
 ### `nu`
 double: <double>. This parameter determines the trade-off between the fraction of outliers and the number of support vectors Default value:'0.1' 
  
  
  
 ### `shrink`
 logical: [+|-]. This parameter determines whether or not to use the shrinking heuristic Default value:'+' 
  
  
  
 ### ` ...`
 : . hidden arguments 
  
 
 
 ##Details
 

 
 
 ##Value
 
a character string defining: OneClassSVM (AnomalyDetectorTrainer,
Trainer).
 
 ##Note
 

 
 
 ##Author(s)
 
Microsoft Corporation
 
 
 ##References
 
[`https://microsoft.sharepoint.com/teams/TLC`](https://microsoft.sharepoint.com/teams/TLC)

 
 
 ##See Also
 

   
 ##Examples

 ```
   
  
  ##---- Should be DIRECTLY executable !! ----
  ##-- ==>  Define data, use random,
  ##--	or do  help(data=index)  for the standard data sets.
  
  ## The function is currently defined as
  function (kernel = "RbfKernel", cacheSize = 100, epsilon = 0.001, 
      nu = 0.1, shrink = TRUE, ...) 
  {
      params <- character()
      params <- mluPasteArg(kernel, "list", params)
      params <- mluPasteArg(cacheSize, "double", params)
      params <- mluPasteArg(epsilon, "double", params)
      params <- mluPasteArg(nu, "double", params)
      params <- mluPasteArg(shrink, "logical", params)
      dotargs <- list(...)
      for (arg in names(dotargs)) {
          params <- mluPasteArg(dotargs[[arg]], "", params, arg)
      }
      params <- sprintf("OneClassSVM{%s}", params)
      return(structure(params, class = c("OneClassSvm", 
  		"AnomalyDetectorTrainer", "maml", "character")))
    }
  
 
```
 
 
 
 
