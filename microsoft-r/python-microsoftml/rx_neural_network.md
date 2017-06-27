--- 
 
# required metadata 
title: "Neural Net" 
description: "Neural networks for regression modeling and for Binary and" 
keywords: "models classification regression neural network dnn" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/27/2017" 
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

## rx_neural_network


### Usage



```
microsoftml.modules.neural_network.rx_neural_network(formula, data, method: [‘binary’, ’multiClass’, ’regression’] = ‘binary’, loss_function: {‘LossFunction’: [‘LogLoss’, ’SquaredError’]} = {‘settings’: {}, ’name’: ‘LogLoss’}, num_iterations=100, optimizer: {‘Optimizer’: [‘AdadeltaOptimizer’, ’SgdOptimizer’]} = {‘settings’: {}, ’name’: ‘SgdOptimizer’}, net_definition=None, init_wts_diameter=0.1, max_norm=0, acceleration: {‘MathPlatformKind’: [‘AvxMath’, ’ClrMath’, ’GpuMath’, ’MklMath’, ’SseMath’]} = {‘settings’: {}, ’name’: ‘AvxMath’}, mini_batch_size=1, normalize: [‘No’, ’Warn’, ’Auto’, ’Yes’] = ‘Auto’, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)
```




### Description

Neural networks for regression modeling and for Binary and
multi-class classification.


### Details

A neural network is a class of prediction models inspired by the human
brain. A neural network can be represented as a weighted directed graph.
Each node in the graph is called a neuron. The neurons in the graph are
arranged in layers, where neurons in one layer are connected by a weighted
edge (weights can be 0 or positive numbers) to neurons in the next layer.
The first layer is called the input layer, and each neuron in the input
layer corresponds to one of the features. The last layer of the function is
called the output layer. So in the case of binary neural networks it
contains two output neurons, one for each class, whose values are the
probabilities of belonging to each class. The remaining layers are called
hidden layers. The values of the neurons in the hidden layers and in the
output layer are set by calculating the weighted sum of the values of the
neurons in the previous layer and applying an activation function to that
weighted sum. A neural network model is defined by the structure of its
graph (namely, the number of hidden layers and the number of neurons in each
hidden layer), the choice of activation function, and the weights on the
graph edges. The neural network algorithm tries to learn the optimal weights
on the edges based on the training data.

Although neural networks are widely known for use in deep learning and
modeling complex problems such as image recognition, they are also easily
adapted to regression problems. Any class of statistical models can be considered
a neural network if they use adaptive weights and can approximate non-linear
functions of their inputs. Neural network regression is especially suited to
problems where a more traditional regression model cannot fit a solution.


### Arguments


##### formula

The formula as described in ``revo_scale_r``.
Interaction terms and ``F()`` are not currently supported in the
.


##### data

A data source object or a character string specifying a
*.xdf* file or a data frame object.


##### method

A character string denoting Fast Tree type:

* ``"binary"`` for the default binary classification neural network. 

* ``"multiClass"`` for multi-class classification neural network. 

* ``"regression"`` for a regression neural network. 


##### num_hidden_nodes

The default number of hidden nodes in the neural net.
The default value is 100.


##### num_iterations

The number of iterations on the full training set.
The default value is 100.


##### optimizer

A list specifying either the ``sgd`` or ``adaptive``
optimization algorithm. This list can be created using ``sgd()`` or
``ada_delta_sgd()``. The default value is ``sgd``.


##### net_definition

