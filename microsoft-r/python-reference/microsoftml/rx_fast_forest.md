--- 
 
# required metadata 
title: "Fast Forest" 
description: "Machine Learning Fast Forest" 
keywords: "models, classification, regression" 
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

## *rx_fast_forest*: Random Forest


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.rx_fast_forest(formula: str, data: [<class ‘revoscalepy.datasource.RxDataSource.RxDataSource’>, <class ‘pandas.core.frame.DataFrame’>], method: [‘binary’, ‘regression’] = ‘binary’, num_trees: int = 100, num_leaves: int = 20, min_split: int = 10, example_fraction: float = 0.7, feature_fraction: float = 1, split_fraction: float = 1, num_bins: int = 255, first_use_penalty: float = 0, gain_conf_level: float = 0, train_threads: int = 8, random_seed: int = None, ml_transforms: list = None, ml_transform_vars: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment: dict = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, ensemble: dict = None, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
```




### Description

Machine Learning Fast Forest


### Details

Decision trees are non-parametric models that perform a sequence
of simple tests on inputs. This decision procedure maps them to outputs
found in the training dataset whose inputs were similar to the instance
being processed. A decision is made at each node of the binary tree data
structure based on a measure of similarity that maps each instance
recursively through the branches of the tree until the appropriate leaf
node is reached and the output decision returned.

Decision trees have several advantages:

* They are efficient in both computation and memory usage during training and prediction. 

* They can represent non-linear decision boundaries. 

* They perform integrated feature selection and classification. 

* They are resilient in the presence of noisy features. 

Fast forest regression is a random forest and quantile regression forest
implementation using the regression tree learner in
[`rx_fast_trees`](rx_fast_trees.md).
The model consists of an ensemble of decision trees. Each tree in a decision
forest outputs a Gaussian distribution by way of prediction. An aggregation
is performed over the ensemble of trees to find a Gaussian distribution
closest to the combined distribution for all trees in the model.

This decision forest classifier consists of an ensemble of decision trees.
Generally, ensemble models provide better coverage and accuracy than single
decision trees. Each tree in a decision forest outputs a Gaussian distribution.


### Arguments


##### formula

The formula as described in [revoscalepy.rx_formula](https://docs.microsoft.com/en-us/r-server/r-reference/revoscalpy/rx_formula).
Interaction terms and `F()` are not currently supported in the
[microsoftml](https://docs.microsoft.com/en-us/r-server/r/concept-what-is-the-microsoftml-package).


##### data

A data source object or a character string specifying a
*.xdf* file or a data frame object.


##### method

A character string denoting Fast Tree type:

* `"binary"` for the default Fast Tree Binary Classification or 

* `"regression"` for Fast Tree Regression. 


##### num_trees

Specifies the total number of decision trees to create in
the ensemble.By creating more decision trees, you can potentially get
better coverage, but the training time increases. The default value is 100.


##### num_leaves

The maximum number of leaves (terminal nodes) that can be created
in any tree. Higher values potentially increase the size of the tree and get
better precision, but risk overfitting and requiring longer training times.
The default value is 20.


##### min_split

Minimum number of training instances required to form a
leaf. That is, the minimal number of documents allowed in a leaf of a
regression tree, out of the sub-sampled data. A ‘split’ means that features
in each level of the tree (node) are randomly divided. The default value is 10.


##### example_fraction

The fraction of randomly chosen instances to use
for each tree. The default value is 0.7.


##### feature_fraction

The fraction of randomly chosen features to use for
each tree. The default value is 0.7.


##### split_fraction

The fraction of randomly chosen features to use on
each split. The default value is 0.7.


##### num_bins

Maximum number of distinct values (bins) per feature.
The default value is 255.


##### first_use_penalty

The feature first use penalty coefficient. The default
value is 0.


##### gain_conf_level

Tree fitting gain confidence requirement (should be in
the range [0,1) ). The default value is 0.


##### train_threads

The number of threads to use in training. If *None*
is specified, the number of threads to use is determined internally.
The default value is *None*.


##### random_seed

Specifies the random seed. The default value is *None*.


##### ml_transforms

Specifies a list of MicrosoftML transforms to be
performed on the data before training or *None* if no transforms are
to be performed. See [`featurize_text`](featurize_text.md),
[`categorical`](categorical.md),
and [`categorical_hash`](categorical_hash.md),
for transformations that are supported.
These transformations are performed after any specified Python transformations.
The default value is *None*.


##### ml_transform_vars

Specifies a character vector of variable names
to be used in `ml_transforms` or *None* if none are to be used.
The default value is *None*.


##### row_selection

NOT SUPPORTED. Specifies the rows (observations) from the data set that
are to be used by the model with the name of a logical variable from the
data set (in quotes) or with a logical expression using variables in the
data set. For example:

* `row_selection = "old"` will only use observations in which the value of the variable `old` is `True`. 

* `row_selection = (age > 20) & (age < 65) & (log(income) > 10)` only uses observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10. 

The row selection is performed after processing any data
transformations (see the arguments `transforms` or
`transform_function`). As with all expressions, `row_selection` can be
defined outside of the function call using the `expression`
function.


##### transforms

NOT SUPPORTED. An expression of the form  that represents the first round
of variable transformations. As with
all expressions, `transforms` (or `row_selection`) can be defined
outside of the function call using the `expression` function.


##### transform_objects

NOT SUPPORTED. A named list that contains objects that can be
referenced by `transforms`, `transform_function`, and
`row_selection`.


##### transform_function

The variable transformation function.


##### transform_variables

A character vector of input data set variables needed for
the transformation function.


##### transform_packages

NOT SUPPORTED. A character vector specifying additional Python packages
(outside of those specified in `RxOptions.get_option("transform_packages")`) to
be made available and preloaded for use in variable transformation functions.
For example, those explicitly defined in [revoscalepy](https://docs.microsoft.com/en-us/sql/advanced-analytics/python/what-is-revoscalepy) functions via
their `transforms` and `transform_function` arguments or those defined
implicitly via their `formula` or `row_selection` arguments.  The
`transform_packages` argument may also be *None*, indicating that
no packages outside `RxOptions.get_option("transform_packages")` are preloaded.


##### transform_environment

NOT SUPPORTED. A user-defined environment to serve as a parent to all
environments developed internally and used for variable data transformation.
If `transform_environment = None`, a new “hash” environment with parent
[revoscalepy.baseenv](https://docs.microsoft.com/en-us/r-server/r-reference/revoscalpy/baseenv) is used instead.


##### blocks_per_read

Specifies the number of blocks to read for each chunk
of data read from the data source.


##### report_progress

An integer value that specifies the level of reporting
on the row processing progress:

* `0`: no progress is reported. 

* `1`: the number of processed rows is printed and updated. 

* `2`: rows processed and timings are reported. 

* `3`: rows processed and all timings are reported. 


##### verbose

An integer value that specifies the amount of output wanted.
If `0`, no verbose output is printed during calculations. Integer
values from `1` to `4` provide increasing amounts of information.


##### compute_context

Sets the context in which computations are executed,
specified with a valid `RxComputeContext`.
Currently local and `RxInSqlServer` compute contexts
are supported.


##### ensemble

NOT SUPPORTED. Control parameters for ensembling.


### Returns

A [`FastForest`](learners_object.md) object with the trained model.


### Note

This algorithm is multi-threaded and will always attempt to load the entire dataset into
memory.


### See also

[`rx_fast_trees`](rx_fast_trees.md),
[`rx_predict`](rx_predict.md)


### References

[Wikipedia: Random forest](http://en.wikipedia.org/wiki/Random_forest)

[Quantile regression forest](http://jmlr.org/papers/volume7/meinshausen06a/meinshausen06a.pdf)

[From Stumps to Trees to Forests](https://blogs.technet.microsoft.com/machinelearning/2014/09/10/from-stumps-to-trees-to-forests/)


### Example



```
'''
Binary Classification.
'''
import numpy
import pandas
from microsoftml import rx_fast_forest, rx_predict
from revoscalepy.etl.RxDataStep import rx_data_step
from microsoftml.datasets.datasets import infert
import sklearn
if sklearn.__version__ < "0.18":
    from sklearn.cross_validation import train_test_split
else:
    from sklearn.model_selection import train_test_split

infertdf = infert.as_df()
infertdf["isCase"] = infertdf.case == 1
data_train, data_test, y_train, y_test = train_test_split(infertdf, infertdf.isCase)

forest_model = rx_fast_forest(
    formula=" isCase ~ age + parity + education + spontaneous + induced ",
    data=data_train)
    
# RuntimeError: The type (RxTextData) for file is not supported.
score_ds = rx_predict(forest_model, data=data_test,
                     extra_vars_to_write=["isCase", "Score"])
                     
# Print the first five rows
print(rx_data_step(score_ds, number_rows_read=5))
```


Output:



```
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
Not adding a normalizer.
Making per-feature arrays
Changing data from row-wise to column-wise
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Processed 186 instances
Binning and forming Feature objects
Reserved memory for tree learner: 7176 bytes
Starting to train ...
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2402626
Elapsed time: 00:00:00.0445115
Beginning processing data.
Rows Read: 62, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0684475
Finished writing 62 rows.
Writing completed.
Data will be written to C:\Users\xavie\AppData\Local\Temp\rre677602.xdf File will be overwritten if it exists.

Rows Processed: 5 
  isCase PredictedLabel      Score  Probability
0  False          False -14.341213     0.003216
1   True          False -10.374554     0.015522
2  False          False  -5.525540     0.098837
3  False          False  -7.565562     0.046255
4  False          False  -0.530725     0.447126
```



### Example



```
'''
Regression.
'''
import numpy
import pandas
from microsoftml import rx_fast_forest, rx_predict
from revoscalepy.etl.RxDataStep import rx_data_step
from microsoftml.datasets.datasets import airquality
import sklearn
if sklearn.__version__ < "0.18":
    from sklearn.cross_validation import train_test_split
else:
    from sklearn.model_selection import train_test_split

airquality = airquality.as_df()


######################################################################
# Estimate a regression fast forest
# Use the built-in data set 'airquality' to create test and train data

df = airquality[airquality.Ozone.notnull()]
df["Ozone"] = df.Ozone.astype(float)

data_train, data_test, y_train, y_test = train_test_split(df, df.Ozone)

airFormula = " Ozone ~ Solar_R + Wind + Temp "

# Regression Fast Forest for train data
ff_reg = rx_fast_forest(airFormula, method="regression", data=data_train)

# Put score and model variables in data frame
score_df = rx_predict(ff_reg, data=data_test, write_model_vars=True)
print(score_df.head())

# Plot actual versus predicted values with smoothed line
# Supported in the next version.
# rx_line_plot(" Score ~ Ozone ", type=["p", "smooth"], data=score_df)
```


Output:



```
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
Not adding a normalizer.
Making per-feature arrays
Changing data from row-wise to column-wise
Beginning processing data.
Rows Read: 87, Read Time: 0, Transform Time: 0
Beginning processing data.
Warning: Skipped 4 instances with missing features during training
Processed 83 instances
Binning and forming Feature objects
Reserved memory for tree learner: 22308 bytes
Starting to train ...
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.1052124
Elapsed time: 00:00:00.0210603
Beginning processing data.
Rows Read: 29, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0631821
Finished writing 29 rows.
Writing completed.
   Solar_R  Wind  Temp      Score
0     36.0  14.3    72  13.254936
1    238.0   6.3    77  24.971542
2    259.0   9.7    73  22.683887
3     71.0  10.3    77  13.646682
4    320.0  16.6    73  22.557140
```

