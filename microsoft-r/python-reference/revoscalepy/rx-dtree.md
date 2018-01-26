--- 
 
# required metadata 
title: "rx_dtree: Fits classification and regression trees (revoscalepy)" 
description: "Fit classification and regression trees on an .xdf file or data frame for small or large data using parallel external memory algorithm." 
keywords: "learner, tree" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
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

# rx_dtree


 


## Usage



```
revoscalepy.rx_dtree(formula, data, output_file=None,
    output_column_name='.rxNode', write_model_vars=False,
    extra_vars_to_write=None, overwrite=False, pweights=None, fweights=None,
    method=None, parms=None, cost=None, min_split=None, min_bucket=None,
    max_depth=10, cp=0, max_compete=0, max_surrogate=0, use_surrogate=2,
    surrogate_style=0, x_val=2, max_num_bins=None, max_unordered_levels=32,
    remove_missings=False, compute_obs_node_id=None, use_sparse_cube=False,
    find_splits_in_parallel=True, prune_cp=0, row_selection=None,
    transforms=None, transform_objects=None, transform_function=None,
    transform_variables=None, transform_packages=None, blocks_per_read=1,
    report_progress=2, verbose=0, compute_context=None, xdf_compression_level=1,
    **kwargs)
```





## Description

Fit classification and regression trees on an .xdf file or data frame for
small or large data using parallel external memory algorithm.


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


### output_column_name

Character string to be used as a column name for
the resulting node indices if output_file is not None. Note that make.names is
used on outColName to ensure that the column name is valid. If the output_file
is an RxOdbcData source, dots are first converted to underscores. Thus, the
default outColName becomes “X_rxNode”.


### write_model_vars

Bool value. If True, and the output file is
different from the input file, variables in the model will be written to
the output file in addition to the node numbers. If variables from the
input data set are transformed in the model, the transformed variables will
also be written out.


### extra_vars_to_write

None or list of strings of additional
variables names from the input data or transforms to include in the
output_file. If writeModelVars is True, model variables will be included as well.


### overwrite

Bool value. If True, an existing output_file with an
existing column named outColName will be overwritten.


### pweights

Character string specifying the variable of numeric values
to use as probability weights for the observations.


### fweights

Character string specifying the variable of integer values
to use as frequency weights for the observations.


### method

Character string specifying the splitting method. Currently,
only “class” or “anova” are supported. The default is “class” if the
response is a factor, otherwise “anova”.


### parms

Optional list with components specifying additional
parameters for the “class” splitting method, as follows:

prior: A vector of prior probabilities. The priors must be positive and
    sum to 1. The default priors are proportional to the data counts.

loss: A loss matrix, which must have zeros on the diagonal and positive
    off-diagonal elements. By default, the off-diagonal elements are set to 1.

split: The splitting index, either gini (the default) or information.

If parms is specified, any of the components can be specified or
    omitted. The defaults will be used for missing components.


### cost

A vector of non-negative costs, containing one element for
each variable in the model. Defaults to one for all variables. When
deciding which split to choose, the improvement on splitting on a variable
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
the output. Setting this to 0 can greatly improve the performance of the
algorithm; in some cases almost half the computation time is spent in
computing surrogate splits.


### use_surrogate

An integer specifying how surrogates are to be used
in the splitting process:
0: Display-only; observations with a missing value for the primary

    split variable are not sent further down the tree.

1: Use surrogates,in order, to split observations missing the primary
    split variable. If all surrogates are missing, the observation is not
    split.

2: Use surrogates, in order, to split observations missing the primary
    split variable. If all surrogates are missing or max_surrogate=0, send
    the observation in the majority direction.

The 0 value corresponds to the behavior of the tree function, and 2
    (the default) corresponds to the recommendations of Breiman et al.


### x_val

