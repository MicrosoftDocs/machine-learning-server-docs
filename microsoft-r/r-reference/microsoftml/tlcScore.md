--- 
 
# required metadata 
title: "tlcScore function (MicrosoftML) | Microsoft Docs" 
description: " Scores a data file. " 
keywords: "(MicrosoftML), tlcScore, Command" 
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
 
 
 
 
 #tlcScore: Command: 'Score' 
 ##Description
 
Scores a data file.
 
 
 ##Usage

```   
  tlcScore(featureColumn = "Features", groupColumn = "GroupId",
    customColumn.with.tag = "", scorer = list(), saver = list(),
    outputDataFile = "", keepHidden = FALSE,
    postTransform.with.tag = list(), outputAllColumns = FALSE,
    outputColumn = "", loader = list(), dataFile = "",
    outputModelFile = "", inputModelFile = "", loadTransforms = FALSE,
    randomSeed = NULL, parallel = NULL, transform.with.tag = list(), ...,
    commandContext = "")
 
```
 
 ##Arguments

   
  
 ### `featureColumn`
 character: <string>. Column to use for features when scorer is not defined Default value:'Features' (short form feat) 
  
  
  
 ### `groupColumn`
 character: <string>. Group column name Default value:'GroupId' (short form group) 
  
  
  
 ### `customColumn.with.tag`
 character: <string>. Input columns: Columns with custom kinds declared through key assignments, e.g., col[Kind]=Name to assign column named 'Name' kind 'Kind' (short form col) 
  
  
  
 ### `scorer`
 list: <name><options>. Scorer to use 
  
  
  
 ### `saver`
 list: <name><options>. The data saver to use 
  
  
  
 ### `outputDataFile`
 character: <string>. File to save the data (short form dout) 
  
  
  
 ### `keepHidden`
 logical: [+|-]. Whether to include hidden columns Default value:'-' (short form keep) 
  
  
  
 ### `postTransform.with.tag`
 list: <name><options>. Post processing transform (short form pxf) 
  
  
  
 ### `outputAllColumns`
 logical: [+|-]. Whether to output all columns or just scores (short form all) 
  
  
  
 ### `outputColumn`
 character: <string>. What columns to output beyond score columns, if outputAllColumns=-. (short form outCol) 
  
  
  
 ### `loader`
 list: <name><options>. The data loader 
  
  
  
 ### `dataFile`
 character: <string>. The data file (short form data) 
  
  
  
 ### `outputModelFile`
 character: <string>. Model file to save (short form out) 
  
  
  
 ### `inputModelFile`
 character: <string>. Model file to load (short form in) 
  
  
  
 ### `loadTransforms`
 logical: [+|-]. Load transforms from model file? (short form loadTrans) 
  
  
  
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
 
the output of the TLC Command: Score (Command).
 
 ##Note
 
args with tag (customColumn.with.tag, postTransform.with.tag,
transform.with.tag) can be specified as: structure('...', tag = '...').
 
 
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
  function (featureColumn = "Features", groupColumn = "GroupId", 
      customColumn.with.tag = "", scorer = list(), saver = list(), 
      outputDataFile = "", keepHidden = FALSE, postTransform.with.tag = list(), 
      outputAllColumns = FALSE, outputColumn = "", loader = list(), 
      dataFile = "", outputModelFile = "", inputModelFile = "", 
      loadTransforms = FALSE, randomSeed = NULL, parallel = NULL, 
      transform.with.tag = list(), ..., commandContext = "") 
  {
      params <- character()
      params <- mluPasteArg(featureColumn, "character", params)
      params <- mluPasteArg(groupColumn, "character", params)
      params <- mluPasteArg(customColumn.with.tag, "character", params)
      params <- mluPasteArg(scorer, "list", params)
      params <- mluPasteArg(saver, "list", params)
      params <- mluPasteArg(outputDataFile, "character", params)
      params <- mluPasteArg(keepHidden, "logical", params)
      params <- mluPasteArg(postTransform.with.tag, "list", params)
      params <- mluPasteArg(outputAllColumns, "logical", params)
      params <- mluPasteArg(outputColumn, "character", params)
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
      params <- switch(commandContext, chain = sprintf("Score{%s}", 
          params), sweep = sprintf("{Score%s}", params), sprintf("Score%s", 
          params))
      if (commandContext == "") 
          mamlRun(params)
      else return(structure(params, class = c("Score", "Command", "maml", "character")))
    }
  
 
```
 
 
 
