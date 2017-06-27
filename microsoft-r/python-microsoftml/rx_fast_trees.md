--- 
 
# required metadata 
title: "Fast Tree" 
description: "Machine Learning Fast Tree" 
keywords: "models, classification, regression" 
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

# rx_fast_trees



```
microsoftml.modules.fast_trees.rx_fast_trees(formula, data, method: [‘binary’, ’regression’] = ‘binary’, num_trees=100, num_leaves=20, learning_rate=0.2, min_split=10, example_fraction=0.7, feature_fraction=1, split_fraction=1, num_bins=255, first_use_penalty=0, gain_conf_level=0, unbalanced_sets=False, train_threads=8, random_seed=None, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)
```




## Description

Machine Learning Fast Tree


## Details

rxFastTrees is an implementation of FastRank. FastRank is an efficient
implementation of the MART gradient boosting algorithm. Gradient boosting is
a machine learning technique for regression problems. It builds each
regression tree in a step-wise fashion, using a predefined loss function to
measure the error for each step and corrects for it in the next. So this
prediction model is actually an ensemble of weaker prediction models. In
regression problems, boosting builds a series of of such trees in a step-wise
fashion and then selects the optimal tree using an arbitrary differentiable
loss function.

MART learns an ensemble of regression trees, which is a decision tree with
scalar values in its leaves. A decision (or regression) tree is a
binary tree-like flow chart, where at each interior node one decides which
of the two child nodes to continue to based on one of the feature values
from the input. At each leaf node, a value is returned. In the
interior nodes, the decision is based on the test ``"x <= v"``, where
``x`` is the value of the feature in the input sample and ``v`` is one
of the possible values of this feature. The functions that can be produced
by a regression tree are all the piece-wise constant functions.

The ensemble of trees is produced by computing, in each step, a regression
tree that approximates the gradient of the loss function, and adding it to
the previous tree with coefficients that minimize the loss of the new tree.
The output of the ensemble produced by MART on a given instance is the sum
of the tree outputs.

* In case of a binary classification problem, the output is converted to a probability by using some form of calibration. 

* In case of a regression problem, the output is the predicted value of the function. 

* In case of a ranking problem, the instances are ordered by the output value of the ensemble. 

If ``method`` is set to ``"regression"``, a regression version of
FastTree is used. If set to ``"ranking"``, a ranking version of FastTree
is used. In the ranking case, the instances should be ordered by the output
of the tree ensemble. The only difference in the settings of these
versions is in the calibration settings, which are needed only for
classification.


## Parameters


### formula

The formula as described in ``revo_scale_r``.
Interaction terms and ``F()`` are not currently supported in the
.


### data

A data source object or a character string specifying a
*.xdf* file or a data frame object.


### method

A character string that specifies the type of Fast Tree:
``"binary"`` for the default Fast Tree Binary Classification or
``"regression"`` for Fast Tree Regression.


### num_trees

Specifies the total number of decision trees to create in
the ensemble.By creating more decision trees, you can potentially get
better coverage, but the training time increases. The default value is 100.


### num_leaves

The maximum number of leaves (terminal nodes) that can be created
in any tree. Higher values potentially increase the size of the tree and get
better precision, but risk overfitting and requiring longer training times.
The default value is 20.


### learning_rate

Determines the size of the step taken in the direction
of the gradient in each step of the learning process.  This determines how
fast or slow the learner converges on the optimal solution.  If the step
size is too big, you might overshoot the optimal solution.  If the step size
is too samll, training takes longer to converge to the best solution.


### min_split

Minimum number of training instances required to form a
leaf. That is, the minimal number of documents allowed in a leaf of a
regression tree, out of the sub-sampled data. A ‘split’ means that features
in each level of the tree (node) are randomly divided. The default value is
10. Only the number of instances is counted even if instances are weighted.


### example_fraction

The fraction of randomly chosen instances to use
for each tree. The default value is 0.7.


### feature_fraction

The fraction of randomly chosen features to use for
each tree. The default value is 1.


### split_fraction

The fraction of randomly chosen features to use on
each split. The default value is 1.


### num_bins

Maximum number of distinct values (bins) per feature. If the
feature has fewer values than the number indicated, each value is placed
in its own bin.  If there are more values, the algorithm creates
``numBins`` bins.


### first_use_penalty

The feature first use penalty coefficient. This is a
form of regularization that incurs a penalty for using a new feature when
creating the tree. Increase this value to create trees that don’t use many
features. The default value is 0.


### gain_conf_level

Tree fitting gain confidence requirement (should be in
the range [0,1)). The default value is 0.


### unbalanced_sets

If ``TRUE``, derivatives optimized for unbalanced
sets are used. Only applicable when ``type`` equal to ``"binary"``.
The default value is ``FALSE``.


### train_threads

The number of threads to use in training. The default
value is 8.


### random_seed

Specifies the random seed. The default value is *None*.


### ml_transforms

Specifies a list of MicrosoftML transforms to be
performed on the data before training or *None* if no transforms are
to be performed. See ``featurize_text()``, ``categorical()``,
and ``categorical_hash()``, for transformations that are supported.
These transformations are performed after any specified R transformations.
The default value is *None*.


### ml_transform_vars

Specifies a character vector of variable names
to be used in ``mlTransforms`` or *None* if none are to be used.
The default value is *None*.


### row_selection