The Net# definition of the structure of the neural
network. For more information about the Net# language, see
[Reference Guide](https://azure.microsoft.com/en-us/documentation/articles/machine-learning-azure-ml-netsharp-reference-guide/.md)


##### init_wts_diameter

Sets the initial weights diameter that specifies
the range from which values are drawn for the initial learning weights.
The weights are initialized randomly from within this range. The default
value is 0.1.


##### max_norm

Specifies an upper bound to constrain the norm of the incoming
weight vector at each hidden unit. This can be very important in maxout
neural networks as well as in cases where training produces unbounded weights.


##### acceleration

Specifies the type of hardware acceleration to use.
Possible values are “sse” and “gpu”.
For GPU acceleration, it is recommended to use a miniBatchSize
greater than one.  If you want to use the GPU acceleration, there are
additional manual setup steps are required:

* Download and install NVidia CUDA Toolkit 6.5 ([CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit-65.md)). 

* Download and install NVidia cuDNN v2 Library ([cudnn Library](https://developer.nvidia.com/rdp/cudnn-archive.md)). 

* Find the libs directory of the MicrosoftRML package by calling ``system.file("mxLibs/x64", package = "MicrosoftML")``. 

* Copy cublas64_65.dll, cudart64_65.dll and cusparse64_65.dll from the CUDA Toolkit 6.5 into the libs directory of the MicrosoftML package. 

* Copy cudnn64_65.dll from the cuDNN v2 Library into the libs directory of the MicrosoftML package. 


##### mini_batch_size

Sets the mini-batch size. Recommended values are between
1 and 256. This parameter is only used when the acceleration is GPU. Setting
this parameter to a higher value improves the speed of training, but it might
negatively affect the accuracy. The default value is 1.


##### normalize

Specifies the type of automatic normalization used:

* ``"Warn"``: if normalization is needed, it is performed automatically. This is the default choice. 

* ``"No"``: no normalization is performed. 

* ``"Yes"``: normalization is performed. 

* ``"Auto"``: if normalization is needed, a warning message is displayed, but normalization is not performed. 

Normalization rescales disparate data ranges to a standard scale. Feature
scaling insures the distances between data points are proportional and
enables various optimization methods such as gradient descent to converge
much faster. If normalization is performed, a ``MaxMin`` normalizer is
used. It normalizes values in an interval [a, b] where ``-1 <= a <= 0``
and ``0 <= b <= 1`` and ``b - a = 1``. This normalizer preserves
sparsity by mapping zero to zero.


##### ml_transforms

Specifies a list of MicrosoftML transforms to be
performed on the data before training or *None* if no transforms are
to be performed. See [``featurize_text``](featurize_text.md),
``categorical``,
and ``categorical_hash()``, for transformations that are supported.
These transformations are performed after any specified R transformations.
The default value is *None*.


##### ml_transform_vars

Specifies a character vector of variable names
to be used in ``mlTransforms`` or *None* if none are to be used.
The default value is *None*.


##### row_selection

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


##### transforms

An expression of the form that represents the
first round of variable transformations. As with
all expressions, ``transforms`` (or ``row_selection``) can be defined
outside of the function call using the ``expression()`` function.


##### transform_objects

A named list that contains objects that can be
referenced by ``transforms``, ``transformsFunc``, and
``row_selection``.


##### transform_func

The variable transformation function.


##### transform_vars

A character vector of input data set variables needed for
the transformation function.


##### transform_packages

A character vector specifying additional R packages
(outside of those specified in ``rxGetOption("transformPackages")``) to
be made available and preloaded for use in variable transformation functions.
For exmple, those explicitly defined in  functions via
their ``transforms`` and ``transform_func`` arguments or those defined
implicitly via their ``formula`` or ``row_selection`` arguments.  The
``transform_packages`` argument may also be *None*, indicating that
no packages outside ``rxGetOption("transformPackages")`` are preloaded.


##### transform_envir

A user-defined environment to serve as a parent to all
environments developed internally and used for variable data transformation.
If ``transformEnvir = NULL``, a new “hash” environment with parent
``baseenv()`` is used instead.


##### blocks_per_read

Specifies the number of blocks to read for each chunk
of data read from the data source.


##### report_progress

An integer value that specifies the level of reporting
on the row processing progress:

* ``0``: no progress is reported. 

* ``1``: the number of processed rows is printed and updated. 

* ``2``: rows processed and timings are reported. 

* ``3``: rows processed and all timings are reported. 


##### verbose

An integer value that specifies the amount of output wanted.
If ``0``, no verbose output is printed during calculations. Integer
values from ``1`` to ``4`` provide increasing amounts of information.


##### compute_context

Sets the context in which computations are executed,
specified with a valid ``revo_scale_r``.
Currently local and ``revo_scale_r`` compute contexts
are supported.


##### ensemble

Control parameters for ensembling.


### Returns

item“rxNeuralNet“: an ``rxNeuralNet`` object with the
trained model.  item“NeuralNet“: a learner specification object of
class ``maml`` for the Neural Net trainer.  }


### Note

This algorithm is single-threaded and will not attempt to load the entire dataset into
memory.


### Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


### See also

[``rx_fast_trees``](rx_fast_trees.md),
[``rx_fast_forest``](rx_fast_forest.md),
``rx_fast_linear``,
[``rx_logistic_regression``](rx_logistic_regression.md),
``rx_one_class_svm``,
[``featurize_text``](featurize_text.md),
[``categorical``](categorical.md),
[``categorical_hash``](categorical_hash.md),
[``rx_predict``](rx_predict.md)


### References

[Wikipedia: Artificial neural network](http://en.wikipedia.org/wiki/Artificial_neural_network.md)


### Example



```
# Estimate a binary neural net
rxNeuralNet1 <- rxNeuralNet(isCase ~ age + parity + education + spontaneous + induced,
                  transforms = list(isCase = case == 1),
                  data = infert)

# Score to a data frame
scoreDF <- rxPredict(rxNeuralNet1, data = infert, 
    extraVarsToWrite = "isCase",
    outData = NULL) # return a data frame
    
# Compute and plot the Radio Operator Curve and AUC
roc1 <- rxRoc(actualVarName = "isCase", predVarNames = "Probability", data = scoreDF) 
plot(roc1)
rxAuc(roc1)

#########################################################################
# Regression neural net

# Create an xdf file with the attitude data
myXdf <- tempfile(pattern = "tempAttitude", fileext = ".xdf")
rxDataStep(attitude, myXdf, rowsPerRead = 50, overwrite = TRUE)
myXdfDS <- RxXdfData(file = myXdf)
    
attitudeForm <- rating ~ complaints + privileges + learning + 
    raises + critical + advance

# Estimate a regression neural net 
res2 <- rxNeuralNet(formula = attitudeForm,  data = myXdfDS, 
    type = "regression")
	
# Score to data frame
scoreOut2 <- rxPredict(res2, data = myXdfDS, 
    extraVarsToWrite = "rating")

# Plot the rating versus the score with a regression line
rxLinePlot(rating~Score, type = c("p","r"), data = scoreOut2)

# Clean up   
file.remove(myXdf)    

#############################################################################
# Multi-class neural net
multiNN <- rxNeuralNet(
    formula = Species~Sepal.Length + Sepal.Width + Petal.Length + Petal.Width,
    type = "multiClass", data = iris)
scoreMultiDF <- rxPredict(multiNN, data = iris, 
    extraVarsToWrite = "Species", outData = NULL)    
# Print the first rows of the data frame with scores
head(scoreMultiDF)
# Compute % of incorrect predictions
badPrediction = scoreMultiDF$Species != scoreMultiDF$PredictedLabel
sum(badPrediction)*100/nrow(scoreMultiDF)
# Look at the observations with incorrect predictions
scoreMultiDF[badPrediction,]
```

