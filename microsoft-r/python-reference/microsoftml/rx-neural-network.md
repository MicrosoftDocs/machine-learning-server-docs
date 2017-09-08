--- 
 
# required metadata 
title: "Neural Net" 
description: "Neural networks for regression modeling and for Binary and multi-class classification." 
keywords: "models, classification, regression, neural network, dnn" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/13/2017" 
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

# *microsoftml.rx_neural_network*: Neural Network


**Applies to: SQL Server 2017**

* [optimizers](optimizers.md) 

* [math](math.md) 


## Usage



```
microsoftml.rx_neural_network(formula: str, data: [<class ‘revoscalepy.datasource.RxDataSource.RxDataSource’>, <class ‘pandas.core.frame.DataFrame’>], method: [‘binary’, ‘multiClass’, ‘regression’] = ‘binary’, num_hidden_nodes: int = 100, num_iterations: int = 100, optimizer: [<function adadelta_optimizer at 0x000001FF51091598>, <function sgd_optimizer at 0x000001FF51091400>] = {‘settings’: {}, ‘name’: ‘SgdOptimizer’}, net_definition: str = None, init_wts_diameter: float = 0.1, max_norm: float = 0, acceleration: [<function avx_math at 0x000001FF51091730>, <function clr_math at 0x000001FF510919D8>, <function gpu_math at 0x000001FF51091A60>, <function mkl_math at 0x000001FF51091AE8>, <function sse_math at 0x000001FF51091B70>] = {‘settings’: {}, ‘name’: ‘AvxMath’}, mini_batch_size: int = 1, normalize: [‘No’, ‘Warn’, ‘Auto’, ‘Yes’] = ‘Auto’, ml_transforms: list = None, ml_transform_vars: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment: dict = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, ensemble: dict = None, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
```




## Description

Neural networks for regression modeling and for Binary and
multi-class classification.


## Details

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


## Arguments


### formula

The formula as described in [revoscalepy.rx_formula](../revoscalepy/rx-formula.md).
Interaction terms and `F()` are not currently supported in
[microsoftml](microsoftml-package.md).


### data

A data source object or a character string specifying a
*.xdf* file or a data frame object.


### method

A character string denoting Fast Tree type:

* `"binary"` for the default binary classification neural network. 

* `"multiClass"` for multi-class classification neural network. 

* `"regression"` for a regression neural network. 


### num_hidden_nodes

The default number of hidden nodes in the neural net.
The default value is 100.


### num_iterations

The number of iterations on the full training set.
The default value is 100.


### optimizer

A list specifying either the `sgd` or `adaptive`
optimization algorithm. This list can be created using `sgd()` or
`ada_delta_sgd()`. The default value is `sgd`.


### net_definition