Specifies the rows (observations) from the data set that
are to be used by the model with the name of a logical variable from the
data set (in quotes) or with a logical expression using variables in the
data set. For example, ``row_selection = "old"`` will only use
observations in which the value of the variable ``old`` is ``TRUE``.
``row_selection = (age > 20) & (age < 65) & (log(income) > 10)`` only uses
observations in which the value of the ``age`` variable is between
20 and 65 and the value of the ``log`` of the ``income`` variable is
greater than 10. The row selection is performed after processing any data
transformations (see the arguments ``transforms`` or
``transform_func``). As with all expressions, ``row_selection`` can be
defined outside of the function call using the ``expression()``
function.


### transforms

An expression of the form  that represents the
first round of variable transformations. As with
all expressions, ``transforms`` (or ``row_selection``) can be defined
outside of the function call using the ``expression()`` function.


### transform_objects

A named list that contains objects that can be
referenced by ``transforms``, ``transform_func``, and
``row_selection``.


### transform_func

The variable transformation function.


### transform_vars

A character vector of input data set variables needed for
the transformation function.


### transform_packages

A character vector specifying additional R packages
(outside of those specified in ``rxGetOption("transformPackages")``) to
be made available and preloaded for use in variable transformation functions.
For exmple, those explicitly defined in  functions via
their ``transforms`` and ``transform_func`` arguments or those defined
implicitly via their ``formula`` or ``row_selection`` arguments.  The
``transform_packages`` argument may also be *None*, indicating that
no packages outside ``rxGetOption("transformPackages")`` are preloaded.


### transform_envir

A user-defined environment to serve as a parent to all
environments developed internally and used for variable data transformation.
If ``transformEnvir = NULL``, a new “hash” environment with parent
``baseenv()`` is used instead.


### blocks_per_read

Specifies the number of blocks to read for each chunk
of data read from the data source.


### report_progress

An integer value that specifies the level of reporting
on the row processing progress:

* ``0``: no progress is reported. 

* ``1``: the number of processed rows is printed and updated. 

* ``2``: rows processed and timings are reported. 

* ``3``: rows processed and all timings are reported. 


### verbose

An integer value that specifies the amount of output wanted.
If ``0``, no verbose output is printed during calculations. Integer
values from ``1`` to ``4`` provide increasing amounts of information.


### compute_context

Sets the context in which computations are executed,
specified with a valid ``revo_scale_r``.
Currently local and ``revo_scale_r`` compute contexts
are supported.


### ensemble

Control parameters for ensembling.


## Returns

* ``rxFastTrees``: A ``rxFastTrees`` object with the trained model. 

* ``FastTree``: A learner specification object of class ``maml`` for the Fast Tree trainer. 


## Note

This algorithm is multi-threaded and will always attempt to load the entire dataset into
memory.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


## See also

[``rx_fast_trees``](microsoftml/modules/fast_trees/rx_fast_trees.md),
[``rx_fast_forest``](rx_fast_forest#microsoftml.modules.fast_forest.rx_fast_forest.md),
``rx_fast_linear``,
[``rx_logistic_regression``](rx_logistic_regression#microsoftml.modules.logistic_regression.rx_logistic_regression.md),
``rx_one_class_svm``,
``featurize_text``,
[``categorical``](categorical#microsoftml.modules.categorical.categorical.md),
[``categorical_hash``](categorical_hash#microsoftml.modules.categorical.categorical_hash.md),
[``rx_predict``](rx_predict#microsoftml.modules.predict.rx_predict.md)


## References

[Wikipedia: Gradient boosting (Gradient tree boosting)](https://en.wikipedia.org/wiki/Gradient_boosting#Gradient_tree_boosting.md)

[Greedy function approximation: A gradient boosting machine.](http://projecteuclid.org/DPubS?service=UI&version=1.0&verb=Display&handle=euclid.aos/1013203451.md)


## Example



```
# Estimate a binary classification tree
infert1 <- infert
infert1$isCase = (infert1$case == 1)
treeModel <- rxFastTrees(formula = isCase ~ age + parity + education + spontaneous + induced,
        data = infert1)

# Create xdf file with per-instance results using rxPredict
xdfOut <- tempfile(pattern = "scoreOut", fileext = ".xdf")
scoreDS <- rxPredict(treeModel, data = infert1,
   extraVarsToWrite = c("isCase", "Score"), 
   outData = xdfOut)
   
rxDataStep(scoreDS, numRows = 10)

# Clean-up
file.remove(xdfOut)

######################################################################
# Estimate a regression fast tree

# Use the built-in data set 'airquality' to create test and train data
DF <- airquality[!is.na(airquality$Ozone), ]	
DF$Ozone <- as.numeric(DF$Ozone)
randomSplit <- rnorm(nrow(DF))
trainAir <- DF[randomSplit >= 0,]
testAir <- DF[randomSplit < 0,]
airFormula <- Ozone ~ Solar.R + Wind + Temp

# Regression Fast Tree for train data
fastTreeReg <- rxFastTrees(airFormula, type = "regression", 
    data = trainAir)  
    
# Put score and model variables in data frame
fastTreeScoreDF <- rxPredict(fastTreeReg, data = testAir, 
    writeModelVars = TRUE)
    
# Plot actual versus predicted values with smoothed line
rxLinePlot(Score ~ Ozone, type = c("p", "smooth"), data = fastTreeScoreDF)
```

