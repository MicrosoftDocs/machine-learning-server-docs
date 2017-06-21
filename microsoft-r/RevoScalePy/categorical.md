--- 
 
# required metadata 
title: "Machine Learning Categorical Data Transform" 
description: "Categorical transform that can be performed on data before" 
keywords: "transform" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/21/2017" 
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

# categorical

microsoftml.modules.categorical.categorical(cols: [<class ‘str’>, <class ‘dict’>, <class ‘list’>], output_kind: [‘Bag’, ‘Ind’, ‘Key’, ‘Bin’] = ‘Ind’, max_num_terms=1000000, terms=None, sort: [‘Occurrence’, ‘Value’] = ‘Occurrence’, text_key_values=False, **kargs)



## Description

Categorical transform that can be performed on data before
training a model.


## Details

The ``categorical`` transform passes through a data set, operating
on text columns, to build a dictionary of categories. For each row,
the entire text string appearing in the input column is defined as a
category. The output of the categorical transform is an indicator vector.
Each slot in this vector corresponds to a category in the dictionary, so
its length is the size of the built dictionary. The categorical transform
can be applied to one or more columns, in which case it builds a separate
dictionary for each column that it is applied to.

``categorical`` is not currently supported to handle factor data.


## Parameters


### vars

A character vector or list of variable names to transform. If
named, the names represent the names of new variables to be created.


### output_kind

A character string that specifies the kind of output kind.

* ``"ind"``: Outputs an indicator vector. The input column is a vector of categories, and the output contains one indicator vector per slot in the input column. 

* ``"bag"``: Outputs a multi-set vector. If the input column is a vector of categories, the output contains one vector, where the value in each slot is the number of occurrences of the category in the input vector. If the input column contains a single category, the indicator vector and the bag vector are equivalent 

* ``"key"``: Outputs an index. The output is an integer id (between 1 and the number of categories in the dictionary) of the category. 

The default value is ``"ind"``.


### max_num_terms

An integer that specifies the maximum number of
categories to include in the dictionary. The default value is 1000000.


### terms

Optional character vector of terms or categories.


### kargs

Additional arguments sent to compute engine.


## Returns

A ``maml`` object defining the transform.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also

[``rx_fast_trees``](rx_fast_trees#microsoftml.modules.fast_trees.rx_fast_trees),
[``rx_fast_forest``](rx_fast_forest#microsoftml.modules.fast_forest.rx_fast_forest),
[``rx_neural_net``](rx_neural_network#microsoftml.modules.neural_network.rx_neural_network),
[``rx_oneclass_svm``](rx_oneclass_svm#microsoftml.modules.oneclass_svm.rx_oneclass_svm),
[``rx_logistic_regression``](rx_logistic_regression#microsoftml.modules.logistic_regression.rx_logistic_regression).


## Example



```
"""
=============
Dummy Example
=============

This example demonstrates an example included in a gallery.
"""


######################
# import dependencies

import os
import sys


#####################
# small test
print(sys.executable)
```



## Example



```
trainReviews <- data.frame(review = c( 
        "This is great",
        "I hate it",
        "Love it",
        "Do not like it",
        "Really like it",
        "I hate it",
        "I like it a lot",
        "I kind of hate it",
        "I do like it",
        "I really hate it",
        "It is very good",
        "I hate it a bunch",
        "I love it a bunch",
        "I hate it",
        "I like it very much",
        "I hate it very much.",
        "I really do love it",
        "I really do hate it",
        "Love it!",
        "Hate it!",
        "I love it",
        "I hate it",
        "I love it",
        "I hate it",
        "I love it"),
     like = c(TRUE, FALSE, TRUE, FALSE, TRUE,
        FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, FALSE, TRUE,
        FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, 
        FALSE, TRUE, FALSE, TRUE), stringsAsFactors = FALSE
    )

    testReviews <- data.frame(review = c(
        "This is great",
        "I hate it",
        "Love it",
        "Really like it",
        "I hate it",
        "I like it a lot",
        "I love it",
        "I do like it",
        "I really hate it",
        "I love it"), stringsAsFactors = FALSE)
```



## Example



```
# Use a categorical transform: the entire string is treated as a category
outModel1 <- rxLogisticRegression(like~reviewCat, data = trainReviews, 
    mlTransforms = list(categorical(vars = c(reviewCat = "review"))))
# Note that 'I hate it' and 'I love it' (the only strings appearing more than once)
# have non-zero weights
summary(outModel1)

# Use the model to score
scoreOutDF1 <- rxPredict(outModel1, data = testReviews, 
    extraVarsToWrite = "review")
scoreOutDF1
```

