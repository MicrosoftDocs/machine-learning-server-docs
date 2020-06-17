--- 

# required metadata 
title: "rxPredict.rxNaiveBayes function (revoAnalytics) | Microsoft Docs" 
description: "     Calculate predicted or fitted values for a data set from an rxNaiveBayes object. " 
keywords: "(revoAnalytics), rxPredict.rxNaiveBayes, models, tree, classif, classification" 
author: "dphansen" 
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


 # rxPredict.rxNaiveBayes: Prediction for Large Data Naive Bayes Classifiers 
 ## Description

Calculate predicted or fitted values for a data set from an rxNaiveBayes object.


 ## Usage

```   
 ## S3 method for class `rxNaiveBayes':
rxPredict  (modelObject, data = NULL, outData = NULL, type = c("class", "prob"), prior = NULL,
      predVarNames = NULL, writeModelVars = FALSE, extraVarsToWrite = NULL, checkFactorLevels = TRUE,
        ...  )

```

 ## Arguments



 ### `modelObject`
  object returned from a call to [rxNaiveBayes](rxNaiveBayes.md). 


 ### `data`
  either a data source object, a character string  specifying a .xdf file, or a data frame object. 


 ### `outData`
  file or existing data frame to store predictions;  can be same as the input file or `NULL`.  If not `NULL`, must be an .xdf file if `data` is an .xdf file  or a data frame if `data` is a data frame. 


 ### `type`
  character string specifying the type of predicted values to be returned. Supported choices are  
* `"class"` -  a vector of predicted classes.   
* `"prob"` -  a matrix of predicted class probabilities whose columns are the probability of the first, second, etc. class.   



 ### `prior`
  a vector of prior probabilities.  If unspecified, the class proportions of the data counts in the training set are used. If present, they should be specified in the order of the factor levels of the response and they must be all non-negative and sum to 1.  



 ### `predVarNames`
  character vector specifying name(s) to give to the prediction results. 


 ### `writeModelVars`
  logical value. If `TRUE`, and the output file is different from the input file,  variables in the model will be written to the output file in addition to the predictions.  If variables from the input data set are transformed in the model,  the transformed variables will also be written out. 


 ### `extraVarsToWrite`
  `NULL` or character vector of additional variables names from the input data to include in the `outData`.   If `writeModelVars` is `TRUE`,  model variables will be included as well. 


 ### `checkFactorLevels`
  logical value. If `TRUE`, the factor levels for the data  will be verified against factor levels in the model.  Setting to `FALSE` can speed up computations if using lots of factors. 



 ### ` ...`
  additional arguments to be passed directly to [rxDataStep](rxDataStep.md) such as `removeMissingsOnRead`, `overwrite`,  `blocksPerRead`, `reportProgress`, `xdfCompressionLevel`. 



 ## Details

Prediction for large data models requires both a fitted model object and a data set, either the
original data (to obtain fitted values and residuals) or a new data set containing the same set
of variables as the original fitted model. Notice that this is different from the behavior of
`predict`, which can usually work on the original data simply by referencing the fitted model.


 ## Value

Depending on the form of `data`, this function variously returns a data frame or a data source
representing a .xdf file.

 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## References

Naive Bayes classifier
[`https://en.wikipedia.org/wiki/Naive_Bayes_classifier`](https://en.wikipedia.org/wiki/Naive_Bayes_classifier)
.


 ## See Also

[rxNaiveBayes](rxNaiveBayes.md).

 ## Examples

 ```

  # multi-class classification with a data.frame
  iris.nb <- rxNaiveBayes(Species ~ ., data = iris)
  iris.nb

  # prediction
  iris.nb.pred <- rxPredict(iris.nb, iris)
  iris.nb.pred
  table(iris.nb.pred[["Species_Pred"]], iris[["Species"]])
```





