--- 
 
# required metadata 
title: "Neural Net" 
description: "Neural networks for regression modeling and for Binary and" 
keywords: "models, classification, regression, neural network, dnn" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
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

## ``rx_neural_network``: Neural Network


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.rx_neural_network(formula: str, data: [<class ‘revoscalepy.datasource.RxDataSource.RxDataSource’>, <class ‘pandas.core.frame.DataFrame’>], method: [‘binary’, ‘multiClass’, ‘regression’] = ‘binary’, num_hidden_nodes: int = 100, num_iterations: int = 100, optimizer: [<function adadelta_optimizer at 0x000001F5FD74F400>, <function sgd_optimizer at 0x000001F5FD74F268>] = {‘name’: ‘SgdOptimizer’, ‘settings’: {}}, net_definition: str = None, init_wts_diameter: float = 0.1, max_norm: float = 0, acceleration: [<function avx_math at 0x000001F5FD74F598>, <function clr_math at 0x000001F5FD74F840>, <function gpu_math at 0x000001F5FD74F8C8>, <function mkl_math at 0x000001F5FD74F950>, <function sse_math at 0x000001F5FD74F9D8>] = {‘name’: ‘AvxMath’, ‘settings’: {}}, mini_batch_size: int = 1, normalize: [‘No’, ‘Warn’, ‘Auto’, ‘Yes’] = ‘Auto’, ml_transforms: list = None, ml_transform_vars: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment: dict = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, ensemble: dict = None, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
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

The formula as described in ``rx_formula``.
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
Possible values are “sse_math” and “gpu_math”.
For GPU acceleration, it is recommended to use a miniBatchSize
greater than one.  If you want to use the GPU acceleration, there are
additional manual setup steps are required:

* Download and install NVidia CUDA Toolkit 6.5 ([CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit-65.md)). 

* Download and install NVidia cuDNN v2 Library ([cudnn Library](https://developer.nvidia.com/rdp/cudnn-archive.md)). 

* Find the libs directory of the microsoftml package by calling ``import microsoftml, os``, ``os.path.join(microsoftml.__path__[0], "mxLibs")``. 

* Copy cublas64_65.dll, cudart64_65.dll and cusparse64_65.dll from the CUDA Toolkit 6.5 into the libs directory of the microsoftml package. 

* Copy cudnn64_65.dll from the cuDNN v2 Library into the libs directory of the microsoftml package. 


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

Specifies a list of microsoftml transforms to be
performed on the data before training or *None* if no transforms are
to be performed. See [``featurize_text``](featurize_text.md),
[``categorical``](categorical.md),
and [``categorical_hash``](categorical_hash.md), for transformations that are supported.
These transformations are performed after any specified Python transformations.
The default value is *None*.


##### ml_transform_vars

Specifies a character vector of variable names
to be used in ``ml_transforms`` or *None* if none are to be used.
The default value is *None*.


##### row_selection

NOT SUPPORTED. Specifies the rows (observations) from the data set that
are to be used by the model with the name of a logical variable from the
data set (in quotes) or with a logical expression using variables in the
data set. For example, ``row_selection = "old"`` will only use
observations in which the value of the variable ``old`` is ``True``.
``row_selection = (age > 20) & (age < 65) & (log(income) > 10)`` only uses
observations in which the value of the ``age`` variable is between
20 and 65 and the value of the ``log`` of the ``income`` variable is
greater than 10. The row selection is performed after processing any data
transformations (see the arguments ``transforms`` or
``transform_function``). As with all expressions, ``row_selection`` can be
defined outside of the function call using the ``expression``
function.


##### transforms

NOT SUPPORTED. An expression of the form that represents the
first round of variable transformations. As with
all expressions, ``transforms`` (or ``row_selection``) can be defined
outside of the function call using the ``expression`` function.


##### transform_objects

NOT SUPPORTED. A named list that contains objects that can be
referenced by ``transforms``, ``transform_function``, and
``row_selection``.


##### transform_function

The variable transformation function.


##### transform_variables

A character vector of input data set variables needed for
the transformation function.


##### transform_packages

NOT SUPPORTED. A character vector specifying additional Python packages
(outside of those specified in ``RxOptions.get_option("transform_packages")``) to
be made available and preloaded for use in variable transformation functions.
For exmple, those explicitly defined in  functions via
their ``transforms`` and ``transform_function`` arguments or those defined
implicitly via their ``formula`` or ``row_selection`` arguments.  The
``transform_packages`` argument may also be *None*, indicating that
no packages outside ``RxOptions.get_option("transform_packages")`` are preloaded.


##### transform_environment

NOT SUPPORTED. A user-defined environment to serve as a parent to all
environments developed internally and used for variable data transformation.
If ``transform_environment = None``, a new “hash” environment with parent
``baseenv`` is used instead.


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
specified with a valid ``RxComputeContext``.
Currently local and ``RxInSqlServer`` compute contexts
are supported.


##### ensemble

NOT SUPPORTED. Control parameters for ensembling.


### Returns

A [``NeuralNetwork``](learners_object.md) object with the trained model.


### Note

This algorithm is single-threaded and will not attempt to load the entire dataset into
memory.


### See also

[``adadelta_optimizer``](adadelta_optimizer.md),
[``sgd_optimizer``](sgd_optimizer.md),
[``avx_math``](avx_math.md),
[``clr_math``](clr_math.md),
[``gpu_math``](gpu_math.md),
[``mkl_math``](mkl_math.md),
[``sse_math``](sse_math.md),
[``rx_predict``](rx_predict.md).


### References

[Wikipedia: Artificial neural network](http://en.wikipedia.org/wiki/Artificial_neural_network.md)


### Example



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
Warning: Training data does not support shuffling, so ignoring request to shuffle
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
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Estimated Pre-training MeanError = 0.739703
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:1/100, MeanErr=0.683761(-7.56%), 38.04M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:2/100, MeanErr=0.648077(-5.22%), 44.89M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:3/100, MeanErr=0.643671(-0.68%), 31.73M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:4/100, MeanErr=0.642908(-0.12%), 44.58M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:5/100, MeanErr=0.642720(-0.03%), 47.49M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:6/100, MeanErr=0.642656(-0.01%), 38.09M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:7/100, MeanErr=0.642626(0.00%), 37.31M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:8/100, MeanErr=0.642607(0.00%), 49.70M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:9/100, MeanErr=0.642591(0.00%), 46.34M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:10/100, MeanErr=0.642577(0.00%), 46.88M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:11/100, MeanErr=0.642563(0.00%), 50.45M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:12/100, MeanErr=0.642549(0.00%), 51.89M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:13/100, MeanErr=0.642535(0.00%), 54.48M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:14/100, MeanErr=0.642521(0.00%), 51.95M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:15/100, MeanErr=0.642507(0.00%), 45.85M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:16/100, MeanErr=0.642493(0.00%), 53.67M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:17/100, MeanErr=0.642479(0.00%), 52.86M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:18/100, MeanErr=0.642465(0.00%), 47.69M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:19/100, MeanErr=0.642451(0.00%), 46.52M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:20/100, MeanErr=0.642437(0.00%), 50.72M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:21/100, MeanErr=0.642423(0.00%), 55.87M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:22/100, MeanErr=0.642409(0.00%), 49.30M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:23/100, MeanErr=0.642395(0.00%), 51.21M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:24/100, MeanErr=0.642381(0.00%), 56.24M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:25/100, MeanErr=0.642367(0.00%), 51.28M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:26/100, MeanErr=0.642353(0.00%), 55.13M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:27/100, MeanErr=0.642339(0.00%), 50.36M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:28/100, MeanErr=0.642325(0.00%), 55.31M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:29/100, MeanErr=0.642311(0.00%), 49.60M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:30/100, MeanErr=0.642296(0.00%), 56.90M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:31/100, MeanErr=0.642282(0.00%), 49.68M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:32/100, MeanErr=0.642268(0.00%), 55.94M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:33/100, MeanErr=0.642254(0.00%), 13.90M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:34/100, MeanErr=0.642239(0.00%), 48.70M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:35/100, MeanErr=0.642225(0.00%), 53.76M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:36/100, MeanErr=0.642211(0.00%), 50.44M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:37/100, MeanErr=0.642196(0.00%), 50.85M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:38/100, MeanErr=0.642182(0.00%), 55.01M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:39/100, MeanErr=0.642167(0.00%), 56.06M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:40/100, MeanErr=0.642153(0.00%), 49.78M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:41/100, MeanErr=0.642138(0.00%), 54.68M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:42/100, MeanErr=0.642124(0.00%), 53.09M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:43/100, MeanErr=0.642109(0.00%), 48.94M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:44/100, MeanErr=0.642094(0.00%), 46.54M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:45/100, MeanErr=0.642080(0.00%), 55.53M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:46/100, MeanErr=0.642065(0.00%), 29.99M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:47/100, MeanErr=0.642050(0.00%), 51.12M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:48/100, MeanErr=0.642035(0.00%), 54.88M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:49/100, MeanErr=0.642021(0.00%), 48.07M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:50/100, MeanErr=0.642006(0.00%), 49.33M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:51/100, MeanErr=0.641991(0.00%), 47.84M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:52/100, MeanErr=0.641976(0.00%), 52.12M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:53/100, MeanErr=0.641961(0.00%), 53.40M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:54/100, MeanErr=0.641946(0.00%), 49.96M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:55/100, MeanErr=0.641931(0.00%), 51.92M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:56/100, MeanErr=0.641916(0.00%), 53.78M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:57/100, MeanErr=0.641900(0.00%), 53.23M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:58/100, MeanErr=0.641885(0.00%), 30.30M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:59/100, MeanErr=0.641870(0.00%), 51.44M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:60/100, MeanErr=0.641855(0.00%), 36.17M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:61/100, MeanErr=0.641839(0.00%), 36.54M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:62/100, MeanErr=0.641824(0.00%), 37.08M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:63/100, MeanErr=0.641808(0.00%), 37.78M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:64/100, MeanErr=0.641793(0.00%), 38.03M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:65/100, MeanErr=0.641777(0.00%), 43.38M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:66/100, MeanErr=0.641762(0.00%), 45.75M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:67/100, MeanErr=0.641746(0.00%), 42.68M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:68/100, MeanErr=0.641730(0.00%), 41.75M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:69/100, MeanErr=0.641715(0.00%), 47.43M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:70/100, MeanErr=0.641699(0.00%), 43.77M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:71/100, MeanErr=0.641683(0.00%), 40.96M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:72/100, MeanErr=0.641667(0.00%), 40.56M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:73/100, MeanErr=0.641651(0.00%), 41.99M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:74/100, MeanErr=0.641635(0.00%), 37.58M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:75/100, MeanErr=0.641619(0.00%), 49.81M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:76/100, MeanErr=0.641603(0.00%), 45.45M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:77/100, MeanErr=0.641587(0.00%), 41.55M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:78/100, MeanErr=0.641570(0.00%), 50.41M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:79/100, MeanErr=0.641554(0.00%), 47.22M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:80/100, MeanErr=0.641538(0.00%), 53.25M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:81/100, MeanErr=0.641521(0.00%), 58.29M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:82/100, MeanErr=0.641505(0.00%), 59.41M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:83/100, MeanErr=0.641488(0.00%), 56.09M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:84/100, MeanErr=0.641471(0.00%), 59.00M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:85/100, MeanErr=0.641455(0.00%), 58.77M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:86/100, MeanErr=0.641438(0.00%), 59.88M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:87/100, MeanErr=0.641421(0.00%), 57.39M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:88/100, MeanErr=0.641404(0.00%), 56.77M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:89/100, MeanErr=0.641387(0.00%), 58.62M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:90/100, MeanErr=0.641370(0.00%), 54.02M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:91/100, MeanErr=0.641353(0.00%), 55.30M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:92/100, MeanErr=0.641336(0.00%), 46.65M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:93/100, MeanErr=0.641319(0.00%), 54.06M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:94/100, MeanErr=0.641301(0.00%), 54.03M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:95/100, MeanErr=0.641284(0.00%), 57.14M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:96/100, MeanErr=0.641266(0.00%), 21.37M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:97/100, MeanErr=0.641249(0.00%), 51.08M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:98/100, MeanErr=0.641231(0.00%), 57.02M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:99/100, MeanErr=0.641214(0.00%), 57.81M WeightUpdates/sec
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:100/100, MeanErr=0.641196(0.00%), 58.54M WeightUpdates/sec
Done!
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Estimated Post-training MeanError = 0.638710
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.4102244
Elapsed time: 00:00:00.0228952
Beginning processing data.
Rows Read: 62, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.1031192
Finished writing 62 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre1477668.xdf File will be overwritten if it exists.

Rows Processed: 5 
  isCase PredictedLabel     Score  Probability
0  False          False -0.634014     0.346600
1  False          False -0.633305     0.346761
2   True          False -0.633376     0.346745
3  False          False -0.633142     0.346798
4  False          False -0.634376     0.346518
```



### Example



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
Warning: Training data does not support shuffling, so ignoring request to shuffle
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
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Estimated Pre-training MeanError = 1.939311
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:1/100, MeanErr=1.932797(-0.34%), 37.82M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:2/100, MeanErr=1.921365(-0.59%), 35.42M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:3/100, MeanErr=1.919943(-0.07%), 23.34M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:4/100, MeanErr=1.919853(0.00%), 37.59M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:5/100, MeanErr=1.919846(0.00%), 37.73M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:6/100, MeanErr=1.919806(0.00%), 39.13M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:7/100, MeanErr=1.919742(0.00%), 38.63M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:8/100, MeanErr=1.919669(0.00%), 37.19M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:9/100, MeanErr=1.919591(0.00%), 39.10M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:10/100, MeanErr=1.919511(0.00%), 38.70M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:11/100, MeanErr=1.919430(0.00%), 39.48M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:12/100, MeanErr=1.919348(0.00%), 40.09M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:13/100, MeanErr=1.919266(0.00%), 38.08M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:14/100, MeanErr=1.919184(0.00%), 36.41M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:15/100, MeanErr=1.919102(0.00%), 35.11M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:16/100, MeanErr=1.919019(0.00%), 36.59M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:17/100, MeanErr=1.918935(0.00%), 36.68M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:18/100, MeanErr=1.918852(0.00%), 39.18M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:19/100, MeanErr=1.918768(0.00%), 37.57M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:20/100, MeanErr=1.918683(0.00%), 36.32M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:21/100, MeanErr=1.918599(0.00%), 32.46M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:22/100, MeanErr=1.918513(0.00%), 39.63M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:23/100, MeanErr=1.918428(0.00%), 39.05M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:24/100, MeanErr=1.918342(0.00%), 34.41M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:25/100, MeanErr=1.918255(0.00%), 37.29M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:26/100, MeanErr=1.918168(0.00%), 30.67M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:27/100, MeanErr=1.918081(0.00%), 33.98M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:28/100, MeanErr=1.917993(0.00%), 38.06M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:29/100, MeanErr=1.917904(0.00%), 39.57M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:30/100, MeanErr=1.917815(0.00%), 38.91M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:31/100, MeanErr=1.917725(0.00%), 39.71M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:32/100, MeanErr=1.917635(0.00%), 38.82M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:33/100, MeanErr=1.917545(0.00%), 17.81M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:34/100, MeanErr=1.917453(0.00%), 38.21M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:35/100, MeanErr=1.917361(0.00%), 40.85M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:36/100, MeanErr=1.917269(0.00%), 38.78M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:37/100, MeanErr=1.917176(0.00%), 37.37M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:38/100, MeanErr=1.917082(0.00%), 30.26M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:39/100, MeanErr=1.916987(0.00%), 37.25M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:40/100, MeanErr=1.916892(0.00%), 38.95M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:41/100, MeanErr=1.916797(0.00%), 40.04M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:42/100, MeanErr=1.916700(-0.01%), 36.32M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:43/100, MeanErr=1.916603(-0.01%), 38.32M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:44/100, MeanErr=1.916505(-0.01%), 33.20M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:45/100, MeanErr=1.916406(-0.01%), 38.11M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:46/100, MeanErr=1.916307(-0.01%), 42.42M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:47/100, MeanErr=1.916206(-0.01%), 39.43M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:48/100, MeanErr=1.916105(-0.01%), 39.97M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:49/100, MeanErr=1.916004(-0.01%), 22.03M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:50/100, MeanErr=1.915901(-0.01%), 37.29M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:51/100, MeanErr=1.915797(-0.01%), 37.62M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:52/100, MeanErr=1.915693(-0.01%), 34.18M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:53/100, MeanErr=1.915588(-0.01%), 40.87M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:54/100, MeanErr=1.915482(-0.01%), 40.76M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:55/100, MeanErr=1.915375(-0.01%), 40.57M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:56/100, MeanErr=1.915267(-0.01%), 39.50M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:57/100, MeanErr=1.915158(-0.01%), 36.69M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:58/100, MeanErr=1.915048(-0.01%), 41.42M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:59/100, MeanErr=1.914937(-0.01%), 39.89M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:60/100, MeanErr=1.914825(-0.01%), 40.28M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:61/100, MeanErr=1.914713(-0.01%), 35.41M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:62/100, MeanErr=1.914599(-0.01%), 39.13M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:63/100, MeanErr=1.914484(-0.01%), 37.14M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:64/100, MeanErr=1.914368(-0.01%), 14.93M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:65/100, MeanErr=1.914251(-0.01%), 37.28M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:66/100, MeanErr=1.914133(-0.01%), 40.25M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:67/100, MeanErr=1.914013(-0.01%), 35.58M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:68/100, MeanErr=1.913893(-0.01%), 42.65M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:69/100, MeanErr=1.913772(-0.01%), 37.33M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:70/100, MeanErr=1.913649(-0.01%), 40.79M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:71/100, MeanErr=1.913525(-0.01%), 40.69M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:72/100, MeanErr=1.913400(-0.01%), 39.87M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:73/100, MeanErr=1.913274(-0.01%), 41.28M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:74/100, MeanErr=1.913146(-0.01%), 36.73M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:75/100, MeanErr=1.913017(-0.01%), 36.40M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:76/100, MeanErr=1.912887(-0.01%), 39.87M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:77/100, MeanErr=1.912756(-0.01%), 39.15M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:78/100, MeanErr=1.912623(-0.01%), 40.30M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:79/100, MeanErr=1.912489(-0.01%), 38.72M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:80/100, MeanErr=1.912354(-0.01%), 29.15M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:81/100, MeanErr=1.912217(-0.01%), 35.44M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:82/100, MeanErr=1.912079(-0.01%), 38.01M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:83/100, MeanErr=1.911939(-0.01%), 39.05M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:84/100, MeanErr=1.911798(-0.01%), 36.75M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:85/100, MeanErr=1.911655(-0.01%), 41.40M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:86/100, MeanErr=1.911511(-0.01%), 38.08M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:87/100, MeanErr=1.911366(-0.01%), 33.25M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:88/100, MeanErr=1.911218(-0.01%), 30.90M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:89/100, MeanErr=1.911070(-0.01%), 40.71M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:90/100, MeanErr=1.910920(-0.01%), 41.91M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:91/100, MeanErr=1.910768(-0.01%), 37.50M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:92/100, MeanErr=1.910614(-0.01%), 40.02M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:93/100, MeanErr=1.910459(-0.01%), 38.15M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:94/100, MeanErr=1.910302(-0.01%), 37.35M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:95/100, MeanErr=1.910144(-0.01%), 39.30M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:96/100, MeanErr=1.909984(-0.01%), 38.22M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:97/100, MeanErr=1.909822(-0.01%), 40.04M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:98/100, MeanErr=1.909658(-0.01%), 41.32M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:99/100, MeanErr=1.909492(-0.01%), 38.34M WeightUpdates/sec
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:100/100, MeanErr=1.909325(-0.01%), 36.37M WeightUpdates/sec
Done!
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Estimated Post-training MeanError = 1.899862
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.3905784
Elapsed time: 00:00:00.0254920
Beginning processing data.
Rows Read: 38, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0976943
Finished writing 38 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre1477679.xdf File will be overwritten if it exists.

Rows Processed: 5 
      Species   Score.0   Score.1   Score.2
0  versicolor  0.340877  0.353992  0.305132
1  versicolor  0.340993  0.353902  0.305105
2      setosa  0.347136  0.351932  0.300932
3      setosa  0.347043  0.352020  0.300936
4   virginica  0.338873  0.354638  0.306489
```



### Example



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
Warning: Training data does not support shuffling, so ignoring request to shuffle
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
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Estimated Pre-training MeanError = 4404.420676
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:1/100, MeanErr=1556.382659(-64.66%), 7.83M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:2/100, MeanErr=126.349075(-91.88%), 8.41M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:3/100, MeanErr=135.388982(7.15%), 7.25M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:4/100, MeanErr=136.086021(0.51%), 8.00M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:5/100, MeanErr=135.384643(-0.52%), 7.80M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:6/100, MeanErr=134.640104(-0.55%), 8.23M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:7/100, MeanErr=133.914178(-0.54%), 8.48M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:8/100, MeanErr=133.208014(-0.53%), 8.54M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:9/100, MeanErr=132.520108(-0.52%), 8.80M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:10/100, MeanErr=131.849008(-0.51%), 7.80M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:11/100, MeanErr=131.193385(-0.50%), 8.08M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:12/100, MeanErr=130.551928(-0.49%), 7.21M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:13/100, MeanErr=129.923558(-0.48%), 8.54M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:14/100, MeanErr=129.307087(-0.47%), 6.90M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:15/100, MeanErr=128.701660(-0.47%), 3.62M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:16/100, MeanErr=128.106296(-0.46%), 5.75M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:17/100, MeanErr=127.520164(-0.46%), 5.93M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:18/100, MeanErr=126.942516(-0.45%), 6.61M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:19/100, MeanErr=126.372612(-0.45%), 6.34M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:20/100, MeanErr=125.809722(-0.45%), 6.62M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:21/100, MeanErr=125.253315(-0.44%), 6.35M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:22/100, MeanErr=124.702799(-0.44%), 5.59M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:23/100, MeanErr=124.157563(-0.44%), 6.84M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:24/100, MeanErr=123.617228(-0.44%), 6.79M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:25/100, MeanErr=123.081228(-0.43%), 5.22M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:26/100, MeanErr=122.549218(-0.43%), 5.80M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:27/100, MeanErr=122.020713(-0.43%), 6.73M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:28/100, MeanErr=121.495461(-0.43%), 7.18M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:29/100, MeanErr=120.972991(-0.43%), 5.47M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:30/100, MeanErr=120.453106(-0.43%), 6.73M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:31/100, MeanErr=119.935389(-0.43%), 6.95M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:32/100, MeanErr=119.419678(-0.43%), 8.42M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:33/100, MeanErr=118.905630(-0.43%), 8.46M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:34/100, MeanErr=118.393058(-0.43%), 6.42M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:35/100, MeanErr=117.881758(-0.43%), 8.55M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:36/100, MeanErr=117.371514(-0.43%), 8.10M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:37/100, MeanErr=116.862108(-0.43%), 8.05M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:38/100, MeanErr=116.353431(-0.44%), 6.35M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:39/100, MeanErr=115.845202(-0.44%), 8.25M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:40/100, MeanErr=115.337429(-0.44%), 8.55M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:41/100, MeanErr=114.829910(-0.44%), 8.38M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:42/100, MeanErr=114.322493(-0.44%), 7.18M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:43/100, MeanErr=113.815146(-0.44%), 7.98M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:44/100, MeanErr=113.307715(-0.45%), 8.41M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:45/100, MeanErr=112.800119(-0.45%), 8.73M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:46/100, MeanErr=112.292293(-0.45%), 7.90M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:47/100, MeanErr=111.784126(-0.45%), 7.96M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:48/100, MeanErr=111.275606(-0.45%), 8.99M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:49/100, MeanErr=110.766614(-0.46%), 8.85M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:50/100, MeanErr=110.257185(-0.46%), 8.60M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:51/100, MeanErr=109.747257(-0.46%), 8.32M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:52/100, MeanErr=109.236774(-0.47%), 7.58M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:53/100, MeanErr=108.725731(-0.47%), 8.54M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:54/100, MeanErr=108.214105(-0.47%), 9.17M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:55/100, MeanErr=107.701900(-0.47%), 8.60M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:56/100, MeanErr=107.189050(-0.48%), 8.02M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:57/100, MeanErr=106.675648(-0.48%), 8.51M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:58/100, MeanErr=106.161614(-0.48%), 3.09M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:59/100, MeanErr=105.647012(-0.48%), 8.15M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:60/100, MeanErr=105.131831(-0.49%), 8.97M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:61/100, MeanErr=104.616184(-0.49%), 8.73M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:62/100, MeanErr=104.099938(-0.49%), 8.98M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:63/100, MeanErr=103.583217(-0.50%), 9.36M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:64/100, MeanErr=103.066081(-0.50%), 7.67M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:65/100, MeanErr=102.548516(-0.50%), 7.48M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:66/100, MeanErr=102.030591(-0.51%), 8.49M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:67/100, MeanErr=101.512362(-0.51%), 8.34M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:68/100, MeanErr=100.993812(-0.51%), 8.43M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:69/100, MeanErr=100.475120(-0.51%), 7.67M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:70/100, MeanErr=99.956234(-0.52%), 9.07M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:71/100, MeanErr=99.437245(-0.52%), 9.14M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:72/100, MeanErr=98.918212(-0.52%), 7.79M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:73/100, MeanErr=98.399250(-0.52%), 8.58M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:74/100, MeanErr=97.880379(-0.53%), 8.82M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:75/100, MeanErr=97.361669(-0.53%), 4.93M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:76/100, MeanErr=96.843146(-0.53%), 8.80M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:77/100, MeanErr=96.324996(-0.54%), 8.88M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:78/100, MeanErr=95.807209(-0.54%), 6.89M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:79/100, MeanErr=95.289908(-0.54%), 5.29M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:80/100, MeanErr=94.773125(-0.54%), 7.64M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:81/100, MeanErr=94.256964(-0.54%), 7.30M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:82/100, MeanErr=93.741481(-0.55%), 9.21M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:83/100, MeanErr=93.226868(-0.55%), 8.32M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:84/100, MeanErr=92.713053(-0.55%), 7.40M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:85/100, MeanErr=92.200250(-0.55%), 7.69M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:86/100, MeanErr=91.688481(-0.56%), 8.67M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:87/100, MeanErr=91.177795(-0.56%), 8.56M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Iter:88/100, MeanErr=90.668352(-0.56%), 8.84M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:89/100, MeanErr=90.160213(-0.56%), 8.81M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:90/100, MeanErr=89.653455(-0.56%), 9.10M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:91/100, MeanErr=89.148198(-0.56%), 3.60M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:92/100, MeanErr=88.644471(-0.57%), 7.77M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:93/100, MeanErr=88.142379(-0.57%), 8.30M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:94/100, MeanErr=87.642032(-0.57%), 7.85M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:95/100, MeanErr=87.143463(-0.57%), 8.94M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:96/100, MeanErr=86.646854(-0.57%), 9.09M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:97/100, MeanErr=86.152191(-0.57%), 8.43M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:98/100, MeanErr=85.659597(-0.57%), 6.93M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Iter:99/100, MeanErr=85.169166(-0.57%), 8.55M WeightUpdates/sec
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0.001
Beginning processing data.
Iter:100/100, MeanErr=84.680970(-0.57%), 8.70M WeightUpdates/sec
Done!
Beginning processing data.
Rows Read: 22, Read Time: 0, Transform Time: 0
Beginning processing data.
Estimated Post-training MeanError = 86.662922
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.3825717
Elapsed time: 00:00:00.0278796
Beginning processing data.
Rows Read: 8, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0994426
Finished writing 8 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre14776810.xdf File will be overwritten if it exists.

Rows Processed: 5 
   rating      Score
0    43.0  63.270035
1    63.0  67.431671
2    63.0  66.522995
3    64.0  68.888336
4    66.0  71.331184
```

