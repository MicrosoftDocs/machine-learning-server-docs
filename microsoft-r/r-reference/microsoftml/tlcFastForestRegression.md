--- 
 
# required metadata 
title: "tlcFastForestRegression function (MicrosoftML) | Microsoft Docs" 
description: " Trains a random forest to fit target values using least-squares. " 
keywords: "(MicrosoftML), tlcFastForestRegression, ffr, RegressorTrainer, Trainer" 
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
 
 
 
 
 
 #tlcFastForestRegression: RegressorTrainer, Trainer: 'FastForestRegression' 
 ##Description
 
Trains a random forest to fit target values using least-squares.
 
 
 ##Usage

```   
  tlcFastForestRegression(quantileSampleCount = 100, featureFraction = 0.7,
    baggingTrainFraction = 0.7, splitFraction = 0.7, numThreads = 8,
    rngSeed = 123, entropyCoefficient = 0, histogramPoolSize = -1,
    diskTranspose = FALSE, maxBins = 255, sparsifyThreshold = 0.7,
    featureFirstUsePenalty = 0, featureReusePenalty = 0,
    gainConfidenceLevel = 0, softmaxTemperature = 0, executionTimes = FALSE,
    numLeaves = 20, minDocumentsInLeafs = 10, numTrees = 100,
    smoothing = 0, testFrequency = 2147483647, baggingSize = 1)
 
```
 
 ##Arguments

   
  
 ### `quantileSampleCount`
 integer: <int>. Number of labels to be sampled from each leaf to make the distribtuion Default value:'100' (short form qsc) 
  
  
  
 ### `featureFraction`
 double: <float>. The fraction of features (chosen randomly) to use on each iteration Default value:'0.7' (short form ff) 
  
  
  
 ### `baggingTrainFraction`
 double: <float>. The fraction of Instances (chosen randomly) to use on each iteration Default value:'0.7' (short form bagfrac) 
  
  
  
 ### `splitFraction`
 double: <float>. The fraction of features (chosen randomly) to use on each split Default value:'0.7' (short form sf) 
  
  
  
 ### `numThreads`
 integer: <int>. The number of threads to use Default value:'8' (short form t) 
  
  
  
 ### `rngSeed`
 integer: <int>. The seed of the random number generator Default value:'123' (short form r1) 
  
  
  
 ### `entropyCoefficient`
 double: <float>. The entropy (regularization) coefficient between 0 and 1 Default value:'0' (short form e) 
  
  
  
 ### `histogramPoolSize`
 integer: <int>. The number of histograms in the pool (between 2 and numLeaves) Default value:'-1' (short form ps) 
  
  
  
 ### `diskTranspose`
 logical: [+|-]. Whether to utilize the disk when performing the transpose Default value:'-' (short form dt) 
  
  
  
 ### `maxBins`
 integer: <int>. Maximum number of distinct values (bins) per feature Default value:'255' (short form mb) 
  
  
  
 ### `sparsifyThreshold`
 double: <float>. Sparsity level needed to use sparse feature representation Default value:'0.7' (short form sp) 
  
  
  
 ### `featureFirstUsePenalty`
 double: <float>. The feature first use penalty coefficient Default value:'0' (short form ffup) 
  
  
  
 ### `featureReusePenalty`
 double: <float>. The feature re-use penalty (regularization) coefficient Default value:'0' (short form frup) 
  
  
  
 ### `gainConfidenceLevel`
 double: <float>. Tree fitting gain confidence requirement (should be in the range [0,1) ). Default value:'0' (short form gainconf) 
  
  
  
 ### `softmaxTemperature`
 double: <float>. The temperature of the randomized softmax distribution for choosing the feature Default value:'0' (short form smtemp) 
  
  
  
 ### `executionTimes`
 logical: [+|-]. Print execution time breakdown to stdout Default value:'-' (short form et) 
  
  
  
 ### `numLeaves`
 integer: <int>. The max number of leaves in each regression tree Default value:'20' (short form nl) 
  
  
  
 ### `minDocumentsInLeafs`
 integer: <int>. The minimal number of documents allowed in a leaf of a regression tree, out of the subsampled data Default value:'10' (short form mil) 
  
  
  
 ### `numTrees`
 integer: <int>. Number of weak hypotheses in the ensemble Default value:'100' (short form iter) 
  
  
  
 ### `smoothing`
 double: <float>. Smoothing paramter for tree regularization Default value: '0.0' (short form s) 
  
  
  
 ### `testFrequency`
 integer: <int>. Calculate metric values for train/valid/test  every k rounds Default value: '2147483647' (short form tf) 
  
  
  
 ### `baggingSize`
 integer: <int>. Number of trees in each bag (0 for disabling bagging) Default value: '0' (short form bag) 
  
  
  
 ### ` ...`
 : . hidden arguments 
  
 
 
 ##Details
 

 
 
 ##Value
 
a character string defining: FastForestRegression (RegressorTrainer,
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
  function (quantileSampleCount = 100, 
      featureFraction = 0.7, baggingTrainFraction = 0.7, splitFraction = 0.7, 
      numThreads = 8, rngSeed = 123, entropyCoefficient = 0, 
      histogramPoolSize = -1, diskTranspose = FALSE, maxBins = 255, 
      sparsifyThreshold = 0.7, featureFirstUsePenalty = 0, featureReusePenalty = 0, 
      gainConfidenceLevel = 0, softmaxTemperature = 0, executionTimes = FALSE, 
      numLeaves = 20, minDocumentsInLeafs = 10, numTrees = 100, smoothing = 0.0,
      testFrequency = 2147483647, baggingSize = 1, ...) 
  {
      params <- character()
      params <- mluPasteArg(quantileSampleCount, "integer", params)
      params <- mluPasteArg(featureFraction, "double", params)
      params <- mluPasteArg(baggingTrainFraction, "double", params)
      params <- mluPasteArg(splitFraction, "double", params)
      params <- mluPasteArg(numThreads, "integer", params)
      params <- mluPasteArg(rngSeed, "integer", params)
      params <- mluPasteArg(entropyCoefficient, "double", params)
      params <- mluPasteArg(histogramPoolSize, "integer", params)
      params <- mluPasteArg(diskTranspose, "logical", params)
      params <- mluPasteArg(maxBins, "integer", params)
      params <- mluPasteArg(sparsifyThreshold, "double", params)
      params <- mluPasteArg(featureFirstUsePenalty, "double", params)
      params <- mluPasteArg(featureReusePenalty, "double", params)
      params <- mluPasteArg(gainConfidenceLevel, "double", params)
      params <- mluPasteArg(softmaxTemperature, "double", params)
      params <- mluPasteArg(executionTimes, "logical", params)
      params <- mluPasteArg(numLeaves, "integer", params)
      params <- mluPasteArg(minDocumentsInLeafs, "integer", params)
      params <- mluPasteArg(numTrees, "integer", params)
      params <- mluPasteArg(smoothing, 'double', params)
      params <- mluPasteArg(testFrequency, 'integer', params)
      params <- mluPasteArg(baggingSize, 'integer', params)
      dotargs <- list(...)
      for (arg in names(dotargs)) {
          params <- mluPasteArg(dotargs[[arg]], "", params, arg)
      }
      params <- sprintf("FastForestRegression{%s}", params)
      return(structure(params, class = c("FastForestRegression", 
          "RegressorTrainer", "maml", "character")))
    }
  
 
```
 
 
 
 