The Net# definition of the structure of the neural
network. For more information about the Net# language, see
[Reference Guide](https://azure.microsoft.com/en-us/documentation/articles/machine-learning-azure-ml-netsharp-reference-guide/)


### init_wts_diameter

Sets the initial weights diameter that specifies
the range from which values are drawn for the initial learning weights.
The weights are initialized randomly from within this range. The default
value is 0.1.


### max_norm

Specifies an upper bound to constrain the norm of the incoming
weight vector at each hidden unit. This can be very important in maxout
neural networks as well as in cases where training produces unbounded weights.


### acceleration

Specifies the type of hardware acceleration to use.
Possible values are “sse_math” and “gpu_math”.
For GPU acceleration, it is recommended to use a miniBatchSize
greater than one.  If you want to use the GPU acceleration, there are
additional manual setup steps are required:

* Download and install NVidia CUDA Toolkit 6.5 ([CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit-65)). 

* Download and install NVidia cuDNN v2 Library ([cudnn Library](https://developer.nvidia.com/rdp/cudnn-archive)). 

* Find the libs directory of the microsoftml package by calling `import microsoftml, os`, `os.path.join(microsoftml.__path__[0], "mxLibs")`. 

* Copy cublas64_65.dll, cudart64_65.dll and cusparse64_65.dll from the CUDA Toolkit 6.5 into the libs directory of the microsoftml package. 

* Copy cudnn64_65.dll from the cuDNN v2 Library into the libs directory of the microsoftml package. 


### mini_batch_size

Sets the mini-batch size. Recommended values are between
1 and 256. This parameter is only used when the acceleration is GPU. Setting
this parameter to a higher value improves the speed of training, but it might
negatively affect the accuracy. The default value is 1.


### normalize

Specifies the type of automatic normalization used:

* `"Warn"`: if normalization is needed, it is performed automatically. This is the default choice. 

* `"No"`: no normalization is performed. 

* `"Yes"`: normalization is performed. 

* `"Auto"`: if normalization is needed, a warning message is displayed, but normalization is not performed. 

Normalization rescales disparate data ranges to a standard scale. Feature
scaling insures the distances between data points are proportional and
enables various optimization methods such as gradient descent to converge
much faster. If normalization is performed, a `MaxMin` normalizer is
used. It normalizes values in an interval [a, b] where `-1 <= a <= 0`
and `0 <= b <= 1` and `b - a = 1`. This normalizer preserves
sparsity by mapping zero to zero.


### ml_transforms

Specifies a list of MicrosoftML transforms to be
performed on the data before training or *None* if no transforms are
to be performed. See [`featurize_text`](featurize-text.md),
[`categorical`](categorical.md),
and [`categorical_hash`](categorical-hash.md), for transformations that are supported.
These transformations are performed after any specified Python transformations.
The default value is *None*.


### ml_transform_vars

Specifies a character vector of variable names
to be used in `ml_transforms` or *None* if none are to be used.
The default value is *None*.


### row_selection

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


### transforms

NOT SUPPORTED. An expression of the form that represents the
first round of variable transformations. As with
all expressions, `transforms` (or `row_selection`) can be defined
outside of the function call using the `expression` function.


### transform_objects

NOT SUPPORTED. A named list that contains objects that can be
referenced by `transforms`, `transform_function`, and
`row_selection`.


### transform_function

The variable transformation function.


### transform_variables

A character vector of input data set variables needed for
the transformation function.


### transform_packages

NOT SUPPORTED. A character vector specifying additional Python packages
(outside of those specified in `RxOptions.get_option("transform_packages")`) to
be made available and preloaded for use in variable transformation functions.
For example, those explicitly defined in [revoscalepy](../revoscalepy/revoscalepy-package.md) functions via
their `transforms` and `transform_function` arguments or those defined
implicitly via their `formula` or `row_selection` arguments.  The
`transform_packages` argument may also be *None*, indicating that
no packages outside `RxOptions.get_option("transform_packages")` are preloaded.


### transform_environment

NOT SUPPORTED. A user-defined environment to serve as a parent to all
environments developed internally and used for variable data transformation.
If `transform_environment = None`, a new “hash” environment with parent
[revoscalepy.baseenv](../revoscalepy/baseenv.md) is used instead.


### blocks_per_read

Specifies the number of blocks to read for each chunk
of data read from the data source.


### report_progress

An integer value that specifies the level of reporting
on the row processing progress:

* `0`: no progress is reported. 

* `1`: the number of processed rows is printed and updated. 

* `2`: rows processed and timings are reported. 

* `3`: rows processed and all timings are reported. 


### verbose

An integer value that specifies the amount of output wanted.
If `0`, no verbose output is printed during calculations. Integer
values from `1` to `4` provide increasing amounts of information.


### compute_context

Sets the context in which computations are executed,
specified with a valid [revoscalepy.RxComputeContext](../revoscalepy/RxComputeContext.md).
Currently local and [revoscalepy.RxInSqlServer](../revoscalepy/RxInSqlServer.md) compute contexts
are supported.


### ensemble

NOT SUPPORTED. Control parameters for ensembling.


## Returns

A [`NeuralNetwork`](learners-object.md) object with the trained model.


## Note

This algorithm is single-threaded and will not attempt to load the entire dataset into
memory.


## See also

[`adadelta_optimizer`](adadelta-optimizer.md),
[`sgd_optimizer`](sgd-optimizer.md),
[`avx_math`](avx-math.md),
[`clr_math`](clr-math.md),
[`gpu_math`](gpu-math.md),
[`mkl_math`](mkl-math.md),
[`sse_math`](sse-math.md),
[`rx_predict`](rx-predict.md).


## References

[Wikipedia: Artificial neural network](http://en.wikipedia.org/wiki/Artificial_neural_network)


## Example



```
'''
Binary Classification.
'''
import numpy
import pandas
from microsoftml import rx_neural_network, rx_predict
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

forest_model = rx_neural_network(
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
Automatically adding a MinMax normalization transform, use 'norm=Warn' or 'norm=No' to turn this behavior off.
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Using: AVX Math

***** Net definition *****
  input Data [5];
  hidden H [100] sigmoid { // Depth 1
    from Data all;
  }
  output Result [1] sigmoid { // Depth 0
    from H all;
  }
***** End net definition *****
Input count: 5
Output count: 1
Output Function: Sigmoid
Loss Function: LogLoss
PreTrainer: NoPreTrainer
___________________________________________________________________
Starting training...
Learning rate: 0.001000
Momentum: 0.000000
InitWtsDiameter: 0.100000
___________________________________________________________________
Initializing 1 Hidden Layers, 701 Weights...
Estimated Pre-training MeanError = 0.739748
Iter:1/100, MeanErr=0.680715(-7.98%), 85.72M WeightUpdates/sec
Iter:2/100, MeanErr=0.643185(-5.51%), 103.31M WeightUpdates/sec
Iter:3/100, MeanErr=0.642608(-0.09%), 25.36M WeightUpdates/sec
Iter:4/100, MeanErr=0.642766(0.02%), 87.21M WeightUpdates/sec
Iter:5/100, MeanErr=0.642487(-0.04%), 108.97M WeightUpdates/sec
Iter:6/100, MeanErr=0.641278(-0.19%), 103.22M WeightUpdates/sec
Iter:7/100, MeanErr=0.643062(0.28%), 103.37M WeightUpdates/sec
Iter:8/100, MeanErr=0.642720(-0.05%), 118.23M WeightUpdates/sec
Iter:9/100, MeanErr=0.642515(-0.03%), 91.84M WeightUpdates/sec
Iter:10/100, MeanErr=0.642644(0.02%), 99.73M WeightUpdates/sec
Iter:11/100, MeanErr=0.642665(0.00%), 118.08M WeightUpdates/sec
Iter:12/100, MeanErr=0.641556(-0.17%), 120.10M WeightUpdates/sec
Iter:13/100, MeanErr=0.642882(0.21%), 105.97M WeightUpdates/sec
Iter:14/100, MeanErr=0.641041(-0.29%), 98.99M WeightUpdates/sec
Iter:15/100, MeanErr=0.642605(0.24%), 101.46M WeightUpdates/sec
Iter:16/100, MeanErr=0.642687(0.01%), 92.29M WeightUpdates/sec
Iter:17/100, MeanErr=0.642604(-0.01%), 105.35M WeightUpdates/sec
Iter:18/100, MeanErr=0.642281(-0.05%), 93.67M WeightUpdates/sec
Iter:19/100, MeanErr=0.642080(-0.03%), 99.15M WeightUpdates/sec
Iter:20/100, MeanErr=0.641956(-0.02%), 99.76M WeightUpdates/sec
Iter:21/100, MeanErr=0.642283(0.05%), 99.82M WeightUpdates/sec
Iter:22/100, MeanErr=0.642473(0.03%), 113.04M WeightUpdates/sec
Iter:23/100, MeanErr=0.642547(0.01%), 108.02M WeightUpdates/sec
Iter:24/100, MeanErr=0.642021(-0.08%), 109.07M WeightUpdates/sec
Iter:25/100, MeanErr=0.642325(0.05%), 102.86M WeightUpdates/sec
Iter:26/100, MeanErr=0.642618(0.05%), 103.58M WeightUpdates/sec
Iter:27/100, MeanErr=0.642307(-0.05%), 106.67M WeightUpdates/sec
Iter:28/100, MeanErr=0.642653(0.05%), 109.71M WeightUpdates/sec
Iter:29/100, MeanErr=0.642507(-0.02%), 97.34M WeightUpdates/sec
Iter:30/100, MeanErr=0.642628(0.02%), 98.69M WeightUpdates/sec
Iter:31/100, MeanErr=0.642198(-0.07%), 99.65M WeightUpdates/sec
Iter:32/100, MeanErr=0.642381(0.03%), 100.41M WeightUpdates/sec
Iter:33/100, MeanErr=0.642453(0.01%), 105.07M WeightUpdates/sec
Iter:34/100, MeanErr=0.642554(0.02%), 110.90M WeightUpdates/sec
Iter:35/100, MeanErr=0.642468(-0.01%), 103.70M WeightUpdates/sec
Iter:36/100, MeanErr=0.642237(-0.04%), 120.02M WeightUpdates/sec
Iter:37/100, MeanErr=0.642466(0.04%), 92.89M WeightUpdates/sec
Iter:38/100, MeanErr=0.642517(0.01%), 95.52M WeightUpdates/sec
Iter:39/100, MeanErr=0.642540(0.00%), 107.08M WeightUpdates/sec
Iter:40/100, MeanErr=0.641731(-0.13%), 100.74M WeightUpdates/sec
Iter:41/100, MeanErr=0.641024(-0.11%), 94.74M WeightUpdates/sec
Iter:42/100, MeanErr=0.642457(0.22%), 22.25M WeightUpdates/sec
Iter:43/100, MeanErr=0.642436(0.00%), 85.13M WeightUpdates/sec
Iter:44/100, MeanErr=0.642207(-0.04%), 105.16M WeightUpdates/sec
Iter:45/100, MeanErr=0.642394(0.03%), 108.08M WeightUpdates/sec
Iter:46/100, MeanErr=0.642128(-0.04%), 106.19M WeightUpdates/sec
Iter:47/100, MeanErr=0.642289(0.03%), 103.13M WeightUpdates/sec
Iter:48/100, MeanErr=0.642427(0.02%), 102.18M WeightUpdates/sec
Iter:49/100, MeanErr=0.642309(-0.02%), 89.54M WeightUpdates/sec
Iter:50/100, MeanErr=0.642067(-0.04%), 90.65M WeightUpdates/sec
Iter:51/100, MeanErr=0.641725(-0.05%), 99.40M WeightUpdates/sec
Iter:52/100, MeanErr=0.642058(0.05%), 110.73M WeightUpdates/sec
Iter:53/100, MeanErr=0.640893(-0.18%), 108.38M WeightUpdates/sec
Iter:54/100, MeanErr=0.642475(0.25%), 69.90M WeightUpdates/sec
Iter:55/100, MeanErr=0.642070(-0.06%), 96.89M WeightUpdates/sec
Iter:56/100, MeanErr=0.642083(0.00%), 109.74M WeightUpdates/sec
Iter:57/100, MeanErr=0.641880(-0.03%), 104.42M WeightUpdates/sec
Iter:58/100, MeanErr=0.642357(0.07%), 110.90M WeightUpdates/sec
Iter:59/100, MeanErr=0.642134(-0.03%), 91.05M WeightUpdates/sec
Iter:60/100, MeanErr=0.642071(-0.01%), 95.52M WeightUpdates/sec
Iter:61/100, MeanErr=0.642211(0.02%), 107.56M WeightUpdates/sec
Iter:62/100, MeanErr=0.641421(-0.12%), 98.20M WeightUpdates/sec
Iter:63/100, MeanErr=0.642326(0.14%), 102.59M WeightUpdates/sec
Iter:64/100, MeanErr=0.642160(-0.03%), 104.03M WeightUpdates/sec
Iter:65/100, MeanErr=0.642235(0.01%), 106.28M WeightUpdates/sec
Iter:66/100, MeanErr=0.642253(0.00%), 112.05M WeightUpdates/sec
Iter:67/100, MeanErr=0.641868(-0.06%), 76.20M WeightUpdates/sec
Iter:68/100, MeanErr=0.642300(0.07%), 105.63M WeightUpdates/sec
Iter:69/100, MeanErr=0.642220(-0.01%), 111.94M WeightUpdates/sec
Iter:70/100, MeanErr=0.642286(0.01%), 88.52M WeightUpdates/sec
Iter:71/100, MeanErr=0.641457(-0.13%), 88.68M WeightUpdates/sec
Iter:72/100, MeanErr=0.642192(0.11%), 101.92M WeightUpdates/sec
Iter:73/100, MeanErr=0.642047(-0.02%), 94.61M WeightUpdates/sec
Iter:74/100, MeanErr=0.642098(0.01%), 112.72M WeightUpdates/sec
Iter:75/100, MeanErr=0.641974(-0.02%), 106.06M WeightUpdates/sec
Iter:76/100, MeanErr=0.641762(-0.03%), 108.35M WeightUpdates/sec
Iter:77/100, MeanErr=0.642014(0.04%), 118.86M WeightUpdates/sec
Iter:78/100, MeanErr=0.642152(0.02%), 117.77M WeightUpdates/sec
Iter:79/100, MeanErr=0.642058(-0.01%), 120.67M WeightUpdates/sec
Iter:80/100, MeanErr=0.641862(-0.03%), 73.77M WeightUpdates/sec
Iter:81/100, MeanErr=0.642023(0.03%), 87.14M WeightUpdates/sec
Iter:82/100, MeanErr=0.642192(0.03%), 109.68M WeightUpdates/sec
Iter:83/100, MeanErr=0.641538(-0.10%), 108.61M WeightUpdates/sec
Iter:84/100, MeanErr=0.641693(0.02%), 68.01M WeightUpdates/sec
Iter:85/100, MeanErr=0.642034(0.05%), 103.10M WeightUpdates/sec
Iter:86/100, MeanErr=0.641996(-0.01%), 110.35M WeightUpdates/sec
Iter:87/100, MeanErr=0.641801(-0.03%), 104.82M WeightUpdates/sec
Iter:88/100, MeanErr=0.641854(0.01%), 102.21M WeightUpdates/sec
Iter:89/100, MeanErr=0.641988(0.02%), 100.86M WeightUpdates/sec
Iter:90/100, MeanErr=0.641880(-0.02%), 106.92M WeightUpdates/sec
Iter:91/100, MeanErr=0.641979(0.02%), 114.34M WeightUpdates/sec
Iter:92/100, MeanErr=0.642056(0.01%), 118.78M WeightUpdates/sec
Iter:93/100, MeanErr=0.640367(-0.26%), 81.06M WeightUpdates/sec
Iter:94/100, MeanErr=0.641212(0.13%), 111.38M WeightUpdates/sec
Iter:95/100, MeanErr=0.641570(0.06%), 123.08M WeightUpdates/sec
Iter:96/100, MeanErr=0.641613(0.01%), 124.10M WeightUpdates/sec
Iter:97/100, MeanErr=0.641926(0.05%), 97.74M WeightUpdates/sec
Iter:98/100, MeanErr=0.641783(-0.02%), 91.35M WeightUpdates/sec
Iter:99/100, MeanErr=0.642002(0.03%), 106.54M WeightUpdates/sec
Iter:100/100, MeanErr=0.641913(-0.01%), 108.68M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 0.639161
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2939539
Elapsed time: 00:00:00.0212031
Beginning processing data.
Rows Read: 62, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0946314
Finished writing 62 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre888068.xdf File will be overwritten if it exists.

Rows Processed: 5 
  isCase PredictedLabel     Score  Probability
0  False          False -0.680194     0.336218
1  False          False -0.665769     0.339444
2   True          False -0.673094     0.337804
3   True          False -0.664929     0.339633
4  False          False -0.682227     0.335764
```



## Example



```
'''
MultiClass Classification.
'''
import numpy
import pandas
from microsoftml import rx_neural_network, rx_predict
from revoscalepy.etl.RxDataStep import rx_data_step
from microsoftml.datasets.datasets import iris

import sklearn
if sklearn.__version__ < "0.18":
    from sklearn.cross_validation import train_test_split
else:
    from sklearn.model_selection import train_test_split

irisdf = iris.as_df()
irisdf["Species"] = irisdf["Species"].astype("category")
data_train, data_test, y_train, y_test = train_test_split(irisdf, irisdf.Species)

model = rx_neural_network(
    formula="  Species ~ Sepal_Length + Sepal_Width + Petal_Length + Petal_Width ",
    method="multiClass",
    data=data_train)
    
# RuntimeError: The type (RxTextData) for file is not supported.
score_ds = rx_predict(model, data=data_test,
                     extra_vars_to_write=["Species", "Score"])
                     
# Print the first five rows
print(rx_data_step(score_ds, number_rows_read=5))
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
Using: AVX Math

***** Net definition *****
  input Data [4];
  hidden H [100] sigmoid { // Depth 1
    from Data all;
  }
  output Result [3] softmax { // Depth 0
    from H all;
  }
***** End net definition *****
Input count: 4
Output count: 3
Output Function: SoftMax
Loss Function: LogLoss
PreTrainer: NoPreTrainer
___________________________________________________________________
Starting training...
Learning rate: 0.001000
Momentum: 0.000000
InitWtsDiameter: 0.100000
___________________________________________________________________
Initializing 1 Hidden Layers, 803 Weights...
Estimated Pre-training MeanError = 1.915342
Iter:1/100, MeanErr=1.917762(0.13%), 62.22M WeightUpdates/sec
Iter:2/100, MeanErr=1.912973(-0.25%), 44.83M WeightUpdates/sec
Iter:3/100, MeanErr=1.913202(0.01%), 30.17M WeightUpdates/sec
Iter:4/100, MeanErr=1.911367(-0.10%), 58.00M WeightUpdates/sec
Iter:5/100, MeanErr=1.911984(0.03%), 67.48M WeightUpdates/sec
Iter:6/100, MeanErr=1.913254(0.07%), 64.48M WeightUpdates/sec
Iter:7/100, MeanErr=1.911650(-0.08%), 65.43M WeightUpdates/sec
Iter:8/100, MeanErr=1.912787(0.06%), 64.73M WeightUpdates/sec
Iter:9/100, MeanErr=1.911616(-0.06%), 68.54M WeightUpdates/sec
Iter:10/100, MeanErr=1.912712(0.06%), 78.77M WeightUpdates/sec
Iter:11/100, MeanErr=1.912189(-0.03%), 67.13M WeightUpdates/sec
Iter:12/100, MeanErr=1.911116(-0.06%), 37.22M WeightUpdates/sec
Iter:13/100, MeanErr=1.910649(-0.02%), 35.02M WeightUpdates/sec
Iter:14/100, MeanErr=1.912634(0.10%), 74.76M WeightUpdates/sec
Iter:15/100, MeanErr=1.908512(-0.22%), 90.11M WeightUpdates/sec
Iter:16/100, MeanErr=1.912267(0.20%), 91.65M WeightUpdates/sec
Iter:17/100, MeanErr=1.911856(-0.02%), 93.77M WeightUpdates/sec
Iter:18/100, MeanErr=1.912003(0.01%), 94.64M WeightUpdates/sec
Iter:19/100, MeanErr=1.911334(-0.03%), 20.57M WeightUpdates/sec
Iter:20/100, MeanErr=1.910927(-0.02%), 78.17M WeightUpdates/sec
Iter:21/100, MeanErr=1.911978(0.06%), 90.80M WeightUpdates/sec
Iter:22/100, MeanErr=1.912381(0.02%), 92.75M WeightUpdates/sec
Iter:23/100, MeanErr=1.908411(-0.21%), 41.50M WeightUpdates/sec
Iter:24/100, MeanErr=1.911969(0.19%), 60.42M WeightUpdates/sec
Iter:25/100, MeanErr=1.911559(-0.02%), 62.56M WeightUpdates/sec
Iter:26/100, MeanErr=1.911120(-0.02%), 66.82M WeightUpdates/sec
Iter:27/100, MeanErr=1.910642(-0.03%), 90.50M WeightUpdates/sec
Iter:28/100, MeanErr=1.910884(0.01%), 89.62M WeightUpdates/sec
Iter:29/100, MeanErr=1.910213(-0.04%), 99.08M WeightUpdates/sec
Iter:30/100, MeanErr=1.908914(-0.07%), 49.07M WeightUpdates/sec
Iter:31/100, MeanErr=1.911125(0.12%), 91.58M WeightUpdates/sec
Iter:32/100, MeanErr=1.911087(0.00%), 94.53M WeightUpdates/sec
Iter:33/100, MeanErr=1.909120(-0.10%), 97.79M WeightUpdates/sec
Iter:34/100, MeanErr=1.909739(0.03%), 95.22M WeightUpdates/sec
Iter:35/100, MeanErr=1.909731(0.00%), 76.40M WeightUpdates/sec
Iter:36/100, MeanErr=1.906712(-0.16%), 67.86M WeightUpdates/sec
Iter:37/100, MeanErr=1.910956(0.22%), 82.68M WeightUpdates/sec
Iter:38/100, MeanErr=1.908970(-0.10%), 92.54M WeightUpdates/sec
Iter:39/100, MeanErr=1.910514(0.08%), 68.49M WeightUpdates/sec
Iter:40/100, MeanErr=1.907988(-0.13%), 64.80M WeightUpdates/sec
Iter:41/100, MeanErr=1.911103(0.16%), 20.61M WeightUpdates/sec
Iter:42/100, MeanErr=1.908498(-0.14%), 72.20M WeightUpdates/sec
Iter:43/100, MeanErr=1.910002(0.08%), 73.75M WeightUpdates/sec
Iter:44/100, MeanErr=1.910360(0.02%), 78.37M WeightUpdates/sec
Iter:45/100, MeanErr=1.909436(-0.05%), 92.37M WeightUpdates/sec
Iter:46/100, MeanErr=1.908876(-0.03%), 58.71M WeightUpdates/sec
Iter:47/100, MeanErr=1.907562(-0.07%), 83.88M WeightUpdates/sec
Iter:48/100, MeanErr=1.909934(0.12%), 85.07M WeightUpdates/sec
Iter:49/100, MeanErr=1.908564(-0.07%), 82.43M WeightUpdates/sec
Iter:50/100, MeanErr=1.909362(0.04%), 73.33M WeightUpdates/sec
Iter:51/100, MeanErr=1.907747(-0.08%), 94.56M WeightUpdates/sec
Iter:52/100, MeanErr=1.908887(0.06%), 43.56M WeightUpdates/sec
Iter:53/100, MeanErr=1.907687(-0.06%), 75.37M WeightUpdates/sec
Iter:54/100, MeanErr=1.907041(-0.03%), 97.87M WeightUpdates/sec
Iter:55/100, MeanErr=1.907740(0.04%), 95.52M WeightUpdates/sec
Iter:56/100, MeanErr=1.908274(0.03%), 93.99M WeightUpdates/sec
Iter:57/100, MeanErr=1.906505(-0.09%), 68.70M WeightUpdates/sec
Iter:58/100, MeanErr=1.909011(0.13%), 69.71M WeightUpdates/sec
Iter:59/100, MeanErr=1.908838(-0.01%), 55.82M WeightUpdates/sec
Iter:60/100, MeanErr=1.907452(-0.07%), 75.70M WeightUpdates/sec
Iter:61/100, MeanErr=1.908214(0.04%), 88.14M WeightUpdates/sec
Iter:62/100, MeanErr=1.908444(0.01%), 62.45M WeightUpdates/sec
Iter:63/100, MeanErr=1.908120(-0.02%), 75.81M WeightUpdates/sec
Iter:64/100, MeanErr=1.907312(-0.04%), 46.11M WeightUpdates/sec
Iter:65/100, MeanErr=1.907911(0.03%), 78.99M WeightUpdates/sec
Iter:66/100, MeanErr=1.907659(-0.01%), 72.13M WeightUpdates/sec
Iter:67/100, MeanErr=1.908359(0.04%), 52.48M WeightUpdates/sec
Iter:68/100, MeanErr=1.908218(-0.01%), 81.10M WeightUpdates/sec
Iter:69/100, MeanErr=1.907473(-0.04%), 89.84M WeightUpdates/sec
Iter:70/100, MeanErr=1.907488(0.00%), 69.06M WeightUpdates/sec
Iter:71/100, MeanErr=1.906880(-0.03%), 70.12M WeightUpdates/sec
Iter:72/100, MeanErr=1.907083(0.01%), 88.94M WeightUpdates/sec
Iter:73/100, MeanErr=1.905112(-0.10%), 88.97M WeightUpdates/sec
Iter:74/100, MeanErr=1.902527(-0.14%), 76.64M WeightUpdates/sec
Iter:75/100, MeanErr=1.907464(0.26%), 42.82M WeightUpdates/sec
Iter:76/100, MeanErr=1.907619(0.01%), 68.98M WeightUpdates/sec
Iter:77/100, MeanErr=1.906105(-0.08%), 82.73M WeightUpdates/sec
Iter:78/100, MeanErr=1.906729(0.03%), 89.29M WeightUpdates/sec
Iter:79/100, MeanErr=1.905711(-0.05%), 70.14M WeightUpdates/sec
Iter:80/100, MeanErr=1.904296(-0.07%), 84.81M WeightUpdates/sec
Iter:81/100, MeanErr=1.905733(0.08%), 98.97M WeightUpdates/sec
Iter:82/100, MeanErr=1.906601(0.05%), 86.93M WeightUpdates/sec
Iter:83/100, MeanErr=1.906297(-0.02%), 70.75M WeightUpdates/sec
Iter:84/100, MeanErr=1.904722(-0.08%), 78.07M WeightUpdates/sec
Iter:85/100, MeanErr=1.903722(-0.05%), 98.37M WeightUpdates/sec
Iter:86/100, MeanErr=1.905725(0.11%), 49.45M WeightUpdates/sec
Iter:87/100, MeanErr=1.905321(-0.02%), 89.03M WeightUpdates/sec
Iter:88/100, MeanErr=1.905148(-0.01%), 86.72M WeightUpdates/sec
Iter:89/100, MeanErr=1.904064(-0.06%), 69.69M WeightUpdates/sec
Iter:90/100, MeanErr=1.905460(0.07%), 74.73M WeightUpdates/sec
Iter:91/100, MeanErr=1.905328(-0.01%), 88.46M WeightUpdates/sec
Iter:92/100, MeanErr=1.900499(-0.25%), 94.85M WeightUpdates/sec
Iter:93/100, MeanErr=1.905201(0.25%), 96.52M WeightUpdates/sec
Iter:94/100, MeanErr=1.905066(-0.01%), 95.44M WeightUpdates/sec
Iter:95/100, MeanErr=1.901161(-0.20%), 92.26M WeightUpdates/sec
Iter:96/100, MeanErr=1.904924(0.20%), 92.61M WeightUpdates/sec
Iter:97/100, MeanErr=1.904770(-0.01%), 16.30M WeightUpdates/sec
Iter:98/100, MeanErr=1.904395(-0.02%), 83.57M WeightUpdates/sec
Iter:99/100, MeanErr=1.904573(0.01%), 95.18M WeightUpdates/sec
Iter:100/100, MeanErr=1.903881(-0.04%), 76.45M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 1.892008
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.3394118
Elapsed time: 00:00:00.0291931
Beginning processing data.
Rows Read: 38, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0908476
Finished writing 38 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre888079.xdf File will be overwritten if it exists.

Rows Processed: 5 
      Species   Score.0   Score.1   Score.2
0  versicolor  0.283913  0.355998  0.360089
1   virginica  0.282003  0.356273  0.361724
2  versicolor  0.283512  0.356116  0.360372
3      setosa  0.289524  0.354882  0.355594
4   virginica  0.280835  0.356423  0.362742
```



## Example



```
'''
Regression.
'''
import numpy
import pandas
from microsoftml import rx_neural_network, rx_predict
from revoscalepy.etl.RxDataStep import rx_data_step
from microsoftml.datasets.datasets import attitude

import sklearn
if sklearn.__version__ < "0.18":
    from sklearn.cross_validation import train_test_split
else:
    from sklearn.model_selection import train_test_split

attitudedf = attitude.as_df()
data_train, data_test = train_test_split(attitudedf)

model = rx_neural_network(
    formula="rating ~ complaints + privileges + learning + raises + critical + advance",
    method="regression",
    data=data_train)
    
# RuntimeError: The type (RxTextData) for file is not supported.
score_ds = rx_predict(model, data=data_test,
                     extra_vars_to_write=["rating"])
                     
# Print the first five rows
print(rx_data_step(score_ds, number_rows_read=5))
```


Output:



```
Automatically adding a MinMax normalization transform, use 'norm=Warn' or 'norm=No' to turn this behavior off.
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Using: AVX Math

***** Net definition *****
  input Data [6];
  hidden H [100] sigmoid { // Depth 1
    from Data all;
  }
  output Result [1] linear { // Depth 0
    from H all;
  }
***** End net definition *****
Input count: 6
Output count: 1
Output Function: Linear
Loss Function: SquaredLoss
PreTrainer: NoPreTrainer
___________________________________________________________________
Starting training...
Learning rate: 0.001000
Momentum: 0.000000
InitWtsDiameter: 0.100000
___________________________________________________________________
Initializing 1 Hidden Layers, 801 Weights...
Estimated Pre-training MeanError = 4443.616849
Iter:1/100, MeanErr=1688.696104(-62.00%), 13.66M WeightUpdates/sec
Iter:2/100, MeanErr=147.375974(-91.27%), 17.05M WeightUpdates/sec
Iter:3/100, MeanErr=109.398474(-25.77%), 17.30M WeightUpdates/sec
Iter:4/100, MeanErr=125.152006(14.40%), 17.64M WeightUpdates/sec
Iter:5/100, MeanErr=112.140097(-10.40%), 17.21M WeightUpdates/sec
Iter:6/100, MeanErr=117.170470(4.49%), 10.56M WeightUpdates/sec
Iter:7/100, MeanErr=119.091771(1.64%), 14.40M WeightUpdates/sec
Iter:8/100, MeanErr=112.303689(-5.70%), 16.45M WeightUpdates/sec
Iter:9/100, MeanErr=119.303997(6.23%), 20.21M WeightUpdates/sec
Iter:10/100, MeanErr=117.800045(-1.26%), 24.54M WeightUpdates/sec
Iter:11/100, MeanErr=114.633063(-2.69%), 22.90M WeightUpdates/sec
Iter:12/100, MeanErr=107.298065(-6.40%), 17.14M WeightUpdates/sec
Iter:13/100, MeanErr=117.806300(9.79%), 9.00M WeightUpdates/sec
Iter:14/100, MeanErr=112.500259(-4.50%), 10.74M WeightUpdates/sec
Iter:15/100, MeanErr=115.247304(2.44%), 20.89M WeightUpdates/sec
Iter:16/100, MeanErr=110.666696(-3.97%), 24.20M WeightUpdates/sec
Iter:17/100, MeanErr=110.244483(-0.38%), 22.92M WeightUpdates/sec
Iter:18/100, MeanErr=109.962128(-0.26%), 23.51M WeightUpdates/sec
Iter:19/100, MeanErr=105.889199(-3.70%), 24.11M WeightUpdates/sec
Iter:20/100, MeanErr=106.685532(0.75%), 17.90M WeightUpdates/sec
Iter:21/100, MeanErr=101.127247(-5.21%), 17.41M WeightUpdates/sec
Iter:22/100, MeanErr=111.209150(9.97%), 13.55M WeightUpdates/sec
Iter:23/100, MeanErr=109.409752(-1.62%), 18.80M WeightUpdates/sec
Iter:24/100, MeanErr=106.943274(-2.25%), 20.51M WeightUpdates/sec
Iter:25/100, MeanErr=103.336468(-3.37%), 23.76M WeightUpdates/sec
Iter:26/100, MeanErr=105.123888(1.73%), 22.60M WeightUpdates/sec
Iter:27/100, MeanErr=102.997358(-2.02%), 23.79M WeightUpdates/sec
Iter:28/100, MeanErr=103.606303(0.59%), 21.70M WeightUpdates/sec
Iter:29/100, MeanErr=100.218537(-3.27%), 23.27M WeightUpdates/sec
Iter:30/100, MeanErr=106.154549(5.92%), 22.91M WeightUpdates/sec
Iter:31/100, MeanErr=101.259744(-4.61%), 23.20M WeightUpdates/sec
Iter:32/100, MeanErr=101.488262(0.23%), 22.72M WeightUpdates/sec
Iter:33/100, MeanErr=101.861005(0.37%), 25.51M WeightUpdates/sec
Iter:34/100, MeanErr=101.152038(-0.70%), 23.68M WeightUpdates/sec
Iter:35/100, MeanErr=97.001608(-4.10%), 22.59M WeightUpdates/sec
Iter:36/100, MeanErr=100.394103(3.50%), 22.98M WeightUpdates/sec
Iter:37/100, MeanErr=96.745522(-3.63%), 25.26M WeightUpdates/sec
Iter:38/100, MeanErr=96.582292(-0.17%), 18.46M WeightUpdates/sec
Iter:39/100, MeanErr=96.724199(0.15%), 17.46M WeightUpdates/sec
Iter:40/100, MeanErr=94.389822(-2.41%), 21.52M WeightUpdates/sec
Iter:41/100, MeanErr=97.756229(3.57%), 24.70M WeightUpdates/sec
Iter:42/100, MeanErr=97.226747(-0.54%), 22.51M WeightUpdates/sec
Iter:43/100, MeanErr=90.191543(-7.24%), 23.70M WeightUpdates/sec
Iter:44/100, MeanErr=89.081925(-1.23%), 22.04M WeightUpdates/sec
Iter:45/100, MeanErr=91.885110(3.15%), 22.14M WeightUpdates/sec
Iter:46/100, MeanErr=95.188611(3.60%), 20.89M WeightUpdates/sec
Iter:47/100, MeanErr=93.794048(-1.47%), 19.38M WeightUpdates/sec
Iter:48/100, MeanErr=88.462710(-5.68%), 22.98M WeightUpdates/sec
Iter:49/100, MeanErr=90.648439(2.47%), 2.91M WeightUpdates/sec
Iter:50/100, MeanErr=92.857687(2.44%), 16.22M WeightUpdates/sec
Iter:51/100, MeanErr=89.711477(-3.39%), 16.59M WeightUpdates/sec
Iter:52/100, MeanErr=88.345516(-1.52%), 20.13M WeightUpdates/sec
Iter:53/100, MeanErr=88.980661(0.72%), 16.06M WeightUpdates/sec
Iter:54/100, MeanErr=88.454659(-0.59%), 16.91M WeightUpdates/sec
Iter:55/100, MeanErr=88.149967(-0.34%), 22.58M WeightUpdates/sec
Iter:56/100, MeanErr=88.220267(0.08%), 23.62M WeightUpdates/sec
Iter:57/100, MeanErr=89.730912(1.71%), 22.47M WeightUpdates/sec
Iter:58/100, MeanErr=88.037392(-1.89%), 23.65M WeightUpdates/sec
Iter:59/100, MeanErr=87.775952(-0.30%), 25.27M WeightUpdates/sec
Iter:60/100, MeanErr=87.235712(-0.62%), 21.31M WeightUpdates/sec
Iter:61/100, MeanErr=83.934821(-3.78%), 11.07M WeightUpdates/sec
Iter:62/100, MeanErr=82.705359(-1.46%), 20.78M WeightUpdates/sec
Iter:63/100, MeanErr=85.431000(3.30%), 23.03M WeightUpdates/sec
Iter:64/100, MeanErr=83.121299(-2.70%), 22.09M WeightUpdates/sec
Iter:65/100, MeanErr=85.250332(2.56%), 24.30M WeightUpdates/sec
Iter:66/100, MeanErr=83.596439(-1.94%), 23.89M WeightUpdates/sec
Iter:67/100, MeanErr=79.363347(-5.06%), 21.20M WeightUpdates/sec
Iter:68/100, MeanErr=81.058028(2.14%), 15.77M WeightUpdates/sec
Iter:69/100, MeanErr=81.570522(0.63%), 19.50M WeightUpdates/sec
Iter:70/100, MeanErr=81.699007(0.16%), 24.31M WeightUpdates/sec
Iter:71/100, MeanErr=77.937687(-4.60%), 23.66M WeightUpdates/sec
Iter:72/100, MeanErr=71.999748(-7.62%), 23.84M WeightUpdates/sec
Iter:73/100, MeanErr=81.970346(13.85%), 13.94M WeightUpdates/sec
Iter:74/100, MeanErr=78.006047(-4.84%), 22.08M WeightUpdates/sec
Iter:75/100, MeanErr=78.293008(0.37%), 22.90M WeightUpdates/sec
Iter:76/100, MeanErr=78.689621(0.51%), 20.78M WeightUpdates/sec
Iter:77/100, MeanErr=76.111351(-3.28%), 19.04M WeightUpdates/sec
Iter:78/100, MeanErr=77.147839(1.36%), 22.77M WeightUpdates/sec
Iter:79/100, MeanErr=75.467489(-2.18%), 24.38M WeightUpdates/sec
Iter:80/100, MeanErr=76.126306(0.87%), 22.61M WeightUpdates/sec
Iter:81/100, MeanErr=75.123851(-1.32%), 26.22M WeightUpdates/sec
Iter:82/100, MeanErr=73.900211(-1.63%), 22.68M WeightUpdates/sec
Iter:83/100, MeanErr=70.984931(-3.94%), 24.25M WeightUpdates/sec
Iter:84/100, MeanErr=71.217664(0.33%), 22.77M WeightUpdates/sec
Iter:85/100, MeanErr=76.182259(6.97%), 9.72M WeightUpdates/sec
Iter:86/100, MeanErr=73.216440(-3.89%), 16.21M WeightUpdates/sec
Iter:87/100, MeanErr=71.776047(-1.97%), 22.34M WeightUpdates/sec
Iter:88/100, MeanErr=71.818941(0.06%), 22.82M WeightUpdates/sec
Iter:89/100, MeanErr=71.168560(-0.91%), 22.22M WeightUpdates/sec
Iter:90/100, MeanErr=70.015311(-1.62%), 24.90M WeightUpdates/sec
Iter:91/100, MeanErr=71.474003(2.08%), 25.21M WeightUpdates/sec
Iter:92/100, MeanErr=69.511116(-2.75%), 23.79M WeightUpdates/sec
Iter:93/100, MeanErr=69.015008(-0.71%), 23.32M WeightUpdates/sec
Iter:94/100, MeanErr=68.029522(-1.43%), 23.69M WeightUpdates/sec
Iter:95/100, MeanErr=69.003171(1.43%), 25.08M WeightUpdates/sec
Iter:96/100, MeanErr=67.878339(-1.63%), 22.27M WeightUpdates/sec
Iter:97/100, MeanErr=67.891143(0.02%), 11.07M WeightUpdates/sec
Iter:98/100, MeanErr=66.366391(-2.25%), 22.39M WeightUpdates/sec
Iter:99/100, MeanErr=65.005477(-2.05%), 23.70M WeightUpdates/sec
Iter:100/100, MeanErr=67.768632(4.25%), 24.63M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 59.453157
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2410078
Elapsed time: 00:00:00.0197780
Beginning processing data.
Rows Read: 8, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0962043
Finished writing 8 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre8880810.xdf File will be overwritten if it exists.

Rows Processed: 5 
   rating      Score
0    67.0  61.620380
1    81.0  68.599335
2    43.0  62.914749
3    74.0  70.728958
4    50.0  63.510494
```



## optimizers

* [*microsoftml.adadelta_optimizer*: Adaptive learing rate method](adadelta-optimizer.md) 

* [*microsoftml.sgd_optimizer*: Stochastic gradient descent](sgd-optimizer.md) 


## math

* [*microsoftml.avx_math*: Acceleration with AVX instructions](avx-math.md) 

* [*microsoftml.clr_math*: Acceleration with .NET math](clr-math.md) 

* [*microsoftml.gpu_math*: Acceleration with NVidia CUDA](gpu-math.md) 

* [*microsoftml.mkl_math*: Acceleration with Intel MKL](mkl-math.md) 

* [*microsoftml.sse_math*: Acceleration with SSE instructions](sse-math.md) 
