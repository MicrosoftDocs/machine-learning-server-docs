--- 
 
# required metadata 
title: "rx_dforest: Fits classification and regression decision forests (revoscalepy)" 
description: "Fits classification and regression decision forests on an .xdf file or data frame for small or large data using parallel external memory algorithm." 
keywords: "learner, tree" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
---

# rx_dforest


 


## Usage



```
revoscalepy.rx_dforest(formula, data, output_file=None, write_model_vars=False,
    overwrite=False, pweights=None, fweights=None, cost=None, min_split=None,
    min_bucket=None, max_depth=10, cp=0, max_compete=0, max_surrogate=0,
    use_surrogate=2, surrogate_style=0, n_tree=10, m_try=None, replace=True,
    cutoff=None, strata=None, sample_rate=None, importance=False, seed=None,
    compute_cp_table=False, compute_oob_error=1, max_num_bins=None,
    max_unordered_levels=32, remove_missings=False, use_sparse_cube=False,
    find_splits_in_parallel=True, row_selection=None, transforms=None,
    transform_objects=None, transform_function=None, transform_variables=None,
    transform_packages=None, blocks_per_read=1, report_progress=2, verbose=0,
    compute_context=None, xdf_compression_level=1, **kwargs)
```





## Description

Fits classification and regression decision forests on an .xdf file or
data frame for small or large data using parallel external memory algorithm.


## Arguments


### formula

Statistical model using symbolic formulas.


### data

Either a data source object, a character string specifying a
‘.xdf’ file, or a data frame object.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### output_file

Either an RxXdfData data source object or a character
string specifying the ‘.xdf’ file for storing the resulting node indices.
If None, then no node indices are stored to disk. If the input data is a
data frame, the node indices are returned automatically.


### write_model_vars

Bool value. If True, and the output file is
different from the input file, variables in the model will be written to
the output file in addition to the node numbers. If variables from the
input data set are transformed in the model, the transformed variables will
also be written out.


### overwrite

Bool value. If True, an existing output_file with an
existing column named outColName will be overwritten.


### pweights

Character string specifying the variable of numeric values
to use as probability weights for the observations.


### fweights

Character string specifying the variable of integer values
to use as frequency weights for the observations.


### cost

A vector of non-negative costs, containing one element for
each variable in the model. Defaults to one for all variables. When
deciding which split: to choose, the improvement on splitting on a variable
is divided by its cost.


### min_split

The minimum number of observations that must exist in a
node before a split is attempted. By default, this is sqrt(num of obs). For
non-XDF data sources, as (num of obs) is unknown in advance, it is wisest
to specify this argument directly.


### min_bucket

The minimum number of observations in a terminal node
(or leaf). By default, this is min_split/3.


### cp

Numeric scalar specifying the complexity parameter. Any split
that does not decrease overall lack-of-fit by at least cp is not attempted.


### max_compete

The maximum number of competitor splits retained in the
output. These are useful model diagnostics, as they allow you to compare
splits in the output with the alternatives.


### max_surrogate

The maximum number of surrogate splits retained in
the output. See the Details for a description of how surrogate splits are
used in the model fitting. Setting this to 0 can greatly improve the
performance of the algorithm; in some cases almost half the computation
time is spent in computing surrogate splits.


### use_surrogate

An integer specifying how surrogates are to be used
in the splitting process:
0: Display-only; observations with a missing value for the primary

    split variable are not sent further down the tree.

1: Use surrogates, in order, to split observations missing the primary
    split variable. If all surrogates are missing, the observation is not
    split.

2: Use surrogates, in order, to split observations missing the primary
    split variable. If all surrogates are missing or max_surrogate=0, send
    the observation in the majority direction.

The 0 value corresponds to the behavior of the tree function, and 2
    (the default) corresponds to the recommendations of Breiman et al.


### surrogate_style

An integer controlling selection of a best
surrogate. The default, 0, instructs the program to use the total number of
correct classifications for a potential surrogate, while 1 instructs the
program to use the percentage of correct classification over the
non-missing values of the surrogate. Thus, 0 penalizes potential surrogates
with a large number of missing values.


### n_tree

