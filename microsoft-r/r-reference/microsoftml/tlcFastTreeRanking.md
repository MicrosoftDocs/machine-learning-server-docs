--- 
 
# required metadata 
title: "tlcFastTreeRanking function (MicrosoftML) | Microsoft Docs" 
description: " Trains gradient boosted decision trees to the LambdaRank quasi-gradient. " 
keywords: "(MicrosoftML), tlcFastTreeRanking, FastRankRanking, FastRankRankingWrapper, btrank, frrank, ftrank, rank, RankerTrainer, Trainer" 
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
 
 
 
 
 
 
 
 
 
 
 #tlcFastTreeRanking: RankerTrainer, Trainer: 'FastTreeRanking' 
 ##Description
 
Trains gradient boosted decision trees to the LambdaRank quasi-gradient.
 
 
 ##Usage

```   
  tlcFastTreeRanking(customGains = "0,3,7,15,31", trainDCG = FALSE,
    featureFraction = 1, baggingSize = 0, baggingTrainFraction = 0.7,
    bestStepRankingRegressionTrees = FALSE, useLineSearch = FALSE,
    numPostBracketSteps = 0, minStepSize = 0,
    optimizationAlgorithm = "GradientDescent", earlyStoppingRule = list(),
    earlyStoppingMetrics = 0, enablePruning = FALSE,
    useTolerantPruning = FALSE, pruningThreshold = 0.004,
    pruningWindowSize = 5, testFrequency = 2147483647, learningRates = 0.2,
    shrinkage = 1, dropoutRate = 0, splitFraction = 1,
    getDerivativesSampleRate = 1, writeLastEnsemble = FALSE, smoothing = 0,
    maxTreeOutput = 100, numThreads = 8, rngSeed = 123,
    entropyCoefficient = 0, histogramPoolSize = -1, diskTranspose = FALSE,
    maxBins = 255, sparsifyThreshold = 0.7, featureFirstUsePenalty = 0,
    featureReusePenalty = 0, gainConfidenceLevel = 0,
    softmaxTemperature = 0, executionTimes = FALSE, numLeaves = 20,
    minDocumentsInLeafs = 10, numTrees = 100, ...)
 
```
 
 ##Arguments

   
  
 ### `customGains`
 character: <string>. Comma seperated list of gains associated to each relevance label. Default value:'0,3,7,15,31' (short form gains) 
  
  
  
 ### `trainDCG`
 logical: [+|-]. Train DCG instead of NDCG Default value:'-' (short form dcg) 
  
  
  
 ### `featureFraction`
 double: <float>. The fraction of features (chosen randomly) to use on each iteration Default value:'1' (short form ff) 
  
  
  
 ### `baggingSize`
 integer: <int>. Number of trees in each bag (0 for disabling bagging) Default value:'0' (short form bag) 
  
  
  
 ### `baggingTrainFraction`
 double: <float>. Percentage of training queries used in each bag Default value:'0.7' (short form bagfrac) 
  
  
  
 ### `bestStepRankingRegressionTrees`
 logical: [+|-]. Use best regression step trees? Default value:'-' (short form bsr) 
  
  
  
 ### `useLineSearch`
 logical: [+|-]. Should we use line search for a step size Default value:'-' (short form ls) 
  
  
  
 ### `numPostBracketSteps`
 integer: <int>. Number of post-bracket line search steps Default value:'0' (short form lssteps) 
  
  
  
 ### `minStepSize`
 double: <float>. Minimum line search step size Default value:'0' (short form minstep) 
  
  
  
 ### `optimizationAlgorithm`
 character: [GradientDescent|AcceleratedGradientDescent|ConjugateGradientDescent]. Optimization algorithm to be used (GradientDescent, AcceleratedGradientDescent) Default value:'GradientDescent' (short form oa) 
  
  
  
 ### `earlyStoppingRule`
 list: <name><options>. Early stopping rule. (Validation set (/valid) is required.) (short form esr) 
  
  
  
 ### `earlyStoppingMetrics`
 integer: <int>. Early stopping metrics. (For regression, 1: L1, 2:L2; for ranking, 1:NDCG@1, 3:NDCG@3) Default value:'0' (short form esmt) 
  
  
  
 ### `enablePruning`
 logical: [+|-]. Enable post-training pruning to avoid overfitting. (a validation set is required) Default value:'-' (short form pruning) 
  
  
  
 ### `useTolerantPruning`
 logical: [+|-]. Use window and tolerance for pruning Default value:'-' (short form prtol) 
  
  
  
 ### `pruningThreshold`
 double: <double>. The tolerance threshold for pruning Default value:'0.004' (short form prth) 
  
  
  
 ### `pruningWindowSize`
 integer: <int>. The moving window size for pruning Default value:'5' (short form prws) 
  
  
  
 ### `testFrequency`
 integer: <int>. Calculate NDCG values for train/valid/test every k rounds Default value:'2147483647' (short form tf) 
  
  
  
 ### `learningRates`
 double: <float>. The learning rate Default value:'0.2' (short form lr) 
  
  
  
 ### `shrinkage`
 double: <float>. Shrinkage Default value:'1' (short form shrk) 
  
  
  
 ### `dropoutRate`
 double: <float>. Dropout rate for tree regularization Default value:'0' (short form tdrop) 
  
  
  
 ### `splitFraction`
 double: <float>. The fraction of features (chosen randomly) to use on each split Default value:'1' (short form sf) 
  
  
  
 ### `getDerivativesSampleRate`
 integer: <int>. same each query 1 in k times in the GetDerivatives function Default value:'1' (short form sr) 
  
  
  
 ### `writeLastEnsemble`
 logical: [+|-]. Write the last ensemble instead of the one determined by early stopping Default value:'-' (short form hl) 
  
  
  
 ### `smoothing`
 double: <float>. Smoothing paramter for tree regularization Default value:'0' (short form s) 
  
  
  
 ### `maxTreeOutput`
 double: <float>. Upper bound on absolute value of single tree output Default value:'100' (short form mo) 
  
  
  
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
  
  
  
 ### ` ...`
 : . hidden arguments 
  
 
 
 ##Details
 

 
 
 ##Value
 
