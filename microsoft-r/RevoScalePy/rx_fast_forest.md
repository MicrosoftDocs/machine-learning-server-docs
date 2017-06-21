--- 
 
# required metadata 
title: "Fast Forest" 
description: "Machine Learning Fast Forest" 
keywords: "models, classification, regression" 
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

# rx_fast_forest

microsoftml.modules.fast_forest.rx_fast_forest(formula, data, method: [‘binary’, ’regression’] = ‘binary’, num_trees=100, num_leaves=20, min_split=10, example_fraction=0.7, feature_fraction=1, split_fraction=1, num_bins=255, first_use_penalty=0, gain_conf_level=0, train_threads=8, random_seed=None, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)



## Description

Machine Learning Fast Forest


## Details

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
[``rx_fast_trees``](rx_fast_trees#microsoftml.modules.fast_trees.rx_fast_trees).
The model consists of an ensemble of decision trees. Each tree in a decision
forest outputs a Gaussian distribution by way of prediction. An aggregation
is performed over the ensemble of trees to find a Gaussian distribution
closest to the combined distribution for all trees in the model.

This decision forest classifier consists of an ensemble of decision trees.
Generally, ensemble models provide better coverage and accuracy than single
decision trees. Each tree in a decision forest outputs a Gaussian distribution
by way of prediction. An aggregation is performed over the ensemble of trees
to find a Gaussian distribution closest to the combined distribution
for all trees in the model.


## Parameters


### formula

The formula as described in ``revo_scale_r``.
Interaction terms and ``F()`` are not currently supported in the
.


### data

A data source object or a character string specifying a
*.xdf* file or a data frame object.


### method

A character string denoting Fast Tree type:

* ``"binary"`` for the default Fast Tree Binary Classification or 

* ``"regression"`` for Fast Tree Regression. 


### num_trees

Specifies the total number of decision trees to create in
the ensemble.By creating more decision trees, you can potentially get
better coverage, but the training time increases. The default value is 100.


### num_leaves

The maximum number of leaves (terminal nodes) that can be created
in any tree. Higher values potentially increase the size of the tree and get
better precision, but risk overfitting and requiring longer training times.
The default value is 20.


### min_split

Minimum number of training instances required to form a
leaf. That is, the minimal number of documents allowed in a leaf of a
regression tree, out of the sub-sampled data. A ‘split’ means that features
in each level of the tree (node) are randomly divided. The default value is 10.


### example_fraction

The fraction of randomly chosen instances to use
for each tree. The default value is 0.7.


### feature_fraction

The fraction of randomly chosen features to use for
each tree. The default value is 0.7.


### split_fraction

The fraction of randomly chosen features to use on
each split. The default value is 0.7.


### num_bins

Maximum number of distinct values (bins) per feature.
The default value is 255.


### first_use_penalty

The feature first use penalty coefficient. The default
value is 0.


### gain_conf_level

Tree fitting gain confidence requirement (should be in
the range [0,1) ). The default value is 0.


### train_threads

The number of threads to use in training. If *None*
is specified, the number of threads to use is determined internally.
The default value is *None*.


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

An expression of the form  that represents the first round
of variable transformations. As with
all expressions, ``transforms`` (or ``row_selection``) can be defined
outside of the function call using the ``expression()`` function.


### transform_objects

A named list that contains objects that can be
referenced by ``transforms``, ``transformsFunc``, and
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

* ``rxFastForest``: A ``rxFastForest`` object with the trained model. 

* ``FastForest``: A learner specification object of class ``maml`` for the Fast Forest trainer. 


## Note

This algorithm is multi-threaded and will always attempt to load the entire dataset into
memory.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also

[``rx_fast_trees``](rx_fast_trees#microsoftml.modules.fast_trees.rx_fast_trees),
``rx_fast_linear()``,
[``rx_logistic_regression``](rx_logistic_regression#microsoftml.modules.logistic_regression.rx_logistic_regression),
``rx_neural_net()``,
``rx_one_class_svm()``,
``featurize_text()``,
``categorical()``,
``categorical_hash()``,
``rx_predict.ml_model()``.


## References

[Wikipedia: Random forest](http://en.wikipedia.org/wiki/Random_forest)

[Quantile regression forest](http://jmlr.org/papers/volume7/meinshausen06a/meinshausen06a.pdf)

[From Stumps to Trees to Forests](https://blogs.technet.microsoft.com/machinelearning/2014/09/10/from-stumps-to-trees-to-forests/)


## Example



```
# Estimate a binary classification forest
infert1 <- infert
infert1$isCase = (infert1$case == 1)
forestModel <- rxFastForest(formula = isCase ~ age + parity + education + spontaneous + induced,
        data = infert1)

# Create text file with per-instance results using rxPredict
txtOutFile <- tempfile(pattern = "scoreOut", fileext = ".txt")
txtOutDS <- RxTextData(file = txtOutFile)
scoreDS <- rxPredict(forestModel, data = infert1,
   extraVarsToWrite = c("isCase", "Score"), outData = txtOutDS)
   
# Print the fist ten rows   
rxDataStep(scoreDS, numRows = 10)
   
# Clean-up
file.remove(txtOutFile)

######################################################################
# Estimate a regression fast forest

# Use the built-in data set 'airquality' to create test and train data
DF <- airquality[!is.na(airquality$Ozone), ]	
DF$Ozone <- as.numeric(DF$Ozone)
randomSplit <- rnorm(nrow(DF))
trainAir <- DF[randomSplit >= 0,]
testAir <- DF[randomSplit < 0,]
airFormula <- Ozone ~ Solar.R + Wind + Temp

# Regression Fast Forest for train data
rxFastForestReg <- rxFastForest(airFormula, type = "regression", 
    data = trainAir)  
    
# Put score and model variables in data frame
rxFastForestScoreDF <- rxPredict(rxFastForestReg, data = testAir, 
    writeModelVars = TRUE)
    
# Plot actual versus predicted values with smoothed line
rxLinePlot(Score ~ Ozone, type = c("p", "smooth"), data = rxFastForestScoreDF)
```