A positive integer specifying the number of trees to grow.


### m_try

A positive integer specifying the number of variables to
sample as split candidates at each tree node. The default values are
sqrt(num of vars) for classification and (num of vars)/3 for regression.


### replace

A bool value specifying if the sampling of observations
should be done with or without replacement.


### cutoff

(Classification only) A vector of length equal to the number
of classes specifying the dividing factors for the class votes. The default
is 1/(num of classes).


### strata

A character string specifying the (factor) variable to use
for stratified sampling.


### sample_rate

A scalar or a vector of positive values specifying the
percentage(s) of observations to sample for each tree:
for unstratified sampling: A scalar of positive value specifying the

    percentage of observations to sample for each tree. The default is 1.0
    for sampling with replacement (i.e., replace=True) and 0.632 for
    sampling without replacement (i.e., replace=False).

for stratified sampling: A vector of positive values of length equal to
    the number of strata specifying the percentages of observations to
    sample from the strata for each tree.


### importance

A bool value specifying if the importance of
predictors should be assessed.


### seed

An integer that will be used to initialize the random number
generator. The default is random. For reproducibility, you can specify the
random seed either using set.seed or by setting this seed argument as part
of your call.


### compute_oob_error

An integer specifying whether and how to compute
the prediction error for out-of-bag samples:
<0: never. This option may reduce the computation time.
=0: only once for the entire forest.
>0: once for each addition of a tree. This is the default.


### max_num_bins

The maximum number of bins to use to cut numeric data.
The default is min(1001, max(101, sqrt(num of obs))). For non-XDF data
sources, as (num of obs) is unknown in advance, it is wisest to specify
this argument directly. If set to 0, unit binning will be used instead of
cutting. See the ‘Details’ section for more information.


### max_unordered_levels

The maximum number of levels allowed for an
unordered factor predictor for multiclass (>2) classification.


### remove_missings

Bool value. If True, rows with missing values
are removed and will not be included in the output data.


### use_sparse_cube

Bool value. If True, sparse cube is used.


### find_splits_in_parallel

Bool value. If True, optimal splits for
each node are determined using parallelization methods; this will typically
speed up computation as the number of nodes on the same level is increased.


### row_selection

None. Not currently supported, reserved for future use.


### transforms

None. Not currently supported, reserved for future use.


### transform_objects

A dictionary of variables besides the data that are used in the transform function.
See rx_data_step for examples.


### transform_function

Name of the function that will be used to modify the data before the model is built.
The variables used in the transformation function must be specified in transform_objects.
See rx_data_step for examples.


### transform_variables

List of strings of the column names needed
for the transform function.


### transform_packages

None. Not currently supported, reserved for future use.


### blocks_per_read

number of blocks to read for each chunk of data read from
the data source.


### report_progress

Integer value with options:
0: No progress is reported.
1: The number of processed rows is printed and updated.
2: Rows processed and timings are reported.
3: Rows processed and all timings are reported.


### verbose

Integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


### compute_context

A RxComputeContext object for prediction.


### kwargs

Additional parameters


## Returns

A RxDForestResults object of dtree model.


## See also

[`rx_predict`](rx-predict.md),
[`rx_predict_rx_dforest`](rx-predict-rx-dforest.md).


## Example



```
import os
from revoscalepy import rx_dforest, rx_import, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import(input_data = ds)

# classification
formula = "Kyphosis ~ Number + Start"
method = "class"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}

dforest = rx_dforest(formula, data = kyphosis, pweights = "Age", method = method,
                     parms = parms, cost = [2, 3], max_num_bins = 100, importance = True,
                     n_tree = 3, m_try = 2, sample_rate = 0.5,
                     replace = False, seed = 0, compute_oob_error = 1)

# regression
formula = "Age ~ Number + Start"
method = "anova"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}

dforest = rx_dforest(formula, data = kyphosis, pweights = "Kyphosis", method = method,
                     parms = parms, cost = [2, 3], max_num_bins = 100, importance = True,
                     n_tree = 3, m_try = 2, sample_rate = 0.5,
                     replace = False, seed = 0, compute_oob_error = 1)
```

