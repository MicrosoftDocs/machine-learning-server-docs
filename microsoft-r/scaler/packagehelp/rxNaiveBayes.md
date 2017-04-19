--- 
 
# required metadata 
title: "Parallel External Memory Algorithm for Naive Bayes Classifiers" 
description: "     Fit Naive Bayes Classifiers on an .xdf file or data frame     for small or large data using parallel external memory algorithm. " 
keywords: "RevoScaleR, rxNaiveBayes, print.rxNaiveBayes, models, tree, classif, classification" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 #`rxNaiveBayes`: Parallel External Memory Algorithm for Naive Bayes Classifiers

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Fit Naive Bayes Classifiers on an .xdf file or data frame
for small or large data using parallel external memory algorithm.
 
 
 ##Usage

```   
  rxNaiveBayes(formula, data, smoothingFactor = 0,   ...  )
 
```
 
 ##Arguments

   
    
 ### `formula`
  formula as described in [rxFormula](rxFormula.md).     
  
    
 ### `data`
  either a data source object, a character string  specifying a .xdf file, or a data frame object. 
  
    
 ### `smoothingFactor`
  a positive smoothing factor to account for cases not present in the training data.  It avoids modeling issues by preventing zero conditional probability estimates. 
  
    
 ### ` ...`
  additional arguments to be passed directly to [rxSummary](rxSummary.md) such as `byTerm`, `rowSelection`, `pweights`, `fweights`, `transforms`, `transformObjects`, `transformFunc`,  `transformVars`, `transformPackages`, `transformEnvir`,  `useSparseCube`, `removeZeroCounts`, `blocksPerRead`,  `reportProgress`, `verbose`, `xdfCompressionLevel`.   
  
 
 
 ##Value
 
an `"rxNaiveBayes"` object containing the following components:


* `apriori` -  a vector of prior class probabilities for the dependent variable.


* `tables` -  a list of tables, one for each predictor variable.   
   * For a categorical variable, the table contains the conditional probabilities of the variable given the target class.  
   * For a numeric variable, the table contains the mean and standard deviation of the variable given the target class.  
 


* `levels` -  the levels of the dependent variable.


* `call` -  the matched call.



 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##References
 
Naive Bayes classifier
[`http://en.wikipedia.org/wiki/Naive_Bayes_classifier`](http://en.wikipedia.org/wiki/Naive_Bayes_classifier)
.
 
 
 ##See Also
 
[rxPredict.rxNaiveBayes](rxPredict.rxNaiveBayes.md).
   
 ##Examples

 ```
   
  # multi-class classification with an .xdf file
  claimsXdf <- file.path(rxGetOption("sampleDataDir"),"claims.xdf")
  claims.nb <- rxNaiveBayes(type ~ age + cost, data = claimsXdf)
  claims.nb
  
  # prediction
  claims.nb.pred <- rxPredict(claims.nb, claimsXdf)
  claims.nb.pred
  table(claims.nb.pred[["type_Pred"]], rxDataStep(claimsXdf)[["type"]])
 
```
 
 
 
 
 
