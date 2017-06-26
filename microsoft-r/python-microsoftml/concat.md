--- 
 
# required metadata 
title: "Machine Learning Concat Transform" 
description: "Combines several columns into a single vector-valued column." 
keywords: "transform" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/26/2017" 
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

# concat



```
microsoftml.modules.schema_manipulation.concat(cols: (<class ‘dict’>, <class ‘list’>), **kargs)
```




## Description

Combines several columns into a single vector-valued column.


## Details

``concat`` creates a single vector-valued column from multiple
columns. It can be performed on data before training a model. The concatenation
can significantly speed up the processing of data when the number of columns
is as large as hundreds to thousands.


## Parameters


### cols

A named list of character vectors of input variable names and
the name of the output variable. Note that all the input variables must
be of the same type. It is possible to produce mulitple output columns
with the concatenation transform. In this case, you need to use a list of
vectors to define a one-to-one mappings between input and output variables.
For example, to concatenate columns InNameA and InNameB into column OutName1
and also columns InNameC and InNameD into column OutName2, use the list:
(list(OutName1 = c(InNameA, InNameB), outName2 = c(InNameC, InNameD)))


### kargs

Additional arguments sent to the compute engine


## Returns

A ``maml`` object defining the concatenation transform.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


## See also

[``rx_fast_trees``](rx_fast_trees#microsoftml.modules.fast_trees.rx_fast_trees.md),
[``rx_fast_forest``](rx_fast_forest#microsoftml.modules.fast_forest.rx_fast_forest.md),
``rx_fast_linear``,
[``rx_logistic_regression``](rx_logistic_regression#microsoftml.modules.logistic_regression.rx_logistic_regression.md),
``rx_one_class_svm``,
``featurize_text``,
[``categorical``](categorical#microsoftml.modules.categorical.categorical.md),
[``categorical_hash``](categorical_hash#microsoftml.modules.categorical.categorical_hash.md),
[``rx_predict.ml_model``](rx_predict#microsoftml.modules.predict.rx_predict.md)


## Example



```
testObs <- rnorm(nrow(iris)) > 0
testIris <- iris[testObs,]
trainIris <- iris[!testObs,]

multiLogitOut <- rxLogisticRegression(
        formula = Species~Features, type = "multiClass", data = trainIris,
        mlTransforms = list(concat(vars = list(
            Features = c("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width")
          ))))
summary(multiLogitOut)
```

