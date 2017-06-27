--- 
 
# required metadata 
title: "Parallel External Memory Algorithm for Classification and Regression Trees" 
description: "Fit classification and regression trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm." 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# rx_dtree
=======
## rx_dtree


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.functions.RxDTree.rx_dtree(formula, data, output_file=None, output_column_name=’.rxNode’, write_model_vars=False, extra_vars_to_write=None, overwrite=False, pweights=None, fweights=None, method=None, parms=None, cost=None, min_split=None, min_bucket=None, max_depth=10, cp=0, max_compete=0, max_surrogate=0, use_surrogate=2, surrogate_style=0, x_val=2, max_num_bins=None, max_unordered_levels=32, remove_missings=False, compute_obs_node_id=None, use_sparse_cube=False, find_splits_in_parallel=True, prune_cp=0, row_selection=None, transforms=None, transform_objects=None, transform_function=None, transform_variables=None, transform_packages=None, transform_environment=None, blocks_per_read=1, report_progress=2, verbose=0, compute_context=None, xdf_compression_level=1, **kwargs)
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Fit classification and regression trees on an ‘.xdf’ file or data frame for small or large data using parallel external memory algorithm.


<<<<<<< HEAD
## Parameters


### formula
=======
### Arguments


##### formula
>>>>>>> heidist-revoscalepy

formula as described in rxFormula. Currently, formula
functions are not supported.


<<<<<<< HEAD
### data
=======
##### data
>>>>>>> heidist-revoscalepy

either a data source object, a character string specifying a
‘.xdf’ file, or a data frame object.


<<<<<<< HEAD
### output_file
=======
##### output_file
>>>>>>> heidist-revoscalepy

either an RxXdfData data source object or a character
string specifying the ‘.xdf’ file for storing the resulting node indices.
If None, then no node indices are stored to disk. If the input data is a
data frame, the node indices are returned automatically. If rowSelection is
specified and not None, then outFile cannot be the same as the data since
the resulting set of node indices will generally not have the same number
of rows as the original data source.


<<<<<<< HEAD
### output_column_name
=======
##### output_column_name
>>>>>>> heidist-revoscalepy

character string to be used as a column name for
the resulting node indices if outFile is not None. Note that make.names is
used on outColName to ensure that the column name is valid. If the outFile
is an RxOdbcData source, dots are first converted to underscores. Thus, the
default outColName becomes “X_rxNode”.


<<<<<<< HEAD
### write_model_vars
=======
##### write_model_vars
>>>>>>> heidist-revoscalepy

logical value. If True, and the output file is
different from the input file, variables in the model will be written to
the output file in addition to the node numbers. If variables from the
input data set are transformed in the model, the transformed variables will
also be written out.


<<<<<<< HEAD
### extra_vars_to_write
=======
##### extra_vars_to_write
>>>>>>> heidist-revoscalepy

None or character vector of additional
variables names from the input data or transforms to include in the
outFile. If writeModelVars is True, model variables will be included as well.


<<<<<<< HEAD
### overwrite
=======
##### overwrite
>>>>>>> heidist-revoscalepy

logical value. If True, an existing outFile with an
existing column named outColName will be overwritten.


<<<<<<< HEAD
### pweights
=======
##### pweights
>>>>>>> heidist-revoscalepy

character string specifying the variable of numeric values
to use as probability weights for the observations.


<<<<<<< HEAD
### fweights
=======
##### fweights
>>>>>>> heidist-revoscalepy

character string specifying the variable of integer values
to use as frequency weights for the observations.


<<<<<<< HEAD
### method
=======
##### method
>>>>>>> heidist-revoscalepy

character string specifying the splitting method. Currently,
only “class” or “anova” are supported. The default is “class” if the
response is a factor, otherwise “anova”.


<<<<<<< HEAD
### parms
=======
##### parms
>>>>>>> heidist-revoscalepy

optional list with components specifying additional
parameters for the “class” splitting method, as follows:

prior: a vector of prior probabilities. The priors must be positive and
    sum to 1. The default priors are proportional to the data counts.