a character string defining: FastTreeRanking (RankerTrainer,
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
  function (customGains = "0,3,7,15,31", trainDCG = FALSE, featureFraction = 1, 
      baggingSize = 0, baggingTrainFraction = 0.7, bestStepRankingRegressionTrees = FALSE, 
      useLineSearch = FALSE, numPostBracketSteps = 0, minStepSize = 0, 
      optimizationAlgorithm = "GradientDescent", earlyStoppingRule = list(), 
      earlyStoppingMetrics = 0, enablePruning = FALSE, useTolerantPruning = FALSE, 
      pruningThreshold = 0.004, pruningWindowSize = 5, testFrequency = 2147483647, 
      learningRates = 0.2, shrinkage = 1, dropoutRate = 0, splitFraction = 1, 
      getDerivativesSampleRate = 1, writeLastEnsemble = FALSE, 
      smoothing = 0, maxTreeOutput = 100,
      numThreads = 8, rngSeed = 123, 
      entropyCoefficient = 0, histogramPoolSize = -1, 
      diskTranspose = FALSE, maxBins = 255, sparsifyThreshold = 0.7, 
      featureFirstUsePenalty = 0, featureReusePenalty = 0, gainConfidenceLevel = 0, 
      softmaxTemperature = 0, executionTimes = FALSE, 
      numLeaves = 20, minDocumentsInLeafs = 10, numTrees = 100, 
      ...) 
  {
      params <- character()
      params <- mluPasteArg(customGains, "character", params)
      params <- mluPasteArg(trainDCG, "logical", params)
      params <- mluPasteArg(featureFraction, "double", params)
      params <- mluPasteArg(baggingSize, "integer", params)
      params <- mluPasteArg(baggingTrainFraction, "double", params)
      params <- mluPasteArg(bestStepRankingRegressionTrees, "logical", params)
      params <- mluPasteArg(useLineSearch, "logical", params)
      params <- mluPasteArg(numPostBracketSteps, "integer", params)
      params <- mluPasteArg(minStepSize, "double", params)
      params <- mluPasteArg(optimizationAlgorithm, "character", params)
      params <- mluPasteArg(earlyStoppingRule, "list", params)
      params <- mluPasteArg(earlyStoppingMetrics, "integer", params)
      params <- mluPasteArg(enablePruning, "logical", params)
      params <- mluPasteArg(useTolerantPruning, "logical", params)
      params <- mluPasteArg(pruningThreshold, "double", params)
      params <- mluPasteArg(pruningWindowSize, "integer", params)
      params <- mluPasteArg(testFrequency, "integer", params)
      params <- mluPasteArg(learningRates, "double", params)
      params <- mluPasteArg(shrinkage, "double", params)
      params <- mluPasteArg(dropoutRate, "double", params)
      params <- mluPasteArg(splitFraction, "double", params)
      params <- mluPasteArg(getDerivativesSampleRate, "integer", params)
      params <- mluPasteArg(writeLastEnsemble, "logical", params)
      params <- mluPasteArg(smoothing, "double", params)
      params <- mluPasteArg(maxTreeOutput, "double", params)
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
      dotargs <- list(...)
      for (arg in names(dotargs)) {
          params <- mluPasteArg(dotargs[[arg]], "", params, arg)
      }
      params <- sprintf("FastTreeRanking{%s}", params)
      return(structure(params, class = c("FastTreeRanking", 
  		"RankerTrainer", "maml", "character")))
    }
  
 
```
 
 
 
 
