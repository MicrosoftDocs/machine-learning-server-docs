--- 
 
# required metadata 
title: "tlcTrain function (MicrosoftML) | Microsoft Docs" 
description: " Trains a predictor. " 
keywords: "(MicrosoftML), tlcTrain, Command" 
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
 
 
 
 
 #tlcTrain: Command: 'Train' 
 ##Description
 
Trains a predictor.
 
 
 ##Usage

```   
  tlcTrain(customColumn.with.tag = "", normalize = "Auto",
    trainer = "AveragedPerceptron", validationFile = "",
    calibrator = "sigmoid", randomSeed = NULL, parallel = NULL,
    transform.with.tag = list(), ..., commandContext = "")
 
```
 
 ##Arguments

   
  
 ### `customColumn.with.tag`
 character: <string>. Columns with custom kinds declared through key assignments, e.g., col[Kind]=Name to assign column named 'Name' kind 'Kind' (short form col) 
  
  
  
 ### `normalize`
 character: [No|Warn|Auto|Yes]. Normalize option for the feature column Default value:'Auto' (short form norm) 
  
  
  
 ### `trainer`
 list: <name><options>. Trainer to use Default value:'AveragedPerceptron' (short form tr) 
  
  
  
 ### `validationFile`
 character: <string>. The validation data file (short form valid) 
  
  
  
 ### `calibrator`
 list: <name><options>. Output calibrator Default value:'PlattCalibration' (short form cali) 
  
  
  
 ### `randomSeed`
 integer: <int>. Random seed (short form seed) 
  
  
  
 ### `parallel`
 integer: <int>. Desired degree of parallelism in the data pipeline (short form n) 
  
  
  
 ### `transform.with.tag`
 list: <name><options>. Transform (short form xf) 
  
  
  
 ### `commandContext`
 character: [chain|sweep|]. The compute context of the command. Default value:'' 
  
  
  
 ### ` ...`
 : . hidden arguments 
  
 
 
 ##Details
 

 
 
 ##Value
 
the output of the TLC Command: Train (Command).
 
 ##Note
 
args with tag (customColumn.with.tag, transform.with.tag) can be
specified as: structure('...', tag = '...').
 
 
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
  function (featureColumn = "Features", labelColumn = "Label", 
      weightColumn = "Weight", groupColumn = "GroupId", nameColumn = "Name", 
      customColumn.with.tag = "", normalizeFeatures = "Auto", trainer = "AveragedPerceptron", 
      validationFile = "", cacheData = FALSE, calibrator = "PlattCalibration", 
      maxCalibrationExamples = 1e+09, continueTrain = FALSE, loader = list(), 
      dataFile = "", outputModelFile = "", inputModelFile = "", 
      loadTransforms = FALSE, randomSeed = NULL, parallel = NULL, 
      transform.with.tag = list(), ..., commandContext = "") 
  {
      params <- character()
      params <- mluPasteArg(featureColumn, "character", params)
      params <- mluPasteArg(labelColumn, "character", params)
      params <- mluPasteArg(weightColumn, "character", params)
      params <- mluPasteArg(groupColumn, "character", params)
      params <- mluPasteArg(nameColumn, "character", params)
      params <- mluPasteArg(customColumn.with.tag, "character", params)
      params <- mluPasteArg(normalizeFeatures, "character", params)
      params <- mluPasteArg(trainer, "list", params)
      params <- mluPasteArg(validationFile, "character", params)
      params <- mluPasteArg(cacheData, "logical", params)
      params <- mluPasteArg(calibrator, "list", params)
      params <- mluPasteArg(maxCalibrationExamples, "integer", params)
      params <- mluPasteArg(continueTrain, "logical", params)
      params <- mluPasteArg(loader, "list", params)
      params <- mluPasteArg(dataFile, "character", params)
      params <- mluPasteArg(outputModelFile, "character", params)
      params <- mluPasteArg(inputModelFile, "character", params)
      params <- mluPasteArg(loadTransforms, "logical", params)
      params <- mluPasteArg(randomSeed, "integer", params)
      params <- mluPasteArg(parallel, "integer", params)
      params <- mluPasteArg(transform.with.tag, "list", params)
      dotargs <- list(...)
      for (arg in names(dotargs)) {
          params <- mluPasteArg(dotargs[[arg]], "", params, arg)
      }
      active.calls <- sapply(sys.calls(), function(acall) as.character(acall[[1]]))
      if (!nzchar(commandContext) && length(active.calls) > 1) {
          chainIds <- grep("chain", active.calls, ignore.case = TRUE)
          sweepIds <- grep("sweep", active.calls, ignore.case = TRUE)
          if (length(chainIds) > 0 && length(sweepIds) == 0) 
              commandContext <- "chain"
          else if (length(sweepIds) > 0 && length(chainIds) == 
              0) 
              commandContext <- "sweep"
          else if (length(sweepIds) > 0 && length(chainIds) > 0) 
              commandContext <- ifelse(max(chainIds) > max(sweepIds), 
                  "chain", "sweep")
      }
      params <- switch(commandContext, chain = sprintf("Train{%s}", 
          params), sweep = sprintf("{Train%s}", params), sprintf("Train%s", 
          params))
      if (commandContext == "") 
          mamlRun(params)
      else return(structure(params, class = c("Train", "Command", "maml", "character")))
    }
  
 
```
 
 
 