loss: a loss matrix, which must have zeros on the diagonal and positive
    off-diagonal elements. By default, the off-diagonal elements are set to 1.

split: the splitting index, either gini (the default) or information.

If parms is specified, any of the components can be specified or
    omitted. The defaults will be used for missing components.


<<<<<<< HEAD
### cost
=======
##### cost
>>>>>>> heidist-revoscalepy

a vector of non-negative costs, containing one element for
each variable in the model. Defaults to one for all variables. When
deciding which split: to choose, the improvement on splitting on a variable
is divided by its cost.


<<<<<<< HEAD
### min_split
=======
##### min_split
>>>>>>> heidist-revoscalepy

the minimum number of observations that must exist in a
node before a split is attempted. By default, this is sqrt(num of obs). For
non-XDF data sources, as (num of obs) is unknown in advance, it is wisest
to specify this argument directly.


<<<<<<< HEAD
### min_bucket
=======
##### min_bucket
>>>>>>> heidist-revoscalepy

the minimum number of observations in a terminal node
(or leaf). By default, this is minSplit/3.


<<<<<<< HEAD
### cp
=======
##### cp
>>>>>>> heidist-revoscalepy

numeric scalar specifying the complexity parameter. Any split
that does not decrease overall lack-of-fit by at least cp is not attempted.


<<<<<<< HEAD
### max_compete
=======
##### max_compete
>>>>>>> heidist-revoscalepy

the maximum number of competitor splits retained in the
output. These are useful model diagnostics, as they allow you to compare
splits in the output with the alternatives.


<<<<<<< HEAD
### max_surrogate
=======
##### max_surrogate
>>>>>>> heidist-revoscalepy

the maximum number of surrogate splits retained in
the output. See the Details for a description of how surrogate splits are
used in the model fitting. Setting this to 0 can greatly improve the
performance of the algorithm; in some cases almost half the computation
time is spent in computing surrogate splits.


<<<<<<< HEAD
### use_surrogate
=======
##### use_surrogate
>>>>>>> heidist-revoscalepy

an integer specifying how surrogates are to be used
in the splitting process:
0: display-only; observations with a missing value for the primary

    split variable are not sent further down the tree.

1: use surrogates,in order, to split observations missing the primary
    split variable. If all surrogates are missing, the observation is not
    split.

2: use surrogates, in order, to split observations missing the primary
    split variable. If all surrogates are missing or maxSurrogate=0, send
    the observation in the majority direction.

The 0 value corresponds to the behavior of the tree function, and 2
    (the default) corresponds to the recommendations of Breiman et al.


<<<<<<< HEAD
### x_val
=======
##### x_val
>>>>>>> heidist-revoscalepy

the number of cross-validations to be performed along with
the model building. Currently, 1:xVal is repeated and used to identify the
folds. If not zero, the cptable component of the resulting model will
contain both the mean (xerror) and standard deviation (xstd) of the
cross-validation errors, which can be used to select the optimal
cost-complexity pruning of the fitted tree. Set it to zero if external
cross-validation will be used to evaluate the fitted model because a value
of k increases the compute time to (k+1)-fold over a value of zero.


<<<<<<< HEAD
### surrogate_style
=======
##### surrogate_style
>>>>>>> heidist-revoscalepy

an integer controlling selection of a best
surrogate. The default, 0, instructs the program to use the total number of
correct classifications for a potential surrogate, while 1 instructs the
program to use the percentage of correct classification over the
non-missing values of the surrogate. Thus, 0 penalizes potential surrogates
with a large number of missing values.


<<<<<<< HEAD
### max_depth
=======
##### max_depth
>>>>>>> heidist-revoscalepy

the maximum depth of any tree node. The computations take
much longer at greater depth, so lowering max_depth can greatly speed up
computation time.


<<<<<<< HEAD
### max_num_bins
=======
##### max_num_bins
>>>>>>> heidist-revoscalepy

