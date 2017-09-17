--- 
 
# required metadata 
title: "tlcMultiClassLogisticRegression function (MicrosoftML) | Microsoft Docs" 
description: " Not Available! " 
keywords: "(MicrosoftML), tlcMultiClassLogisticRegression, MulticlassLogisticRegressionPredictorNew, mlr, multilr, MultiClassClassifierTrainer, Trainer" 
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
 
 
 
 
 
 
 
 #tlcMultiClassLogisticRegression: MultiClassClassifierTrainer, Trainer: 'MultiClassLogisticRegression' 
 ##Description
 
Not Available!
 
 
 ##Usage

```   
  tlcMultiClassLogisticRegression(l2Weight = 1, l1Weight = 1,
    optTol = 1e-07, memorySize = 20, maxIterations = 2147483647,
    showTrainingStats = FALSE, sgdInitializationTolerance = 0,
    quiet = FALSE, initWtsDiameter = 0, numThreads = NULL,
    denseOptimizer = FALSE)
 
```
 
 ##Arguments

   
  
 ### `l2Weight`
 double: <float>. L2 regularization weight Default value:'1' (short form l2) 
  
  
  
 ### `l1Weight`
 double: <float>. L1 regularization weight Default value:'1' (short form l1) 
  
  
  
 ### `optTol`
 double: <float>. Tolerance parameter for optimization convergence. Lower = slower, more accurate Default value:'1E-07' (short form ot) 
  
  
  
 ### `memorySize`
 integer: <int>. Memory size for L-BFGS. Lower=faster, less accurate Default value:'20' (short form m) 
  
  
  
 ### `maxIterations`
 integer: <int>. Maximum iterations. Default value:'2147483647' (short form maxiter) 
  
  
  
 ### `showTrainingStats`
 logical: [+|-]. Include training statistics in model Default value:'-' 
  
  
  
 ### `sgdInitializationTolerance`
 double: <float>. Run SGD to initialize LR weights, converging to this tolerance Default value:'0' (short form sgd) 
  
  
  
 ### `quiet`
 logical: [+|-]. If set to true, produce no output during training. Default value:'-' (short form q) 
  
  
  
 ### `initWtsDiameter`
 double: <float>. Init weights diameter Default value:'0' (short form initwts) 
  
  
  
 ### `numThreads`
 integer: <int>. Number of threads (short form nt) 
  
  
  
 ### `denseOptimizer`
 logical: [+|-]. Force densification of the internal optimization vectors Default value:'-' (short form do) 
  
  
  
 ### ` ...`
 : . hidden arguments 
  
 
 
 ##Details
 

 
 
 ##Value
 
a character string defining: MultiClassLogisticRegression
(MultiClassClassifierTrainer, Trainer).
 
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
  function (l2Weight = 1, l1Weight = 1, optTol = 1e-07, memorySize = 20, 
      maxIterations = 2147483647, showTrainingStats = FALSE, sgdInitializationTolerance = 0, 
      quiet = FALSE, initWtsDiameter = 0, numThreads = NULL, denseOptimizer = FALSE, 
      ...) 
  {
      params <- character()
      params <- mluPasteArg(l2Weight, "double", params)
      params <- mluPasteArg(l1Weight, "double", params)
      params <- mluPasteArg(optTol, "double", params)
      params <- mluPasteArg(memorySize, "integer", params)
      params <- mluPasteArg(maxIterations, "integer", params)
      params <- mluPasteArg(showTrainingStats, "logical", params)
      params <- mluPasteArg(sgdInitializationTolerance, "double", params)
      params <- mluPasteArg(quiet, "logical", params)
      params <- mluPasteArg(initWtsDiameter, "double", params)
      params <- mluPasteArg(numThreads, "integer", params)
      params <- mluPasteArg(denseOptimizer, "logical", params)
      dotargs <- list(...)
      for (arg in names(dotargs)) {
          params <- mluPasteArg(dotargs[[arg]], "", params, arg)
      }
      params <- sprintf("MultiClassLogisticRegression{%s}", params)
      return(structure(params, class = c("MultiClassLogisticRegression", 
          "MultiClassClassifierTrainer", "maml", "character")))
    }
  
 
```
 
 
 
 
