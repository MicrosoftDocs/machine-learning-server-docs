--- 
 
# required metadata 
title: "Conversion of an rxDForest, rxDTree, or rpart object to an randomForest Object" 
description: " Converts objects containing decision tree results to an randomForest object. " 
keywords: "RevoScaleR, as.randomForest, as.randomForest.rxDForest, as.randomForest.rxDTree, as.randomForest.rpart, category, models" 
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
 
 
 
 
 
 #`as.randomForest`: Conversion of an rxDForest, rxDTree, or rpart object to an randomForest Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Converts objects containing decision tree results to an randomForest object.
 
 
 ##Usage

```   
 ## S3 method for class `rxDForest':
as.randomForest  (x, ...)
 ## S3 method for class `rxDTree':
as.randomForest  (x, ...)
 ## S3 method for class `rpart':
as.randomForest  (x, use.weight = TRUE, ties.method = c("random", "first", "last"), ...)
 
```
 
 ##Arguments

   
    
 ### `x`
  object of class rxDForest, rxDTree, or rpart. 
  
    
 ### `use.weight`
  a logical value (default being `TRUE`) specifying if the majority splitting direction  at a node should be decided based on the sum of case weights or the number of observations when the split variable at the node is a factor or ordered factor  but a certain level is not present (or not defined for the factor). 
  
    
 ### `ties.method`
  a character string specifying how ties are handled when deciding the majority direction,  with the default being `"random"`. Refer to max.col for details. 
  
    
 ### ` ...`
 additional arguments to be passed directly to `as.randomForest.rpart`. 
  
 
 
 
 ##Details
 
These functions convert an existing object of class rxDForest, rxDTree, 
or rpart to an object of class `randomForest`, respectively.
The underlying structure of the output object will be a subset of 
that produced by an equivalent call to `randomForest`. 
In many cases, this method can be used to coerce an object for use with the **pmml** package.
**RevoScaleR** model objects that contain `transforms` or a `transformFunc` are not supported.
 
 
 
 ##Value
 
an object of class randomForest.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxDForest](../../scaler/packagehelp/rxdforest.md),
[rxDTree](../../scaler/packagehelp/rxdtree.md),
rpart,
randomForest,
[as.rpart](../../scaler/packagehelp/as.rpart.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
# If the pmml and the randomForest packages are installed 
library(pmml)
library(randomForest)

mydata <- infert
form <- case ~ age + parity + education + spontaneous + induced
ntree <- 2

fit.dforest <- rxDForest(form, data = mydata, nTree = ntree)    #, cp = 0.01, seed = 1
fit.dforest
fit.dforest.rforest <- as.randomForest(fit.dforest)
predict(fit.dforest.rforest, newdata = mydata)
pmml(fit.dforest.rforest)

fit.rforest <- randomForest(form, data = mydata, ntree = ntree)
fit.rforest
predict(fit.rforest, newdata = mydata)
pmml(fit.rforest)
 ## End(Not run) 
  
 
```
 
 
 
 