the maximum number of bins to use to cut numeric data.
The default is min(1001, max(101, sqrt(num of obs))). For non-XDF data
sources, as (num of obs) is unknown in advance, it is wisest to specify
this argument directly. If set to 0, unit binning will be used instead of
cutting. See the ‘Details’ section for more information.


<<<<<<< HEAD
### max_unordered_levels
=======
##### max_unordered_levels
>>>>>>> heidist-revoscalepy

the maximum number of levels allowed for an
unordered factor predictor for multiclass (>2) classification.


<<<<<<< HEAD
### remove_missings
=======
##### remove_missings
>>>>>>> heidist-revoscalepy

logical value. If True, rows with missing values
are removed and will not be included in the output data.


<<<<<<< HEAD
### compute_obs_node_id
=======
##### compute_obs_node_id
>>>>>>> heidist-revoscalepy

logical value or None. If True, the tree node
IDs for all the observations are computed and returned. If None, the IDs
are computed for data.frame with less than 1000 observations and are
returned as the where component in the fitted rxDTree object.


<<<<<<< HEAD
### use_sparse_cube
=======
##### use_sparse_cube
>>>>>>> heidist-revoscalepy

logical value. If True, sparse cube is used.


<<<<<<< HEAD
### find_splits_in_parallel
=======
##### find_splits_in_parallel
>>>>>>> heidist-revoscalepy

logical value. If True, optimal splits for
each node are determined using parallelization methods; this will typically
speed up computation as the number of nodes on the same level is increased.


<<<<<<< HEAD
### prune_cp
=======
##### prune_cp
>>>>>>> heidist-revoscalepy

Optional complexity parameter for pruning. If prune_cp >
0, prune.rxDTree is called on the completed tree with the specified
prune_cp and the pruned tree is returned. This contrasts with the cp
parameter that determines which splits are considered in growing the tree.
The option prune_cp=”auto” causes rxDTree to call the function
rxDTreeBestCp on the completed tree, then use its return value as the cp
value for prune.rxDTree.


<<<<<<< HEAD
### row_selection
=======
##### row_selection
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future
use.


<<<<<<< HEAD
### transforms
=======
##### transforms
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transform_objects
=======
##### transform_objects
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transform_function
=======
##### transform_function
>>>>>>> heidist-revoscalepy

variable transformation function. The variables used
in the transformation function must be specified in transformVars if they
are not variables used in the model. See rxTransform for details.


<<<<<<< HEAD
### transform_variables
=======
##### transform_variables
>>>>>>> heidist-revoscalepy

character vector of input data set variables needed
for the transformation function. See rx_transform for details.


<<<<<<< HEAD
### transform_packages
=======
##### transform_packages
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### transform_environment
=======
##### transform_environment
>>>>>>> heidist-revoscalepy

None. Not currently supported, reserved for future use.


<<<<<<< HEAD
### blocks_per_read
=======
##### blocks_per_read
>>>>>>> heidist-revoscalepy

number of blocks to read for each chunk of data read from
the data source.


<<<<<<< HEAD
### report_progress
=======
##### report_progress
>>>>>>> heidist-revoscalepy

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


<<<<<<< HEAD
### verbose
=======
##### verbose
>>>>>>> heidist-revoscalepy

integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


<<<<<<< HEAD
### compute_context
=======
##### compute_context
>>>>>>> heidist-revoscalepy

a RxComputeContext object for prediction.


<<<<<<< HEAD
### kwargs
=======
##### kwargs
>>>>>>> heidist-revoscalepy

additional parameters


<<<<<<< HEAD
## Returns
=======
### Returns
>>>>>>> heidist-revoscalepy

a rx_dtree_results object of dtree model.


<<<<<<< HEAD
## Author
=======
### Author
>>>>>>> heidist-revoscalepy

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


<<<<<<< HEAD
## See also


## Example
=======
### See also


### Example
>>>>>>> heidist-revoscalepy



```
import os
from revoscalepy import rx_dtree, rx_import_datasource, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import_datasource(input_data = ds)

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
```

