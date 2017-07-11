--- 
 
# required metadata 
title: "Machine Learning Concat Transform" 
description: "Combines several columns into a single vector-valued column." 
keywords: "transform, schema" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/11/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

## *concat*: Concatenates multiple columns into a single vector


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.concat(cols: [<class ‘dict’>, <class ‘list’>], **kargs)
```




### Description

Combines several columns into a single vector-valued column.


### Details

`concat` creates a single vector-valued column from multiple
columns. It can be performed on data before training a model. The concatenation
can significantly speed up the processing of data when the number of columns
is as large as hundreds to thousands.


### Arguments


##### cols

A character dict or list of variable names to transform. If
`dict`, the keys represent the names of new variables to be created.
Note that all the input variables must
be of the same type. It is possible to produce mulitple output columns
with the concatenation transform. In this case, you need to use a list of
vectors to define a one-to-one mappings between input and output variables.
For example, to concatenate columns InNameA and InNameB into column OutName1
and also columns InNameC and InNameD into column OutName2, use the dict:
dict(OutName1 = [InNameA, InNameB], outName2 = [InNameC, InNameD])


##### kargs

Additional arguments sent to the compute engine.


### Returns

An object defining the concatenation transform.


### See also

[`drop_columns`](drop_columns.md),
[`select_columns`](select_columns.md).


### Example



```
'''
Example on logistic regression and concat.
'''
import numpy
import pandas
from microsoftml import rx_logistic_regression, concat, rx_predict
from microsoftml.datasets.datasets import iris
import sklearn

if sklearn.__version__ < "0.18":
    from sklearn.cross_validation import train_test_split
else:
    from sklearn.model_selection import train_test_split

# We use iris dataset.
irisdf = iris.as_df()

# The training features.
features = ["Sepal_Length", "Sepal_Width", "Petal_Length", "Petal_Width"]

# The label.
label = "Label"

# microsoftml needs a single dataframe with features and label.
cols = features + [label]

# We split into train/test. y_train, y_test are not used.
data_train, data_test, y_train, y_test = train_test_split(irisdf[cols], irisdf[label])

# We train a logistic regression.
# A concat transform is added to group features in a single vector column.
multi_logit_out = rx_logistic_regression(
                        formula="Label ~ Features",
                        method="multiClass",
                        data=data_train,
                        ml_transforms=[concat(cols={'Features': features})])
                        
# We show the coefficients.
print(multi_logit_out.coef_)

# We predict.
prediction = rx_predict(multi_logit_out, data=data_test)

print(prediction.head())
```


Output:



```
Automatically adding a MinMax normalization transform, use 'norm=Warn' or 'norm=No' to turn this behavior off.
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
LBFGS multi-threading will attempt to load dataset into memory. In case of out-of-memory issues, turn off multi-threading by setting trainThreads to 1.
Beginning optimization
num vars: 15
improvement criterion: Mean Improvement
L1 regularization selected 11 of 15 weights.
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2855185
Elapsed time: 00:00:00.0383873
OrderedDict([('0+(Bias)', 2.2455461025238037), ('1+(Bias)', 0.25067681074142456), ('2+(Bias)', -2.4962286949157715), ('0+Petal_Width', -2.8705310821533203), ('0+Petal_Length', -2.5397212505340576), ('0+Sepal_Width', 0.15964530408382416), ('0+Sepal_Length', -0.0009276362834498286), ('1+Sepal_Width', -0.22585509717464447), ('1+Petal_Length', 0.09019631147384644), ('2+Petal_Width', 2.6425082683563232), ('2+Petal_Length', 1.4481079578399658)])
Beginning processing data.
Rows Read: 38, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.1074682
Finished writing 38 rows.
Writing completed.
    Score.0   Score.1   Score.2
0  0.367040  0.452903  0.180057
1  0.038351  0.353085  0.608563
2  0.035455  0.321195  0.643350
3  0.287874  0.477909  0.234217
4  0.342016  0.467583  0.190401
```