The number of cross-validations to be performed along with
the model building. Currently, 1:x_val is repeated and used to identify the
folds. If not zero, the cptable component of the resulting model will
contain both the mean (xerror) and standard deviation (xstd) of the
cross-validation errors, which can be used to select the optimal
cost-complexity pruning of the fitted tree. Set it to zero if external
cross-validation will be used to evaluate the fitted model because a value
of k increases the compute time to (k+1)-fold over a value of zero.


### surrogate_style

An integer controlling selection of a best
surrogate. The default, 0, instructs the program to use the total number of
correct classifications for a potential surrogate, while 1 instructs the
program to use the percentage of correct classification over the
non-missing values of the surrogate. Thus, 0 penalizes potential surrogates
with a large number of missing values.


### max_depth

The maximum depth of any tree node. The computations take
much longer at greater depth, so lowering max_depth can greatly speed up
computation time.


### max_num_bins

The maximum number of bins to use to cut numeric data.
The default is min(1001, max(101, sqrt(num of obs))). For non-XDF data
sources, as (num of obs) is unknown in advance, it is wisest to specify
this argument directly. If set to 0, unit binning will be used instead of
cutting.


### max_unordered_levels

The maximum number of levels allowed for an
unordered factor predictor for multiclass (>2) classification.


### remove_missings

Bool value. If True, rows with missing values
are removed and will not be included in the output data.


### compute_obs_node_id

Bool value or None. If True, the tree node
IDs for all the observations are computed and returned. If None, the IDs
are computed for data.frame with less than 1000 observations and are
returned as the where component in the fitted rxDTree object.


### use_sparse_cube

Bool value. If True, sparse cube is used.


### find_splits_in_parallel

Bool value. If True, optimal splits for
each node are determined using parallelization methods; this will typically
speed up computation as the number of nodes on the same level is increased.


### prune_cp

Optional complexity parameter for pruning. If prune_cp >
0, prune.rxDTree is called on the completed tree with the specified
prune_cp and the pruned tree is returned. This contrasts with the cp
parameter that determines which splits are considered in growing the tree.
The option prune_cp=”auto” causes rxDTree to call the function
rxDTreeBestCp on the completed tree, then use its return value as the cp
value for prune.rxDTree.


### row_selection

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

A RxDTreeResults object of dtree model.


## See also

[`rx_predict`](rx-predict.md),
[`rx_predict_rx_dtree`](rx-predict-rx-dtree.md).


## Example



```
import os
from revoscalepy import rx_dtree, rx_import, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import(input_data = ds)

# classification
formula = "Kyphosis ~ Number + Start"
method = "class"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}
cost = [2,3]
dtree = rx_dtree(formula, data = kyphosis, pweights = "Age", method = method, parms = parms, cost = cost, max_num_bins = 100)

# regression
formula = "Age ~ Number + Start"
method = "anova"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}
cost = [2,3]
dtree = rx_dtree(formula, data = kyphosis, pweights = "Kyphosis", method = method, parms = parms, cost = cost, max_num_bins = 100)

# transform function
def my_transform(dataset, context):
    dataset['arrdelay2'] = dataset['ArrDelay'] * 10
    dataset['crsdeptime2'] = dataset['CRSDepTime']
    # Use the follow code to set high/low values for new columns
    # rx_attributes metadata needs to be set last
    dataset['arrdelay2'].rx_attributes = {'.rxLowHigh': [-860.0, 14900.0]}
    dataset['crsdeptime2'].rx_attributes = {'.rxLowHigh': [0.016666999086737633, 23.983333587646484]}
    return dataset

data_path = RxOptions.get_option("sampleDataDir")
data = RxXdfData(os.path.join(data_path, "AirlineDemoSmall.xdf")).head(20)
form = "ArrDelay ~ arrdelay2 + crsdeptime2"
dtree = rx_dtree(form, data=data, transform_function=my_transform, transform_variables=["ArrDelay", "CRSDepTime", "DayOfWeek"])
```

