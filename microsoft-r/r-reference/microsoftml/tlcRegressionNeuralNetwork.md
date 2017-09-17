--- 
 
# required metadata 
title: "tlcRegressionNeuralNetwork function (MicrosoftML) | Microsoft Docs" 
description: " Regression neural network is a regression algorithm which uses neural network with single output. TLC supports various types of neural networks, including deep neural networks (DNNs) and convolutional neural networks (CNN) via Net# language. " 
keywords: "(MicrosoftML), tlcRegressionNeuralNetwork, RegressionNet, rnn, RegressorTrainer, Trainer" 
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
 
 
 
 
 
 
 #tlcRegressionNeuralNetwork: RegressorTrainer, Trainer: 'RegressionNeuralNetwork' 
 ##Description
 
Regression neural network is a regression algorithm which uses neural
network with single output. TLC supports various types of neural networks,
including deep neural networks (DNNs) and convolutional neural networks
(CNN) via Net# language.
 
 
 ##Usage

```   
  tlcRegressionNeuralNetwork(lossFunction = "SquaredError",
    defaultHiddenNodes = 100, netFileName = "", numIterations = 100,
    displayRefresh = 1, optimizationAlgorithm = "sgd",
    initWtsDiameter = 0.1, maxNorm = 0, earlyStoppingRule = list(),
    earlyStoppingMetrics = 0, pruning = FALSE, pruningFactor = 0.01,
    pruningRounds = 10, pruningRoundIterations = 5, acceleration = "avx",
    preTrainerType = "NoPreTrainer", preTrainingEpoch = NULL,
    miniBatchSize = 1, shuffle = TRUE, inputDropoutRate = 0,
    hiddenDropoutRate = 0, netDefinition = "")
 
```
 
 ##Arguments

   
  
 ### `lossFunction`
 list: <name><options>. Loss function Default value:'SquaredError' (short form loss) 
  
  
  
 ### `defaultHiddenNodes`
 integer: <int>. Default number of hidden nodes Default value:'100' (short form hidden) 
  
  
  
 ### `netFileName`
 character: <string>. Net file name (short form filename) 
  
  
  
 ### `numIterations`
 integer: <int>. Number of training iterations Default value:'100' (short form iter) 
  
  
  
 ### `displayRefresh`
 integer: <int>. Display refresh frequency in number iterations Default value:'1' (short form refresh) 
  
  
  
 ### `optimizationAlgorithm`
 list: <name><options>. Optimization algorithm (Adadelta or SGD) Default value:'sgd' (short form algo) 
  
  
  
 ### `initWtsDiameter`
 double: <float>. Init weights diameter Default value:'0.1' (short form initwts) 
  
  
  
 ### `maxNorm`
 double: <float>. Constrains the norm of incoming weights of a node Default value:'0' 
  
  
  
 ### `earlyStoppingRule`
 list: <name><options>. Early stopping rule (short form esr) 
  
  
  
 ### `earlyStoppingMetrics`
 integer: <int>. Early stopping metrics Default value:'0' (short form esmt) 
  
  
  
 ### `pruning`
 logical: [+|-]. Enable post-training pruning (Optimal Brain Damage) Default value:'-' (short form prune) 
  
  
  
 ### `pruningFactor`
 double: <float>. Pruning factor: % of weights removed each pruning iteration Default value:'0.01' (short form prunefact) 
  
  
  
 ### `pruningRounds`
 integer: <int>. Number of pruning rounds Default value:'10' (short form pruneround) 
  
  
  
 ### `pruningRoundIterations`
 integer: <int>. Number of pruning round iterations Default value:'5' (short form pruneiter) 
  
  
  
 ### `acceleration`
 list: <name><options>. Hardware acceleration level Default value:'avx' (short form accel) 
  
  
  
 ### `preTrainerType`
 character: [NoPreTrainer|Greedy]. Net Pre-Trainer Default value:'NoPreTrainer' (short form pretrain) 
  
  
  
 ### `preTrainingEpoch`
 integer: <int>. Number of epochs for pre-training. If not set, defaults to numIterations(iter). (short form prepoch) 
  
  
  
 ### `miniBatchSize`
 integer: <int>. Mini-batch size Default value:'1' (short form mbsize) 
  
  
  
 ### `shuffle`
 logical: [+|-]. Whether to shuffle for each training iteration Default value:'+' (short form shuf) 
  
  
  
 ### `inputDropoutRate`
 double: <float>. Input dropout rate Default value:'0' (short form idrop) 
  
  
  
 ### `hiddenDropoutRate`
 double: <float>. Hidden dropout rate Default value:'0' (short form hdrop) 
  
  
  
 ### `netDefinition`
 character: <string>. Neural network definition (short form net) 
  
  
  
 ### ` ...`
 : . hidden arguments 
  
 
 
 ##Details
 

 
 
 ##Value
 
a character string defining: RegressionNeuralNetwork
(RegressorTrainer, Trainer).
 
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
  function (lossFunction = "SquaredError", defaultHiddenNodes = 100, 
      netFileName = "", numIterations = 100, displayRefresh = 1, 
      optimizationAlgorithm = "sgd", initWtsDiameter = 0.1, maxNorm = 0, 
      earlyStoppingRule = list(), earlyStoppingMetrics = 0, pruning = FALSE, 
      pruningFactor = 0.01, pruningRounds = 10, pruningRoundIterations = 5, 
      acceleration = "avx", preTrainerType = "NoPreTrainer", preTrainingEpoch = NULL, 
      miniBatchSize = 1, shuffle = TRUE, inputDropoutRate = 0, 
      hiddenDropoutRate = 0, netDefinition = "", ...) 
  {
      params <- character()
      params <- mluPasteArg(lossFunction, "list", params)
      params <- mluPasteArg(defaultHiddenNodes, "integer", params)
      params <- mluPasteArg(netFileName, "character", params)
      params <- mluPasteArg(numIterations, "integer", params)
      params <- mluPasteArg(displayRefresh, "integer", params)
      params <- mluPasteArg(optimizationAlgorithm, "list", params)
      params <- mluPasteArg(initWtsDiameter, "double", params)
      params <- mluPasteArg(maxNorm, "double", params)
      params <- mluPasteArg(earlyStoppingRule, "list", params)
      params <- mluPasteArg(earlyStoppingMetrics, "integer", params)
      params <- mluPasteArg(pruning, "logical", params)
      params <- mluPasteArg(pruningFactor, "double", params)
      params <- mluPasteArg(pruningRounds, "integer", params)
      params <- mluPasteArg(pruningRoundIterations, "integer", params)
      params <- mluPasteArg(acceleration, "list", params)
      params <- mluPasteArg(preTrainerType, "character", params)
      params <- mluPasteArg(preTrainingEpoch, "integer", params)
      params <- mluPasteArg(miniBatchSize, "integer", params)
      params <- mluPasteArg(shuffle, "logical", params)
      params <- mluPasteArg(inputDropoutRate, "double", params)
      params <- mluPasteArg(hiddenDropoutRate, "double", params)
      params <- mluPasteArg(netDefinition, "character", params)
      dotargs <- list(...)
      for (arg in names(dotargs)) {
          params <- mluPasteArg(dotargs[[arg]], "", params, arg)
      }
      params <- sprintf("RegressionNeuralNetwork{%s}", params)
      return(structure(params, class = c("RegressionNeuralNetwork", 
          "RegressorTrainer", "maml", "character")))
    }
  
 
```
 
 
 
 
