--- 
 
# required metadata 
title: "Neural Net" 
description: "Neural networks for regression modeling and for Binary and multi-class classification." 
keywords: "models, classification, regression, neural network, dnn" 
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

## *rx_neural_network*: Neural Network


*Applies to:* SQL Server 2017, Machine Learning Services 9.3

* [optimizers](optimizers.md) 

* [math](math.md) 


### Usage



```
microsoftml.rx_neural_network(formula: str, data: [<class ‘revoscalepy.datasource.RxDataSource.RxDataSource’>, <class ‘pandas.core.frame.DataFrame’>], method: [‘binary’, ‘multiClass’, ‘regression’] = ‘binary’, num_hidden_nodes: int = 100, num_iterations: int = 100, optimizer: [<function adadelta_optimizer at 0x00000177684130D0>, <function sgd_optimizer at 0x0000017768401BF8>] = {‘settings’: {}, ‘name’: ‘SgdOptimizer’}, net_definition: str = None, init_wts_diameter: float = 0.1, max_norm: float = 0, acceleration: [<function avx_math at 0x0000017768401F28>, <function clr_math at 0x0000017768413268>, <function gpu_math at 0x00000177684132F0>, <function mkl_math at 0x0000017768413378>, <function sse_math at 0x0000017768413400>] = {‘settings’: {}, ‘name’: ‘AvxMath’}, mini_batch_size: int = 1, normalize: [‘No’, ‘Warn’, ‘Auto’, ‘Yes’] = ‘Auto’, ml_transforms: list = None, ml_transform_vars: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment: dict = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, ensemble: dict = None, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
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

The formula as described in [revoscalepy.rx_formula](https://docs.microsoft.com/en-us/r-server/r-reference/revoscalpy/rx_formula).
Interaction terms and `F()` are not currently supported in the
[microsoftml](https://docs.microsoft.com/en-us/r-server/r/concept-what-is-the-microsoftml-package).


##### data

A data source object or a character string specifying a
*.xdf* file or a data frame object.


##### method

A character string denoting Fast Tree type:

* `"binary"` for the default binary classification neural network. 

* `"multiClass"` for multi-class classification neural network. 

* `"regression"` for a regression neural network. 


##### num_hidden_nodes

The default number of hidden nodes in the neural net.
The default value is 100.


##### num_iterations

The number of iterations on the full training set.
The default value is 100.


##### optimizer

A list specifying either the `sgd` or `adaptive`
optimization algorithm. This list can be created using `sgd()` or
`ada_delta_sgd()`. The default value is `sgd`.


##### net_definition

The Net# definition of the structure of the neural
network. For more information about the Net# language, see
[Reference Guide](https://azure.microsoft.com/en-us/documentation/articles/machine-learning-azure-ml-netsharp-reference-guide/)


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

* Download and install NVidia CUDA Toolkit 6.5 ([CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit-65)). 

* Download and install NVidia cuDNN v2 Library ([cudnn Library](https://developer.nvidia.com/rdp/cudnn-archive)). 

* Find the libs directory of the microsoftml package by calling `import microsoftml, os`, `os.path.join(microsoftml.__path__[0], "mxLibs")`. 

* Copy cublas64_65.dll, cudart64_65.dll and cusparse64_65.dll from the CUDA Toolkit 6.5 into the libs directory of the microsoftml package. 

* Copy cudnn64_65.dll from the cuDNN v2 Library into the libs directory of the microsoftml package. 


##### mini_batch_size

Sets the mini-batch size. Recommended values are between
1 and 256. This parameter is only used when the acceleration is GPU. Setting
this parameter to a higher value improves the speed of training, but it might
negatively affect the accuracy. The default value is 1.


##### normalize

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


##### ml_transforms

Specifies a list of MicrosoftML transforms to be
performed on the data before training or *None* if no transforms are
to be performed. See [`featurize_text`](featurize_text.md),
[`categorical`](categorical.md),
and [`categorical_hash`](categorical_hash.md), for transformations that are supported.
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

NOT SUPPORTED. An expression of the form that represents the
first round of variable transformations. As with
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
specified with a valid [revoscalepy.RxComputeContext](https://docs.microsoft.com/en-us/r-server/r-reference/revoscalpy/RxComputeContext).
Currently local and [revoscalepy.RxInSqlServer](https://docs.microsoft.com/en-us/r-server/r-reference/revoscalpy/RxInSqlServer) compute contexts
are supported.


##### ensemble

NOT SUPPORTED. Control parameters for ensembling.


### Returns

A [`NeuralNetwork`](learners_object.md) object with the trained model.


### Note

This algorithm is single-threaded and will not attempt to load the entire dataset into
memory.


### See also

[`adadelta_optimizer`](adadelta_optimizer.md),
[`sgd_optimizer`](sgd_optimizer.md),
[`avx_math`](avx_math.md),
[`clr_math`](clr_math.md),
[`gpu_math`](gpu_math.md),
[`mkl_math`](mkl_math.md),
[`sse_math`](sse_math.md),
[`rx_predict`](rx_predict.md).


### References

[Wikipedia: Artificial neural network](http://en.wikipedia.org/wiki/Artificial_neural_network)


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
Rows Read: 186, Read Time: 0.001, Transform Time: 0
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
Estimated Pre-training MeanError = 0.730590
Iter:1/100, MeanErr=0.691490(-5.35%), 130.09M WeightUpdates/sec
Iter:2/100, MeanErr=0.666216(-3.66%), 134.71M WeightUpdates/sec
Iter:3/100, MeanErr=0.664896(-0.20%), 126.21M WeightUpdates/sec
Iter:4/100, MeanErr=0.664536(-0.05%), 133.13M WeightUpdates/sec
Iter:5/100, MeanErr=0.664888(0.05%), 136.78M WeightUpdates/sec
Iter:6/100, MeanErr=0.664904(0.00%), 98.70M WeightUpdates/sec
Iter:7/100, MeanErr=0.665018(0.02%), 61.67M WeightUpdates/sec
Iter:8/100, MeanErr=0.664918(-0.02%), 125.35M WeightUpdates/sec
Iter:9/100, MeanErr=0.664909(0.00%), 141.28M WeightUpdates/sec
Iter:10/100, MeanErr=0.664908(0.00%), 131.59M WeightUpdates/sec
Iter:11/100, MeanErr=0.664451(-0.07%), 128.32M WeightUpdates/sec
Iter:12/100, MeanErr=0.663529(-0.14%), 135.15M WeightUpdates/sec
Iter:13/100, MeanErr=0.664531(0.15%), 144.50M WeightUpdates/sec
Iter:14/100, MeanErr=0.664893(0.05%), 132.23M WeightUpdates/sec
Iter:15/100, MeanErr=0.664734(-0.02%), 141.28M WeightUpdates/sec
Iter:16/100, MeanErr=0.664658(-0.01%), 126.07M WeightUpdates/sec
Iter:17/100, MeanErr=0.664414(-0.04%), 121.92M WeightUpdates/sec
Iter:18/100, MeanErr=0.664565(0.02%), 85.39M WeightUpdates/sec
Iter:19/100, MeanErr=0.663448(-0.17%), 85.06M WeightUpdates/sec
Iter:20/100, MeanErr=0.664974(0.23%), 115.80M WeightUpdates/sec
Iter:21/100, MeanErr=0.664760(-0.03%), 105.99M WeightUpdates/sec
Iter:22/100, MeanErr=0.664799(0.01%), 107.02M WeightUpdates/sec
Iter:23/100, MeanErr=0.664361(-0.07%), 58.39M WeightUpdates/sec
Iter:24/100, MeanErr=0.664651(0.04%), 121.61M WeightUpdates/sec
Iter:25/100, MeanErr=0.664480(-0.03%), 138.03M WeightUpdates/sec
Iter:26/100, MeanErr=0.664844(0.05%), 158.83M WeightUpdates/sec
Iter:27/100, MeanErr=0.664653(-0.03%), 159.83M WeightUpdates/sec
Iter:28/100, MeanErr=0.664116(-0.08%), 167.11M WeightUpdates/sec
Iter:29/100, MeanErr=0.662992(-0.17%), 151.81M WeightUpdates/sec
Iter:30/100, MeanErr=0.664311(0.20%), 128.17M WeightUpdates/sec
Iter:31/100, MeanErr=0.664438(0.02%), 139.02M WeightUpdates/sec
Iter:32/100, MeanErr=0.663755(-0.10%), 149.47M WeightUpdates/sec
Iter:33/100, MeanErr=0.665021(0.19%), 150.50M WeightUpdates/sec
Iter:34/100, MeanErr=0.664630(-0.06%), 151.95M WeightUpdates/sec
Iter:35/100, MeanErr=0.664332(-0.04%), 159.59M WeightUpdates/sec
Iter:36/100, MeanErr=0.664627(0.04%), 106.22M WeightUpdates/sec
Iter:37/100, MeanErr=0.663591(-0.16%), 143.50M WeightUpdates/sec
Iter:38/100, MeanErr=0.664399(0.12%), 134.77M WeightUpdates/sec
Iter:39/100, MeanErr=0.664797(0.06%), 157.61M WeightUpdates/sec
Iter:40/100, MeanErr=0.664349(-0.07%), 130.35M WeightUpdates/sec
Iter:41/100, MeanErr=0.664250(-0.01%), 103.56M WeightUpdates/sec
Iter:42/100, MeanErr=0.664207(-0.01%), 122.46M WeightUpdates/sec
Iter:43/100, MeanErr=0.664357(0.02%), 118.85M WeightUpdates/sec
Iter:44/100, MeanErr=0.664683(0.05%), 110.86M WeightUpdates/sec
Iter:45/100, MeanErr=0.664323(-0.05%), 132.12M WeightUpdates/sec
Iter:46/100, MeanErr=0.664173(-0.02%), 134.49M WeightUpdates/sec
Iter:47/100, MeanErr=0.663999(-0.03%), 145.84M WeightUpdates/sec
Iter:48/100, MeanErr=0.663668(-0.05%), 159.06M WeightUpdates/sec
Iter:49/100, MeanErr=0.664426(0.11%), 100.07M WeightUpdates/sec
Iter:50/100, MeanErr=0.663205(-0.18%), 140.56M WeightUpdates/sec
Iter:51/100, MeanErr=0.663493(0.04%), 156.94M WeightUpdates/sec
Iter:52/100, MeanErr=0.664565(0.16%), 163.55M WeightUpdates/sec
Iter:53/100, MeanErr=0.664257(-0.05%), 154.30M WeightUpdates/sec
Iter:54/100, MeanErr=0.664533(0.04%), 158.37M WeightUpdates/sec
Iter:55/100, MeanErr=0.664280(-0.04%), 149.61M WeightUpdates/sec
Iter:56/100, MeanErr=0.664543(0.04%), 154.08M WeightUpdates/sec
Iter:57/100, MeanErr=0.664410(-0.02%), 104.91M WeightUpdates/sec
Iter:58/100, MeanErr=0.664104(-0.05%), 135.32M WeightUpdates/sec
Iter:59/100, MeanErr=0.664460(0.05%), 141.22M WeightUpdates/sec
Iter:60/100, MeanErr=0.664304(-0.02%), 145.58M WeightUpdates/sec
Iter:61/100, MeanErr=0.664408(0.02%), 150.15M WeightUpdates/sec
Iter:62/100, MeanErr=0.664059(-0.05%), 96.64M WeightUpdates/sec
Iter:63/100, MeanErr=0.664333(0.04%), 151.05M WeightUpdates/sec
Iter:64/100, MeanErr=0.663928(-0.06%), 157.76M WeightUpdates/sec
Iter:65/100, MeanErr=0.664319(0.06%), 154.37M WeightUpdates/sec
Iter:66/100, MeanErr=0.664129(-0.03%), 157.46M WeightUpdates/sec
Iter:67/100, MeanErr=0.664231(0.02%), 157.01M WeightUpdates/sec
Iter:68/100, MeanErr=0.663682(-0.08%), 167.11M WeightUpdates/sec
Iter:69/100, MeanErr=0.664151(0.07%), 160.92M WeightUpdates/sec
Iter:70/100, MeanErr=0.663072(-0.16%), 129.89M WeightUpdates/sec
Iter:71/100, MeanErr=0.664193(0.17%), 103.04M WeightUpdates/sec
Iter:72/100, MeanErr=0.663938(-0.04%), 127.87M WeightUpdates/sec
Iter:73/100, MeanErr=0.664114(0.03%), 103.85M WeightUpdates/sec
Iter:74/100, MeanErr=0.664101(0.00%), 101.30M WeightUpdates/sec
Iter:75/100, MeanErr=0.664130(0.00%), 76.86M WeightUpdates/sec
Iter:76/100, MeanErr=0.664208(0.01%), 81.45M WeightUpdates/sec
Iter:77/100, MeanErr=0.664257(0.01%), 83.43M WeightUpdates/sec
Iter:78/100, MeanErr=0.663957(-0.05%), 60.13M WeightUpdates/sec
Iter:79/100, MeanErr=0.663993(0.01%), 113.14M WeightUpdates/sec
Iter:80/100, MeanErr=0.663889(-0.02%), 141.10M WeightUpdates/sec
Iter:81/100, MeanErr=0.664064(0.03%), 117.91M WeightUpdates/sec
Iter:82/100, MeanErr=0.660868(-0.48%), 141.53M WeightUpdates/sec
Iter:83/100, MeanErr=0.664742(0.59%), 136.95M WeightUpdates/sec
Iter:84/100, MeanErr=0.663644(-0.17%), 138.27M WeightUpdates/sec
Iter:85/100, MeanErr=0.664162(0.08%), 131.39M WeightUpdates/sec
Iter:86/100, MeanErr=0.664042(-0.02%), 106.33M WeightUpdates/sec
Iter:87/100, MeanErr=0.663140(-0.14%), 121.29M WeightUpdates/sec
Iter:88/100, MeanErr=0.664320(0.18%), 88.20M WeightUpdates/sec
Iter:89/100, MeanErr=0.663686(-0.10%), 143.00M WeightUpdates/sec
Iter:90/100, MeanErr=0.664123(0.07%), 152.80M WeightUpdates/sec
Iter:91/100, MeanErr=0.663631(-0.07%), 140.62M WeightUpdates/sec
Iter:92/100, MeanErr=0.663809(0.03%), 150.22M WeightUpdates/sec
Iter:93/100, MeanErr=0.663860(0.01%), 130.14M WeightUpdates/sec
Iter:94/100, MeanErr=0.663814(-0.01%), 155.17M WeightUpdates/sec
Iter:95/100, MeanErr=0.664074(0.04%), 153.58M WeightUpdates/sec
Iter:96/100, MeanErr=0.663993(-0.01%), 148.53M WeightUpdates/sec
Iter:97/100, MeanErr=0.663752(-0.04%), 151.60M WeightUpdates/sec
Iter:98/100, MeanErr=0.664019(0.04%), 152.02M WeightUpdates/sec
Iter:99/100, MeanErr=0.662735(-0.19%), 134.71M WeightUpdates/sec
Iter:100/100, MeanErr=0.664119(0.21%), 135.15M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 0.661113
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2275649
Elapsed time: 00:00:00.0152035
Beginning processing data.
Rows Read: 62, Read Time: 0.001, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0678565
Finished writing 62 rows.
Writing completed.
Data will be written to C:\Users\xavie\AppData\Local\Temp\rre677668.xdf File will be overwritten if it exists.

Rows Processed: 5 
  isCase PredictedLabel     Score  Probability
0  False          False -0.526422     0.371352
1  False          False -0.527511     0.371098
2  False          False -0.526885     0.371244
3  False          False -0.527576     0.371083
4  False          False -0.527263     0.371156
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
Estimated Pre-training MeanError = 1.925313
Iter:1/100, MeanErr=1.926644(0.07%), 21.67M WeightUpdates/sec
Iter:2/100, MeanErr=1.918046(-0.45%), 42.21M WeightUpdates/sec
Iter:3/100, MeanErr=1.917902(-0.01%), 34.93M WeightUpdates/sec
Iter:4/100, MeanErr=1.917902(0.00%), 94.03M WeightUpdates/sec
Iter:5/100, MeanErr=1.917264(-0.03%), 111.87M WeightUpdates/sec
Iter:6/100, MeanErr=1.917767(0.03%), 89.73M WeightUpdates/sec
Iter:7/100, MeanErr=1.917295(-0.02%), 86.36M WeightUpdates/sec
Iter:8/100, MeanErr=1.917244(0.00%), 72.48M WeightUpdates/sec
Iter:9/100, MeanErr=1.917821(0.03%), 82.30M WeightUpdates/sec
Iter:10/100, MeanErr=1.913704(-0.21%), 84.50M WeightUpdates/sec
Iter:11/100, MeanErr=1.916545(0.15%), 23.26M WeightUpdates/sec
Iter:12/100, MeanErr=1.916350(-0.01%), 89.70M WeightUpdates/sec
Iter:13/100, MeanErr=1.916910(0.03%), 103.01M WeightUpdates/sec
Iter:14/100, MeanErr=1.916924(0.00%), 76.68M WeightUpdates/sec
Iter:15/100, MeanErr=1.910940(-0.31%), 40.08M WeightUpdates/sec
Iter:16/100, MeanErr=1.916100(0.27%), 77.94M WeightUpdates/sec
Iter:17/100, MeanErr=1.914411(-0.09%), 105.49M WeightUpdates/sec
Iter:18/100, MeanErr=1.917069(0.14%), 111.70M WeightUpdates/sec
Iter:19/100, MeanErr=1.917084(0.00%), 121.87M WeightUpdates/sec
Iter:20/100, MeanErr=1.917078(0.00%), 100.11M WeightUpdates/sec
Iter:21/100, MeanErr=1.917574(0.03%), 97.58M WeightUpdates/sec
Iter:22/100, MeanErr=1.917019(-0.03%), 83.36M WeightUpdates/sec
Iter:23/100, MeanErr=1.915290(-0.09%), 32.30M WeightUpdates/sec
Iter:24/100, MeanErr=1.916726(0.07%), 71.59M WeightUpdates/sec
Iter:25/100, MeanErr=1.915015(-0.09%), 96.06M WeightUpdates/sec
Iter:26/100, MeanErr=1.911989(-0.16%), 91.24M WeightUpdates/sec
Iter:27/100, MeanErr=1.915882(0.20%), 91.17M WeightUpdates/sec
Iter:28/100, MeanErr=1.916736(0.04%), 80.36M WeightUpdates/sec
Iter:29/100, MeanErr=1.913311(-0.18%), 108.98M WeightUpdates/sec
Iter:30/100, MeanErr=1.913811(0.03%), 112.81M WeightUpdates/sec
Iter:31/100, MeanErr=1.916077(0.12%), 116.39M WeightUpdates/sec
Iter:32/100, MeanErr=1.916517(0.02%), 120.71M WeightUpdates/sec
Iter:33/100, MeanErr=1.914955(-0.08%), 77.35M WeightUpdates/sec
Iter:34/100, MeanErr=1.915425(0.02%), 86.36M WeightUpdates/sec
Iter:35/100, MeanErr=1.915095(-0.02%), 100.95M WeightUpdates/sec
Iter:36/100, MeanErr=1.912926(-0.11%), 92.09M WeightUpdates/sec
Iter:37/100, MeanErr=1.914749(0.10%), 62.23M WeightUpdates/sec
Iter:38/100, MeanErr=1.913462(-0.07%), 73.79M WeightUpdates/sec
Iter:39/100, MeanErr=1.915216(0.09%), 72.41M WeightUpdates/sec
Iter:40/100, MeanErr=1.915082(-0.01%), 83.60M WeightUpdates/sec
Iter:41/100, MeanErr=1.914830(-0.01%), 84.00M WeightUpdates/sec
Iter:42/100, MeanErr=1.913280(-0.08%), 105.05M WeightUpdates/sec
Iter:43/100, MeanErr=1.915441(0.11%), 85.26M WeightUpdates/sec
Iter:44/100, MeanErr=1.913819(-0.08%), 99.98M WeightUpdates/sec
Iter:45/100, MeanErr=1.913428(-0.02%), 25.04M WeightUpdates/sec
Iter:46/100, MeanErr=1.914131(0.04%), 83.42M WeightUpdates/sec
Iter:47/100, MeanErr=1.911981(-0.11%), 90.63M WeightUpdates/sec
Iter:48/100, MeanErr=1.915121(0.16%), 115.91M WeightUpdates/sec
Iter:49/100, MeanErr=1.914237(-0.05%), 85.97M WeightUpdates/sec
Iter:50/100, MeanErr=1.914256(0.00%), 74.27M WeightUpdates/sec
Iter:51/100, MeanErr=1.914513(0.01%), 108.46M WeightUpdates/sec
Iter:52/100, MeanErr=1.913373(-0.06%), 104.14M WeightUpdates/sec
Iter:53/100, MeanErr=1.913108(-0.01%), 102.96M WeightUpdates/sec
Iter:54/100, MeanErr=1.914261(0.06%), 90.05M WeightUpdates/sec
Iter:55/100, MeanErr=1.910450(-0.20%), 99.98M WeightUpdates/sec
Iter:56/100, MeanErr=1.914064(0.19%), 48.23M WeightUpdates/sec
Iter:57/100, MeanErr=1.913597(-0.02%), 86.72M WeightUpdates/sec
Iter:58/100, MeanErr=1.910929(-0.14%), 104.72M WeightUpdates/sec
Iter:59/100, MeanErr=1.912527(0.08%), 110.99M WeightUpdates/sec
Iter:60/100, MeanErr=1.913345(0.04%), 117.10M WeightUpdates/sec
Iter:61/100, MeanErr=1.911330(-0.11%), 119.38M WeightUpdates/sec
Iter:62/100, MeanErr=1.909690(-0.09%), 109.76M WeightUpdates/sec
Iter:63/100, MeanErr=1.912808(0.16%), 94.62M WeightUpdates/sec
Iter:64/100, MeanErr=1.911461(-0.07%), 89.63M WeightUpdates/sec
Iter:65/100, MeanErr=1.912925(0.08%), 101.68M WeightUpdates/sec
Iter:66/100, MeanErr=1.912967(0.00%), 100.11M WeightUpdates/sec
Iter:67/100, MeanErr=1.913066(0.01%), 40.16M WeightUpdates/sec
Iter:68/100, MeanErr=1.912175(-0.05%), 69.68M WeightUpdates/sec
Iter:69/100, MeanErr=1.911986(-0.01%), 77.54M WeightUpdates/sec
Iter:70/100, MeanErr=1.911892(0.00%), 92.20M WeightUpdates/sec
Iter:71/100, MeanErr=1.910784(-0.06%), 91.91M WeightUpdates/sec
Iter:72/100, MeanErr=1.912165(0.07%), 15.90M WeightUpdates/sec
Iter:73/100, MeanErr=1.907908(-0.22%), 71.57M WeightUpdates/sec
Iter:74/100, MeanErr=1.911824(0.21%), 80.76M WeightUpdates/sec
Iter:75/100, MeanErr=1.911102(-0.04%), 81.62M WeightUpdates/sec
Iter:76/100, MeanErr=1.907967(-0.16%), 117.83M WeightUpdates/sec
Iter:77/100, MeanErr=1.911890(0.21%), 96.54M WeightUpdates/sec
Iter:78/100, MeanErr=1.910740(-0.06%), 61.07M WeightUpdates/sec
Iter:79/100, MeanErr=1.910781(0.00%), 84.38M WeightUpdates/sec
Iter:80/100, MeanErr=1.910306(-0.02%), 106.43M WeightUpdates/sec
Iter:81/100, MeanErr=1.910362(0.00%), 85.29M WeightUpdates/sec
Iter:82/100, MeanErr=1.910906(0.03%), 104.33M WeightUpdates/sec
Iter:83/100, MeanErr=1.910384(-0.03%), 52.92M WeightUpdates/sec
Iter:84/100, MeanErr=1.910919(0.03%), 94.81M WeightUpdates/sec
Iter:85/100, MeanErr=1.910843(0.00%), 95.53M WeightUpdates/sec
Iter:86/100, MeanErr=1.909650(-0.06%), 78.61M WeightUpdates/sec
Iter:87/100, MeanErr=1.908350(-0.07%), 62.39M WeightUpdates/sec
Iter:88/100, MeanErr=1.909900(0.08%), 59.74M WeightUpdates/sec
Iter:89/100, MeanErr=1.910736(0.04%), 67.69M WeightUpdates/sec
Iter:90/100, MeanErr=1.909085(-0.09%), 41.67M WeightUpdates/sec
Iter:91/100, MeanErr=1.908917(-0.01%), 70.26M WeightUpdates/sec
Iter:92/100, MeanErr=1.909353(0.02%), 45.31M WeightUpdates/sec
Iter:93/100, MeanErr=1.909247(-0.01%), 70.50M WeightUpdates/sec
Iter:94/100, MeanErr=1.909686(0.02%), 87.83M WeightUpdates/sec
Iter:95/100, MeanErr=1.909142(-0.03%), 103.90M WeightUpdates/sec
Iter:96/100, MeanErr=1.901593(-0.40%), 97.79M WeightUpdates/sec
Iter:97/100, MeanErr=1.908373(0.36%), 43.74M WeightUpdates/sec
Iter:98/100, MeanErr=1.908131(-0.01%), 51.47M WeightUpdates/sec
Iter:99/100, MeanErr=1.909244(0.06%), 23.05M WeightUpdates/sec
Iter:100/100, MeanErr=1.907185(-0.11%), 21.31M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 1.896982
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2953101
Elapsed time: 00:00:00.0182802
Beginning processing data.
Rows Read: 38, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0712023
Finished writing 38 rows.
Writing completed.
Data will be written to C:\Users\xavie\AppData\Local\Temp\rre677679.xdf File will be overwritten if it exists.

Rows Processed: 5 
      Species   Score.0   Score.1   Score.2
0      setosa  0.306292  0.352901  0.340807
1   virginica  0.297068  0.355387  0.347545
2      setosa  0.305993  0.353035  0.340972
3  versicolor  0.301401  0.354414  0.344185
4   virginica  0.298529  0.355090  0.346381
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
Estimated Pre-training MeanError = 4431.587508
Iter:1/100, MeanErr=1732.891296(-60.90%), 25.02M WeightUpdates/sec
Iter:2/100, MeanErr=153.409363(-91.15%), 23.82M WeightUpdates/sec
Iter:3/100, MeanErr=118.063756(-23.04%), 16.82M WeightUpdates/sec
Iter:4/100, MeanErr=117.111723(-0.81%), 11.16M WeightUpdates/sec
Iter:5/100, MeanErr=113.286739(-3.27%), 20.39M WeightUpdates/sec
Iter:6/100, MeanErr=114.052654(0.68%), 23.21M WeightUpdates/sec
Iter:7/100, MeanErr=113.830717(-0.19%), 23.98M WeightUpdates/sec
Iter:8/100, MeanErr=111.569165(-1.99%), 23.75M WeightUpdates/sec
Iter:9/100, MeanErr=112.852787(1.15%), 26.49M WeightUpdates/sec
Iter:10/100, MeanErr=113.097143(0.22%), 28.04M WeightUpdates/sec
Iter:11/100, MeanErr=110.464180(-2.33%), 30.49M WeightUpdates/sec
Iter:12/100, MeanErr=111.615003(1.04%), 28.95M WeightUpdates/sec
Iter:13/100, MeanErr=107.387412(-3.79%), 29.68M WeightUpdates/sec
Iter:14/100, MeanErr=111.018414(3.38%), 35.97M WeightUpdates/sec
Iter:15/100, MeanErr=110.918491(-0.09%), 14.94M WeightUpdates/sec
Iter:16/100, MeanErr=105.959554(-4.47%), 28.32M WeightUpdates/sec
Iter:17/100, MeanErr=105.417626(-0.51%), 36.26M WeightUpdates/sec
Iter:18/100, MeanErr=101.722564(-3.51%), 34.58M WeightUpdates/sec
Iter:19/100, MeanErr=104.439772(2.67%), 32.85M WeightUpdates/sec
Iter:20/100, MeanErr=103.860748(-0.55%), 34.18M WeightUpdates/sec
Iter:21/100, MeanErr=98.501875(-5.16%), 37.45M WeightUpdates/sec
Iter:22/100, MeanErr=105.877514(7.49%), 32.58M WeightUpdates/sec
Iter:23/100, MeanErr=105.192747(-0.65%), 30.87M WeightUpdates/sec
Iter:24/100, MeanErr=101.231016(-3.77%), 31.32M WeightUpdates/sec
Iter:25/100, MeanErr=104.191897(2.92%), 31.75M WeightUpdates/sec
Iter:26/100, MeanErr=96.809382(-7.09%), 34.52M WeightUpdates/sec
Iter:27/100, MeanErr=102.712610(6.10%), 34.55M WeightUpdates/sec
Iter:28/100, MeanErr=100.143548(-2.50%), 34.18M WeightUpdates/sec
Iter:29/100, MeanErr=102.151606(2.01%), 37.99M WeightUpdates/sec
Iter:30/100, MeanErr=99.446707(-2.65%), 32.97M WeightUpdates/sec
Iter:31/100, MeanErr=99.512111(0.07%), 32.42M WeightUpdates/sec
Iter:32/100, MeanErr=90.989352(-8.56%), 33.56M WeightUpdates/sec
Iter:33/100, MeanErr=100.555054(10.51%), 34.77M WeightUpdates/sec
Iter:34/100, MeanErr=94.510907(-6.01%), 36.47M WeightUpdates/sec
Iter:35/100, MeanErr=98.095334(3.79%), 34.05M WeightUpdates/sec
Iter:36/100, MeanErr=87.772617(-10.52%), 33.69M WeightUpdates/sec
Iter:37/100, MeanErr=100.431796(14.42%), 37.36M WeightUpdates/sec
Iter:38/100, MeanErr=98.025471(-2.40%), 35.40M WeightUpdates/sec
Iter:39/100, MeanErr=93.616667(-4.50%), 31.95M WeightUpdates/sec
Iter:40/100, MeanErr=95.567692(2.08%), 35.12M WeightUpdates/sec
Iter:41/100, MeanErr=93.324832(-2.35%), 35.57M WeightUpdates/sec
Iter:42/100, MeanErr=88.208258(-5.48%), 32.75M WeightUpdates/sec
Iter:43/100, MeanErr=94.455553(7.08%), 34.15M WeightUpdates/sec
Iter:44/100, MeanErr=90.753527(-3.92%), 34.71M WeightUpdates/sec
Iter:45/100, MeanErr=93.307899(2.81%), 34.88M WeightUpdates/sec
Iter:46/100, MeanErr=93.747801(0.47%), 32.32M WeightUpdates/sec
Iter:47/100, MeanErr=93.289460(-0.49%), 32.85M WeightUpdates/sec
Iter:48/100, MeanErr=87.644454(-6.05%), 34.13M WeightUpdates/sec
Iter:49/100, MeanErr=88.007132(0.41%), 33.24M WeightUpdates/sec
Iter:50/100, MeanErr=92.459351(5.06%), 5.18M WeightUpdates/sec
Iter:51/100, MeanErr=89.934175(-2.73%), 4.39M WeightUpdates/sec
Iter:52/100, MeanErr=90.304940(0.41%), 28.14M WeightUpdates/sec
Iter:53/100, MeanErr=83.077270(-8.00%), 31.75M WeightUpdates/sec
Iter:54/100, MeanErr=91.006037(9.54%), 26.63M WeightUpdates/sec
Iter:55/100, MeanErr=87.371337(-3.99%), 27.79M WeightUpdates/sec
Iter:56/100, MeanErr=86.584348(-0.90%), 32.21M WeightUpdates/sec
Iter:57/100, MeanErr=88.972126(2.76%), 33.21M WeightUpdates/sec
Iter:58/100, MeanErr=86.331117(-2.97%), 35.23M WeightUpdates/sec
Iter:59/100, MeanErr=87.222200(1.03%), 33.26M WeightUpdates/sec
Iter:60/100, MeanErr=86.409502(-0.93%), 32.32M WeightUpdates/sec
Iter:61/100, MeanErr=85.757669(-0.75%), 37.14M WeightUpdates/sec
Iter:62/100, MeanErr=86.520757(0.89%), 35.89M WeightUpdates/sec
Iter:63/100, MeanErr=83.760556(-3.19%), 14.41M WeightUpdates/sec
Iter:64/100, MeanErr=86.364208(3.11%), 31.08M WeightUpdates/sec
Iter:65/100, MeanErr=83.941776(-2.80%), 32.65M WeightUpdates/sec
Iter:66/100, MeanErr=84.440861(0.59%), 25.81M WeightUpdates/sec
Iter:67/100, MeanErr=81.211987(-3.82%), 31.00M WeightUpdates/sec
Iter:68/100, MeanErr=83.414775(2.71%), 34.60M WeightUpdates/sec
Iter:69/100, MeanErr=78.628970(-5.74%), 32.77M WeightUpdates/sec
Iter:70/100, MeanErr=78.758947(0.17%), 35.51M WeightUpdates/sec
Iter:71/100, MeanErr=80.565879(2.29%), 33.41M WeightUpdates/sec
Iter:72/100, MeanErr=79.998539(-0.70%), 34.74M WeightUpdates/sec
Iter:73/100, MeanErr=80.395648(0.50%), 36.38M WeightUpdates/sec
Iter:74/100, MeanErr=78.498790(-2.36%), 24.73M WeightUpdates/sec
Iter:75/100, MeanErr=79.603451(1.41%), 10.18M WeightUpdates/sec
Iter:76/100, MeanErr=80.082897(0.60%), 26.79M WeightUpdates/sec
Iter:77/100, MeanErr=78.035992(-2.56%), 25.34M WeightUpdates/sec
Iter:78/100, MeanErr=74.663794(-4.32%), 29.66M WeightUpdates/sec
Iter:79/100, MeanErr=77.159675(3.34%), 32.99M WeightUpdates/sec
Iter:80/100, MeanErr=76.891332(-0.35%), 36.47M WeightUpdates/sec
Iter:81/100, MeanErr=77.547883(0.85%), 35.23M WeightUpdates/sec
Iter:82/100, MeanErr=75.253815(-2.96%), 34.05M WeightUpdates/sec
Iter:83/100, MeanErr=72.597325(-3.53%), 33.09M WeightUpdates/sec
Iter:84/100, MeanErr=77.191398(6.33%), 31.06M WeightUpdates/sec
Iter:85/100, MeanErr=75.357856(-2.38%), 36.68M WeightUpdates/sec
Iter:86/100, MeanErr=75.371998(0.02%), 32.94M WeightUpdates/sec
Iter:87/100, MeanErr=73.502589(-2.48%), 12.62M WeightUpdates/sec
Iter:88/100, MeanErr=70.027318(-4.73%), 30.81M WeightUpdates/sec
Iter:89/100, MeanErr=74.921495(6.99%), 29.64M WeightUpdates/sec
Iter:90/100, MeanErr=72.224062(-3.60%), 24.50M WeightUpdates/sec
Iter:91/100, MeanErr=69.426291(-3.87%), 21.87M WeightUpdates/sec
Iter:92/100, MeanErr=72.133984(3.90%), 28.29M WeightUpdates/sec
Iter:93/100, MeanErr=71.271876(-1.20%), 20.05M WeightUpdates/sec
Iter:94/100, MeanErr=71.023653(-0.35%), 28.70M WeightUpdates/sec
Iter:95/100, MeanErr=67.928765(-4.36%), 32.80M WeightUpdates/sec
Iter:96/100, MeanErr=68.846256(1.35%), 35.18M WeightUpdates/sec
Iter:97/100, MeanErr=68.488290(-0.52%), 30.59M WeightUpdates/sec
Iter:98/100, MeanErr=67.395029(-1.60%), 26.66M WeightUpdates/sec
Iter:99/100, MeanErr=68.411513(1.51%), 12.16M WeightUpdates/sec
Iter:100/100, MeanErr=67.952709(-0.67%), 30.20M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 61.751150
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.1785694
Elapsed time: 00:00:00.0150234
Beginning processing data.
Rows Read: 8, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0648070
Finished writing 8 rows.
Writing completed.
Data will be written to C:\Users\xavie\AppData\Local\Temp\rre6776810.xdf File will be overwritten if it exists.

Rows Processed: 5 
   rating      Score
0    43.0  56.830418
1    71.0  67.153664
2    66.0  68.607536
3    82.0  67.106064
4    72.0  70.539070
```



### optimizers

* [*adadelta_optimizer*: Adaptive learing rate method](adadelta_optimizer.md) 

* [*sgd_optimizer*: Stochastic Gradient Descent](sgd_optimizer.md) 


### math

* [*avx_math*](avx_math.md) 

* [*clr_math*](clr_math.md) 

* [*gpu_math*](gpu_math.md) 

* [*mkl_math*](mkl_math.md) 

* [*sse_math*](sse_math.md) 
