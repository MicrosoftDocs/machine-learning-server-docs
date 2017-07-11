--- 
 
# required metadata 
title: "Machine Learning Categorical HashData Transform" 
description: "Categorical hash transform that can be performed on data before training a model." 
keywords: "transform, catagory, hash" 
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

## *categorical_hash*: Hashes and converts a text column into categories


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.categorical_hash(cols: [<class ‘str’>, <class ‘dict’>, <class ‘list’>], hash_bits: int = 16, seed: int = 314489979, ordered: bool = True, invert_hash: int = 0, output_kind: [‘Bag’, ‘Ind’, ‘Key’, ‘Bin’] = ‘Bag’, **kargs)
```




### Description

Categorical hash transform that can be performed on data before
training a model.


### Details

`categorical_hash` converts a categorical value into an indicator
array by hashing the value and using the hash as an index in the bag. If
the input column is a vector, a single indicator bag is returned for it.
`categorical_hash` does not currently support handling factor data.


### Arguments


##### cols

A character string or list of variable names to transform. If
`dict`, the keys represent the names of new variables to be created.


##### hash_bits

An integer specifying the number of bits to hash into.
Must be between 1 and 30, inclusive. The default value is 16.


##### seed

An integer specifying the hashing seed. The default value is
314489979.


##### ordered

`True` to include the position of each term in the
hash. Otherwise, `False`. The default value is `True`.


##### invert_hash

An integer specifying the limit on the number of keys
that can be used to generate the slot name. `0` means no invert
hashing; `-1` means no limit. While a zero value gives better
performance, a non-zero value is needed to get meaningful coefficent names.
The default value is `0`.


##### output_kind

A character string that specifies the kind
of output kind.

* `"Bag"`: Outputs a multi-set vector. If the input column is a vector of categories, the output contains one vector, where the value in each slot is the number of occurrences of the category in the input vector. If the input column contains a single category, the indicator vector and the bag vector are equivalent 

* `"Ind"`: Outputs an indicator vector. The input column is a vector of categories, and the output contains one indicator vector per slot in the input column. 

* `"Key`: Outputs an index. The output is an integer id (between 1 and the number of categories in the dictionary) of the category. 

* `"Bin`: Outputs a vector which is the binary representation of the category. 

The default value is `"Bag"`.


##### kargs

Additional arguments sent to the compute engine.


### Returns

an object defining the transform.


### See also

[`categorical`](categorical.md)


### Example



```
'''
Example on rx_logistic_regression and categorical_hash.
'''
import numpy
import pandas
from microsoftml import rx_logistic_regression, categorical_hash, rx_predict
from microsoftml.datasets.datasets import movie_reviews


train_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Do not like it", "Really like it",
        "I hate it", "I like it a lot", "I kind of hate it", "I do like it",
        "I really hate it", "It is very good", "I hate it a bunch", "I love it a bunch",
        "I hate it", "I like it very much", "I hate it very much.",
        "I really do love it", "I really do hate it", "Love it!", "Hate it!",
        "I love it", "I hate it", "I love it", "I hate it", "I love it"],
    like=[True, False, True, False, True, False, True, False, True, False,
        True, False, True, False, True, False, True, False, True, False, True,
        False, True, False, True]))
        
test_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Really like it", "I hate it",
        "I like it a lot", "I love it", "I do like it", "I really hate it", "I love it"]))


# Use a categorical hash transform.
out_model = rx_logistic_regression("like ~ reviewCat",
                data=train_reviews,
                ml_transforms=[categorical_hash(cols=dict(reviewCat="review"))])
                
# Weights are similar to categorical.
print(out_model.coef_)

# Use the model to score.
source_out_df = rx_predict(out_model, data=test_reviews, extra_vars_to_write=["review"])
print(source_out_df.head())
```


Output:



```
Not adding a normalizer.
Beginning processing data.
Rows Read: 25, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 25, Read Time: 0, Transform Time: 0
Beginning processing data.
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
LBFGS multi-threading will attempt to load dataset into memory. In case of out-of-memory issues, turn off multi-threading by setting trainThreads to 1.
Beginning optimization
num vars: 65537
improvement criterion: Mean Improvement
Warning: Premature convergence occurred. The OptimizationTolerance may be set too small. ro equals zero. Is your function linear?
L1 regularization selected 3 of 65537 weights.
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.1619594
Elapsed time: 00:00:00.0351158
OrderedDict([('(Bias)', 0.21322768926620483), ('f1783', -0.7938960194587708), ('f38537', 0.19675208628177643)])
Beginning processing data.
Rows Read: 10, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0773028
Finished writing 10 rows.
Writing completed.
           review PredictedLabel     Score  Probability
0   This is great           True  0.213228     0.553106
1       I hate it          False -0.580668     0.358779
2         Love it           True  0.213228     0.553106
3  Really like it           True  0.213228     0.553106
4       I hate it          False -0.580668     0.358779
```

