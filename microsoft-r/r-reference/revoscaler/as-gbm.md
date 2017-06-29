--- 
 
# required metadata 
title: "Conversion of an rxBTrees, rxDTree, or rpart object to an gbm Object" 
description: " Converts objects containing decision tree results to an gbm object. " 
keywords: "RevoScaleR, as.gbm, as.gbm.rxBTrees, as.gbm.rxDTree, as.gbm.rpart, category, models" 
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
 
 
 
 
 
 #`as.gbm`: Conversion of an rxBTrees, rxDTree, or rpart object to an gbm Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Converts objects containing decision tree results to an gbm object.
 
 
 ##Usage

```   
 ## S3 method for class `rxBTrees':
as.gbm  (x, ...)
 ## S3 method for class `rxDTree':
as.gbm  (x, ...)
 ## S3 method for class `rpart':
as.gbm  (x, use.weight = TRUE, cat.start = 0L, shrinkage = 1.0, ...)
 
```
 
 ##Arguments

   
    
 ### `x`
  object of class rxBTrees, rxDTree, or rpart. 
  
    
 ### `use.weight`
  a logical value (default being `TRUE`) specifying if the majority splitting direction  at a node should be decided based on the sum of case weights or the number of observations when the split variable at the node is a factor or ordered factor  but a certain level is not present (or not defined for the factor). 
  
    
 ### `cat.start`
  an integer specifying the starting position to add the categorical splits from the current tree  in the list of all the categorical splits in the collection of trees. 
  
    
 ### `shrinkage`
  numeric scalar specifying the learning rate of the boosting procedure for the current tree. 
  
    
 ### ` ...`
 additional arguments to be passed directly to `as.gbm.rpart`. 
  
 
 
 
 ##Details
 
These functions convert an existing object of class rxBTrees, rxDTree, 
or rpart to an object of class `gbm`, respectively.
The underlying structure of the output object will be a subset of 
that produced by an equivalent call to `gbm`. 
In many cases, this method can be used to coerce an object for use with the **pmml** package.
**RevoScaleR** model objects that contain `transforms` or a `transformFunc` are not supported.
 
 
 
 ##Value
 
an object of class gbm.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxBTrees](rxbtrees.md),
[rxDTree](rxdtree.md),
rpart,
gbm,
[as.rpart](as-rpart.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
# If the pmml and the gbm packages are installed 
library(pmml)
library(gbm)

mydata <- infert
form <- case ~ age + parity + education + spontaneous + induced
ntree <- 2

fit.btrees <- rxBTrees(form, data = mydata, nTree = ntree, 
    lossFunction = "gaussian", learningRate = 0.1)
fit.btrees
fit.btrees.gbm <- as.gbm(fit.btrees)
predict(fit.btrees.gbm, newdata = mydata, n.trees = ntree)
pmml(fit.btrees.gbm)

fit.gbm <- gbm(form, data = mydata, n.trees = ntree,
    distribution = "gaussian", shrinkage = 0.1)
fit.gbm
predict(fit.gbm, newdata = mydata, n.trees = ntree)
pmml(fit.gbm)
 ## End(Not run) 
  
 
```
 
 
 
 
