--- 
 
# required metadata 
title: "Neural Net" 
description: "Neural networks for regression modeling and for Binary and multi-class classification." 
keywords: "models, classification, regression, neural network, dnn" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/12/2017" 
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


**Applies to: SQL Server 2017, Machine Learning Services 9.3**

* [optimizers](optimizers.md) 

* [math](math.md) 


## Usage



```
microsoftml.rx_neural_network(formula: str, data: [<class ‘revoscalepy.datasource.RxDataSource.RxDataSource’>, <class ‘pandas.core.frame.DataFrame’>], method: [‘binary’, ‘multiClass’, ‘regression’] = ‘binary’, num_hidden_nodes: int = 100, num_iterations: int = 100, optimizer: [<function adadelta_optimizer at 0x000001E4F8F67400>, <function sgd_optimizer at 0x000001E4F8F67268>] = {‘name’: ‘SgdOptimizer’, ‘settings’: {}}, net_definition: str = None, init_wts_diameter: float = 0.1, max_norm: float = 0, acceleration: [<function avx_math at 0x000001E4F8F67598>, <function clr_math at 0x000001E4F8F67840>, <function gpu_math at 0x000001E4F8F678C8>, <function mkl_math at 0x000001E4F8F67950>, <function sse_math at 0x000001E4F8F679D8>] = {‘name’: ‘AvxMath’, ‘settings’: {}}, mini_batch_size: int = 1, normalize: [‘No’, ‘Warn’, ‘Auto’, ‘Yes’] = ‘Auto’, ml_transforms: list = None, ml_transform_vars: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment: dict = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, ensemble: dict = None, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
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

The formula as described in [revoscalepy.rx_formula](/python-reference/revoscale.py/api/rx_formula.md).
Interaction terms and `F()` are not currently supported in
[microsoftml](./microsoftml/index.md).


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
to be performed. See [`featurize_text`](featurize_text.md),
[`categorical`](categorical.md),
and [`categorical_hash`](categorical_hash.md), for transformations that are supported.
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
For example, those explicitly defined in [revoscalepy](/python-reference/revoscale.py/index.md) functions via
their `transforms` and `transform_function` arguments or those defined
implicitly via their `formula` or `row_selection` arguments.  The
`transform_packages` argument may also be *None*, indicating that
no packages outside `RxOptions.get_option("transform_packages")` are preloaded.


### transform_environment

NOT SUPPORTED. A user-defined environment to serve as a parent to all
environments developed internally and used for variable data transformation.
If `transform_environment = None`, a new “hash” environment with parent
[revoscalepy.baseenv](/python-reference/revoscale.py/api/baseenv.md) is used instead.


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
specified with a valid [revoscalepy.RxComputeContext](/python-reference/revoscale.py/api/RxComputeContext.md).
Currently local and [revoscalepy.RxInSqlServer](/python-reference/revoscale.py/api/RxInSqlServer.md) compute contexts
are supported.


### ensemble

NOT SUPPORTED. Control parameters for ensembling.


## Returns

A [`NeuralNetwork`](learners_object.md) object with the trained model.


## Note

This algorithm is single-threaded and will not attempt to load the entire dataset into
memory.


## See also

[`adadelta_optimizer`](adadelta_optimizer.md),
[`sgd_optimizer`](sgd_optimizer.md),
[`avx_math`](avx_math.md),
[`clr_math`](clr_math.md),
[`gpu_math`](gpu_math.md),
[`mkl_math`](mkl_math.md),
[`sse_math`](sse_math.md),
[`rx_predict`](rx_predict.md).


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
Rows Read: 186, Read Time: 0, Transform Time: 0
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
Estimated Pre-training MeanError = 0.741068
Iter:1/100, MeanErr=0.675086(-8.90%), 89.32M WeightUpdates/sec
Iter:2/100, MeanErr=0.641967(-4.91%), 89.66M WeightUpdates/sec
Iter:3/100, MeanErr=0.639073(-0.45%), 98.67M WeightUpdates/sec
Iter:4/100, MeanErr=0.638535(-0.08%), 132.76M WeightUpdates/sec
Iter:5/100, MeanErr=0.639074(0.08%), 143.56M WeightUpdates/sec
Iter:6/100, MeanErr=0.639051(0.00%), 148.00M WeightUpdates/sec
Iter:7/100, MeanErr=0.638842(-0.03%), 73.37M WeightUpdates/sec
Iter:8/100, MeanErr=0.638976(0.02%), 143.68M WeightUpdates/sec
Iter:9/100, MeanErr=0.637778(-0.19%), 141.77M WeightUpdates/sec
Iter:10/100, MeanErr=0.639203(0.22%), 140.44M WeightUpdates/sec
Iter:11/100, MeanErr=0.638963(-0.04%), 148.47M WeightUpdates/sec
Iter:12/100, MeanErr=0.638998(0.01%), 161.07M WeightUpdates/sec
Iter:13/100, MeanErr=0.638815(-0.03%), 163.63M WeightUpdates/sec
Iter:14/100, MeanErr=0.638672(-0.02%), 151.74M WeightUpdates/sec
Iter:15/100, MeanErr=0.639144(0.07%), 94.06M WeightUpdates/sec
Iter:16/100, MeanErr=0.639084(-0.01%), 110.38M WeightUpdates/sec
Iter:17/100, MeanErr=0.638049(-0.16%), 124.17M WeightUpdates/sec
Iter:18/100, MeanErr=0.638564(0.08%), 136.10M WeightUpdates/sec
Iter:19/100, MeanErr=0.639069(0.08%), 157.76M WeightUpdates/sec
Iter:20/100, MeanErr=0.638205(-0.14%), 140.03M WeightUpdates/sec
Iter:21/100, MeanErr=0.638933(0.11%), 105.11M WeightUpdates/sec
Iter:22/100, MeanErr=0.638247(-0.11%), 137.98M WeightUpdates/sec
Iter:23/100, MeanErr=0.638819(0.09%), 158.29M WeightUpdates/sec
Iter:24/100, MeanErr=0.637532(-0.20%), 102.62M WeightUpdates/sec
Iter:25/100, MeanErr=0.639163(0.26%), 148.33M WeightUpdates/sec
Iter:26/100, MeanErr=0.637936(-0.19%), 132.97M WeightUpdates/sec
Iter:27/100, MeanErr=0.638846(0.14%), 145.14M WeightUpdates/sec
Iter:28/100, MeanErr=0.638658(-0.03%), 162.82M WeightUpdates/sec
Iter:29/100, MeanErr=0.638190(-0.07%), 157.09M WeightUpdates/sec
Iter:30/100, MeanErr=0.639191(0.16%), 121.29M WeightUpdates/sec
Iter:31/100, MeanErr=0.639014(-0.03%), 157.61M WeightUpdates/sec
Iter:32/100, MeanErr=0.637660(-0.21%), 158.22M WeightUpdates/sec
Iter:33/100, MeanErr=0.638864(0.19%), 107.30M WeightUpdates/sec
Iter:34/100, MeanErr=0.638515(-0.05%), 100.07M WeightUpdates/sec
Iter:35/100, MeanErr=0.638902(0.06%), 122.83M WeightUpdates/sec
Iter:36/100, MeanErr=0.638714(-0.03%), 152.23M WeightUpdates/sec
Iter:37/100, MeanErr=0.638779(0.01%), 98.96M WeightUpdates/sec
Iter:38/100, MeanErr=0.638711(-0.01%), 131.59M WeightUpdates/sec
Iter:39/100, MeanErr=0.638632(-0.01%), 153.36M WeightUpdates/sec
Iter:40/100, MeanErr=0.637275(-0.21%), 163.39M WeightUpdates/sec
Iter:41/100, MeanErr=0.638927(0.26%), 166.60M WeightUpdates/sec
Iter:42/100, MeanErr=0.638372(-0.09%), 163.06M WeightUpdates/sec
Iter:43/100, MeanErr=0.638626(0.04%), 135.32M WeightUpdates/sec
Iter:44/100, MeanErr=0.638431(-0.03%), 162.82M WeightUpdates/sec
Iter:45/100, MeanErr=0.638782(0.05%), 157.99M WeightUpdates/sec
Iter:46/100, MeanErr=0.638654(-0.02%), 159.52M WeightUpdates/sec
Iter:47/100, MeanErr=0.638738(0.01%), 150.22M WeightUpdates/sec
Iter:48/100, MeanErr=0.638697(-0.01%), 158.22M WeightUpdates/sec
Iter:49/100, MeanErr=0.638123(-0.09%), 158.75M WeightUpdates/sec
Iter:50/100, MeanErr=0.638128(0.00%), 79.66M WeightUpdates/sec
Iter:51/100, MeanErr=0.639071(0.15%), 121.56M WeightUpdates/sec
Iter:52/100, MeanErr=0.638232(-0.13%), 160.14M WeightUpdates/sec
Iter:53/100, MeanErr=0.638757(0.08%), 158.22M WeightUpdates/sec
Iter:54/100, MeanErr=0.638683(-0.01%), 152.30M WeightUpdates/sec
Iter:55/100, MeanErr=0.638711(0.00%), 161.47M WeightUpdates/sec
Iter:56/100, MeanErr=0.638555(-0.02%), 138.67M WeightUpdates/sec
Iter:57/100, MeanErr=0.638860(0.05%), 142.44M WeightUpdates/sec
Iter:58/100, MeanErr=0.638727(-0.02%), 145.65M WeightUpdates/sec
Iter:59/100, MeanErr=0.638734(0.00%), 139.37M WeightUpdates/sec
Iter:60/100, MeanErr=0.638653(-0.01%), 155.90M WeightUpdates/sec
Iter:61/100, MeanErr=0.638590(-0.01%), 162.74M WeightUpdates/sec
Iter:62/100, MeanErr=0.638272(-0.05%), 121.38M WeightUpdates/sec
Iter:63/100, MeanErr=0.638733(0.07%), 95.03M WeightUpdates/sec
Iter:64/100, MeanErr=0.638028(-0.11%), 157.24M WeightUpdates/sec
Iter:65/100, MeanErr=0.638917(0.14%), 157.84M WeightUpdates/sec
Iter:66/100, MeanErr=0.638548(-0.06%), 129.12M WeightUpdates/sec
Iter:67/100, MeanErr=0.638692(0.02%), 161.94M WeightUpdates/sec
Iter:68/100, MeanErr=0.638349(-0.05%), 155.31M WeightUpdates/sec
Iter:69/100, MeanErr=0.637284(-0.17%), 155.90M WeightUpdates/sec
Iter:70/100, MeanErr=0.637946(0.10%), 165.77M WeightUpdates/sec
Iter:71/100, MeanErr=0.638614(0.10%), 169.77M WeightUpdates/sec
Iter:72/100, MeanErr=0.638680(0.01%), 142.20M WeightUpdates/sec
Iter:73/100, MeanErr=0.638575(-0.02%), 157.84M WeightUpdates/sec
Iter:74/100, MeanErr=0.638028(-0.09%), 141.77M WeightUpdates/sec
Iter:75/100, MeanErr=0.637893(-0.02%), 163.39M WeightUpdates/sec
Iter:76/100, MeanErr=0.637792(-0.02%), 106.26M WeightUpdates/sec
Iter:77/100, MeanErr=0.638765(0.15%), 160.68M WeightUpdates/sec
Iter:78/100, MeanErr=0.638592(-0.03%), 165.93M WeightUpdates/sec
Iter:79/100, MeanErr=0.638657(0.01%), 160.29M WeightUpdates/sec
Iter:80/100, MeanErr=0.637643(-0.16%), 145.46M WeightUpdates/sec
Iter:81/100, MeanErr=0.638455(0.13%), 141.04M WeightUpdates/sec
Iter:82/100, MeanErr=0.638318(-0.02%), 145.01M WeightUpdates/sec
Iter:83/100, MeanErr=0.638152(-0.03%), 153.15M WeightUpdates/sec
Iter:84/100, MeanErr=0.638468(0.05%), 164.69M WeightUpdates/sec
Iter:85/100, MeanErr=0.638530(0.01%), 159.52M WeightUpdates/sec
Iter:86/100, MeanErr=0.638116(-0.06%), 158.44M WeightUpdates/sec
Iter:87/100, MeanErr=0.638095(0.00%), 166.77M WeightUpdates/sec
Iter:88/100, MeanErr=0.637885(-0.03%), 159.52M WeightUpdates/sec
Iter:89/100, MeanErr=0.637559(-0.05%), 106.22M WeightUpdates/sec
Iter:90/100, MeanErr=0.637849(0.05%), 140.50M WeightUpdates/sec
Iter:91/100, MeanErr=0.637964(0.02%), 156.27M WeightUpdates/sec
Iter:92/100, MeanErr=0.638496(0.08%), 158.60M WeightUpdates/sec
Iter:93/100, MeanErr=0.638161(-0.05%), 147.60M WeightUpdates/sec
Iter:94/100, MeanErr=0.638388(0.04%), 153.36M WeightUpdates/sec
Iter:95/100, MeanErr=0.637560(-0.13%), 158.06M WeightUpdates/sec
Iter:96/100, MeanErr=0.638496(0.15%), 161.70M WeightUpdates/sec
Iter:97/100, MeanErr=0.638412(-0.01%), 158.98M WeightUpdates/sec
Iter:98/100, MeanErr=0.638309(-0.02%), 166.27M WeightUpdates/sec
Iter:99/100, MeanErr=0.637145(-0.18%), 167.70M WeightUpdates/sec
Iter:100/100, MeanErr=0.638677(0.24%), 142.44M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 0.635816
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2147768
Elapsed time: 00:00:00.0165917
Beginning processing data.
Rows Read: 62, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0705864
Finished writing 62 rows.
Writing completed.
Data will be written to C:\Users\xavie\AppData\Local\Temp\rre698468.xdf File will be overwritten if it exists.

Rows Processed: 5 
  isCase PredictedLabel     Score  Probability
0  False          False -0.650996     0.342764
1   True          False -0.643079     0.344550
2  False          False -0.658959     0.340973
3  False          False -0.656342     0.341561
4   True          False -0.650189     0.342946
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
Estimated Pre-training MeanError = 1.963604
Iter:1/100, MeanErr=1.942966(-1.05%), 100.46M WeightUpdates/sec
Iter:2/100, MeanErr=1.919654(-1.20%), 101.81M WeightUpdates/sec
Iter:3/100, MeanErr=1.915278(-0.23%), 121.80M WeightUpdates/sec
Iter:4/100, MeanErr=1.916975(0.09%), 98.81M WeightUpdates/sec
Iter:5/100, MeanErr=1.914570(-0.13%), 102.50M WeightUpdates/sec
Iter:6/100, MeanErr=1.915704(0.06%), 129.49M WeightUpdates/sec
Iter:7/100, MeanErr=1.914280(-0.07%), 83.02M WeightUpdates/sec
Iter:8/100, MeanErr=1.916170(0.10%), 88.10M WeightUpdates/sec
Iter:9/100, MeanErr=1.916023(-0.01%), 106.78M WeightUpdates/sec
Iter:10/100, MeanErr=1.916018(0.00%), 119.56M WeightUpdates/sec
Iter:11/100, MeanErr=1.914976(-0.05%), 97.16M WeightUpdates/sec
Iter:12/100, MeanErr=1.914292(-0.04%), 81.60M WeightUpdates/sec
Iter:13/100, MeanErr=1.914932(0.03%), 88.58M WeightUpdates/sec
Iter:14/100, MeanErr=1.915664(0.04%), 113.26M WeightUpdates/sec
Iter:15/100, MeanErr=1.914882(-0.04%), 118.63M WeightUpdates/sec
Iter:16/100, MeanErr=1.914701(-0.01%), 93.84M WeightUpdates/sec
Iter:17/100, MeanErr=1.913635(-0.06%), 103.52M WeightUpdates/sec
Iter:18/100, MeanErr=1.915080(0.08%), 94.07M WeightUpdates/sec
Iter:19/100, MeanErr=1.914811(-0.01%), 71.36M WeightUpdates/sec
Iter:20/100, MeanErr=1.914959(0.01%), 95.01M WeightUpdates/sec
Iter:21/100, MeanErr=1.915282(0.02%), 95.13M WeightUpdates/sec
Iter:22/100, MeanErr=1.914395(-0.05%), 87.12M WeightUpdates/sec
Iter:23/100, MeanErr=1.914474(0.00%), 60.11M WeightUpdates/sec
Iter:24/100, MeanErr=1.913641(-0.04%), 77.30M WeightUpdates/sec
Iter:25/100, MeanErr=1.914433(0.04%), 90.66M WeightUpdates/sec
Iter:26/100, MeanErr=1.914019(-0.02%), 90.70M WeightUpdates/sec
Iter:27/100, MeanErr=1.912635(-0.07%), 46.50M WeightUpdates/sec
Iter:28/100, MeanErr=1.913866(0.06%), 99.54M WeightUpdates/sec
Iter:29/100, MeanErr=1.911985(-0.10%), 108.92M WeightUpdates/sec
Iter:30/100, MeanErr=1.914238(0.12%), 114.28M WeightUpdates/sec
Iter:31/100, MeanErr=1.912795(-0.08%), 102.09M WeightUpdates/sec
Iter:32/100, MeanErr=1.908765(-0.21%), 101.90M WeightUpdates/sec
Iter:33/100, MeanErr=1.914297(0.29%), 111.32M WeightUpdates/sec
Iter:34/100, MeanErr=1.912574(-0.09%), 108.77M WeightUpdates/sec
Iter:35/100, MeanErr=1.907379(-0.27%), 86.69M WeightUpdates/sec
Iter:36/100, MeanErr=1.914644(0.38%), 91.65M WeightUpdates/sec
Iter:37/100, MeanErr=1.913310(-0.07%), 70.92M WeightUpdates/sec
Iter:38/100, MeanErr=1.911134(-0.11%), 50.11M WeightUpdates/sec
Iter:39/100, MeanErr=1.913248(0.11%), 99.11M WeightUpdates/sec
Iter:40/100, MeanErr=1.912588(-0.03%), 97.70M WeightUpdates/sec
Iter:41/100, MeanErr=1.910880(-0.09%), 93.30M WeightUpdates/sec
Iter:42/100, MeanErr=1.912091(0.06%), 82.72M WeightUpdates/sec
Iter:43/100, MeanErr=1.911829(-0.01%), 96.67M WeightUpdates/sec
Iter:44/100, MeanErr=1.912921(0.06%), 109.18M WeightUpdates/sec
Iter:45/100, MeanErr=1.912595(-0.02%), 90.48M WeightUpdates/sec
Iter:46/100, MeanErr=1.910989(-0.08%), 102.04M WeightUpdates/sec
Iter:47/100, MeanErr=1.912666(0.09%), 103.52M WeightUpdates/sec
Iter:48/100, MeanErr=1.911434(-0.06%), 104.81M WeightUpdates/sec
Iter:49/100, MeanErr=1.911350(0.00%), 48.48M WeightUpdates/sec
Iter:50/100, MeanErr=1.912006(0.03%), 57.72M WeightUpdates/sec
Iter:51/100, MeanErr=1.911754(-0.01%), 92.62M WeightUpdates/sec
Iter:52/100, MeanErr=1.908763(-0.16%), 87.06M WeightUpdates/sec
Iter:53/100, MeanErr=1.907036(-0.09%), 94.42M WeightUpdates/sec
Iter:54/100, MeanErr=1.910449(0.18%), 87.59M WeightUpdates/sec
Iter:55/100, MeanErr=1.911944(0.08%), 98.00M WeightUpdates/sec
Iter:56/100, MeanErr=1.910528(-0.07%), 93.26M WeightUpdates/sec
Iter:57/100, MeanErr=1.907157(-0.18%), 94.34M WeightUpdates/sec
Iter:58/100, MeanErr=1.910724(0.19%), 93.07M WeightUpdates/sec
Iter:59/100, MeanErr=1.910273(-0.02%), 93.68M WeightUpdates/sec
Iter:60/100, MeanErr=1.909947(-0.02%), 50.17M WeightUpdates/sec
Iter:61/100, MeanErr=1.910150(0.01%), 108.30M WeightUpdates/sec
Iter:62/100, MeanErr=1.910207(0.00%), 125.91M WeightUpdates/sec
Iter:63/100, MeanErr=1.910276(0.00%), 109.18M WeightUpdates/sec
Iter:64/100, MeanErr=1.908094(-0.11%), 100.82M WeightUpdates/sec
Iter:65/100, MeanErr=1.910788(0.14%), 95.01M WeightUpdates/sec
Iter:66/100, MeanErr=1.908934(-0.10%), 108.41M WeightUpdates/sec
Iter:67/100, MeanErr=1.910455(0.08%), 116.45M WeightUpdates/sec
Iter:68/100, MeanErr=1.908379(-0.11%), 125.50M WeightUpdates/sec
Iter:69/100, MeanErr=1.907360(-0.05%), 133.68M WeightUpdates/sec
Iter:70/100, MeanErr=1.903606(-0.20%), 122.99M WeightUpdates/sec
Iter:71/100, MeanErr=1.906989(0.18%), 73.18M WeightUpdates/sec
Iter:72/100, MeanErr=1.908466(0.08%), 90.02M WeightUpdates/sec
Iter:73/100, MeanErr=1.908259(-0.01%), 118.44M WeightUpdates/sec
Iter:74/100, MeanErr=1.908658(0.02%), 110.51M WeightUpdates/sec
Iter:75/100, MeanErr=1.907479(-0.06%), 125.43M WeightUpdates/sec
Iter:76/100, MeanErr=1.907631(0.01%), 124.13M WeightUpdates/sec
Iter:77/100, MeanErr=1.903713(-0.21%), 129.72M WeightUpdates/sec
Iter:78/100, MeanErr=1.907253(0.19%), 124.20M WeightUpdates/sec
Iter:79/100, MeanErr=1.908152(0.05%), 118.08M WeightUpdates/sec
Iter:80/100, MeanErr=1.907401(-0.04%), 105.20M WeightUpdates/sec
Iter:81/100, MeanErr=1.907477(0.00%), 124.40M WeightUpdates/sec
Iter:82/100, MeanErr=1.904047(-0.18%), 113.71M WeightUpdates/sec
Iter:83/100, MeanErr=1.907634(0.19%), 52.72M WeightUpdates/sec
Iter:84/100, MeanErr=1.906481(-0.06%), 111.05M WeightUpdates/sec
Iter:85/100, MeanErr=1.905955(-0.03%), 123.25M WeightUpdates/sec
Iter:86/100, MeanErr=1.905156(-0.04%), 126.68M WeightUpdates/sec
Iter:87/100, MeanErr=1.905950(0.04%), 116.21M WeightUpdates/sec
Iter:88/100, MeanErr=1.906250(0.02%), 118.14M WeightUpdates/sec
Iter:89/100, MeanErr=1.906022(-0.01%), 131.14M WeightUpdates/sec
Iter:90/100, MeanErr=1.905287(-0.04%), 129.94M WeightUpdates/sec
Iter:91/100, MeanErr=1.904423(-0.05%), 133.21M WeightUpdates/sec
Iter:92/100, MeanErr=1.904820(0.02%), 93.45M WeightUpdates/sec
Iter:93/100, MeanErr=1.904056(-0.04%), 85.42M WeightUpdates/sec
Iter:94/100, MeanErr=1.905394(0.07%), 51.52M WeightUpdates/sec
Iter:95/100, MeanErr=1.901856(-0.19%), 113.09M WeightUpdates/sec
Iter:96/100, MeanErr=1.905337(0.18%), 125.70M WeightUpdates/sec
Iter:97/100, MeanErr=1.904292(-0.05%), 136.48M WeightUpdates/sec
Iter:98/100, MeanErr=1.903845(-0.02%), 125.22M WeightUpdates/sec
Iter:99/100, MeanErr=1.902412(-0.08%), 131.14M WeightUpdates/sec
Iter:100/100, MeanErr=1.903518(0.06%), 122.00M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 1.892265
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.2616145
Elapsed time: 00:00:00.0180637
Beginning processing data.
Rows Read: 38, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0705425
Finished writing 38 rows.
Writing completed.
Data will be written to C:\Users\xavie\AppData\Local\Temp\rre698479.xdf File will be overwritten if it exists.

Rows Processed: 5 
      Species   Score.0   Score.1   Score.2
0   virginica  0.367232  0.318087  0.314681
1  versicolor  0.370167  0.317127  0.312706
2  versicolor  0.370468  0.316988  0.312544
3   virginica  0.367870  0.317836  0.314294
4  versicolor  0.370814  0.316888  0.312298
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
Rows Read: 22, Read Time: 0.001, Transform Time: 0
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
Estimated Pre-training MeanError = 4288.548162
Iter:1/100, MeanErr=1625.811185(-62.09%), 23.30M WeightUpdates/sec
Iter:2/100, MeanErr=154.817728(-90.48%), 28.59M WeightUpdates/sec
Iter:3/100, MeanErr=152.220261(-1.68%), 33.64M WeightUpdates/sec
Iter:4/100, MeanErr=146.780356(-3.57%), 32.21M WeightUpdates/sec
Iter:5/100, MeanErr=145.416835(-0.93%), 36.35M WeightUpdates/sec
Iter:6/100, MeanErr=137.882895(-5.18%), 34.02M WeightUpdates/sec
Iter:7/100, MeanErr=143.762667(4.26%), 34.47M WeightUpdates/sec
Iter:8/100, MeanErr=144.958921(0.83%), 33.36M WeightUpdates/sec
Iter:9/100, MeanErr=143.307255(-1.14%), 34.08M WeightUpdates/sec
Iter:10/100, MeanErr=143.383959(0.05%), 30.45M WeightUpdates/sec
Iter:11/100, MeanErr=139.787088(-2.51%), 31.35M WeightUpdates/sec
Iter:12/100, MeanErr=138.261868(-1.09%), 30.78M WeightUpdates/sec
Iter:13/100, MeanErr=139.708136(1.05%), 3.99M WeightUpdates/sec
Iter:14/100, MeanErr=136.103930(-2.58%), 23.53M WeightUpdates/sec
Iter:15/100, MeanErr=139.017129(2.14%), 31.57M WeightUpdates/sec
Iter:16/100, MeanErr=132.672702(-4.56%), 32.99M WeightUpdates/sec
Iter:17/100, MeanErr=135.132319(1.85%), 36.35M WeightUpdates/sec
Iter:18/100, MeanErr=135.703286(0.42%), 24.32M WeightUpdates/sec
Iter:19/100, MeanErr=134.216810(-1.10%), 22.11M WeightUpdates/sec
Iter:20/100, MeanErr=134.828383(0.46%), 24.30M WeightUpdates/sec
Iter:21/100, MeanErr=124.433529(-7.71%), 26.81M WeightUpdates/sec
Iter:22/100, MeanErr=131.697046(5.84%), 31.46M WeightUpdates/sec
Iter:23/100, MeanErr=128.239385(-2.63%), 22.77M WeightUpdates/sec
Iter:24/100, MeanErr=129.215567(0.76%), 33.95M WeightUpdates/sec
Iter:25/100, MeanErr=125.166020(-3.13%), 33.11M WeightUpdates/sec
Iter:26/100, MeanErr=128.155367(2.39%), 35.32M WeightUpdates/sec
Iter:27/100, MeanErr=126.571742(-1.24%), 31.73M WeightUpdates/sec
Iter:28/100, MeanErr=127.531848(0.76%), 31.13M WeightUpdates/sec
Iter:29/100, MeanErr=123.789253(-2.93%), 35.09M WeightUpdates/sec
Iter:30/100, MeanErr=122.659480(-0.91%), 35.07M WeightUpdates/sec
Iter:31/100, MeanErr=120.822289(-1.50%), 33.66M WeightUpdates/sec
Iter:32/100, MeanErr=123.584365(2.29%), 33.36M WeightUpdates/sec
Iter:33/100, MeanErr=120.948739(-2.13%), 34.08M WeightUpdates/sec
Iter:34/100, MeanErr=120.309867(-0.53%), 36.53M WeightUpdates/sec
Iter:35/100, MeanErr=121.284711(0.81%), 34.29M WeightUpdates/sec
Iter:36/100, MeanErr=119.732461(-1.28%), 35.21M WeightUpdates/sec
Iter:37/100, MeanErr=119.040057(-0.58%), 34.55M WeightUpdates/sec
Iter:38/100, MeanErr=115.106132(-3.30%), 26.32M WeightUpdates/sec
Iter:39/100, MeanErr=115.677194(0.50%), 21.71M WeightUpdates/sec
Iter:40/100, MeanErr=112.593542(-2.67%), 31.39M WeightUpdates/sec
Iter:41/100, MeanErr=106.204713(-5.67%), 32.75M WeightUpdates/sec
Iter:42/100, MeanErr=117.750456(10.87%), 32.04M WeightUpdates/sec
Iter:43/100, MeanErr=114.797985(-2.51%), 35.04M WeightUpdates/sec
Iter:44/100, MeanErr=105.717625(-7.91%), 34.77M WeightUpdates/sec
Iter:45/100, MeanErr=112.070946(6.01%), 33.36M WeightUpdates/sec
Iter:46/100, MeanErr=111.963795(-0.10%), 26.33M WeightUpdates/sec
Iter:47/100, MeanErr=110.656390(-1.17%), 23.38M WeightUpdates/sec
Iter:48/100, MeanErr=105.336215(-4.81%), 5.12M WeightUpdates/sec
Iter:49/100, MeanErr=110.097218(4.52%), 27.91M WeightUpdates/sec
Iter:50/100, MeanErr=100.563206(-8.66%), 32.23M WeightUpdates/sec
Iter:51/100, MeanErr=107.710145(7.11%), 34.31M WeightUpdates/sec
Iter:52/100, MeanErr=104.976535(-2.54%), 33.64M WeightUpdates/sec
Iter:53/100, MeanErr=107.620278(2.52%), 33.49M WeightUpdates/sec
Iter:54/100, MeanErr=98.200313(-8.75%), 35.12M WeightUpdates/sec
Iter:55/100, MeanErr=105.280516(7.21%), 32.16M WeightUpdates/sec
Iter:56/100, MeanErr=104.684729(-0.57%), 30.20M WeightUpdates/sec
Iter:57/100, MeanErr=88.526827(-15.43%), 35.57M WeightUpdates/sec
Iter:58/100, MeanErr=108.108743(22.12%), 35.97M WeightUpdates/sec
Iter:59/100, MeanErr=102.303119(-5.37%), 28.00M WeightUpdates/sec
Iter:60/100, MeanErr=98.563782(-3.66%), 10.07M WeightUpdates/sec
Iter:61/100, MeanErr=97.859361(-0.71%), 26.04M WeightUpdates/sec
Iter:62/100, MeanErr=94.570119(-3.36%), 36.62M WeightUpdates/sec
Iter:63/100, MeanErr=98.130325(3.76%), 31.52M WeightUpdates/sec
Iter:64/100, MeanErr=96.893025(-1.26%), 30.70M WeightUpdates/sec
Iter:65/100, MeanErr=98.139161(1.29%), 35.35M WeightUpdates/sec
Iter:66/100, MeanErr=96.247537(-1.93%), 33.66M WeightUpdates/sec
Iter:67/100, MeanErr=88.551133(-8.00%), 33.64M WeightUpdates/sec
Iter:68/100, MeanErr=96.699175(9.20%), 33.09M WeightUpdates/sec
Iter:69/100, MeanErr=95.410056(-1.33%), 24.35M WeightUpdates/sec
Iter:70/100, MeanErr=92.663697(-2.88%), 24.33M WeightUpdates/sec
Iter:71/100, MeanErr=94.167808(1.62%), 25.14M WeightUpdates/sec
Iter:72/100, MeanErr=89.987045(-4.44%), 11.85M WeightUpdates/sec
Iter:73/100, MeanErr=91.472234(1.65%), 29.90M WeightUpdates/sec
Iter:74/100, MeanErr=91.027656(-0.49%), 35.15M WeightUpdates/sec
Iter:75/100, MeanErr=89.446611(-1.74%), 35.07M WeightUpdates/sec
Iter:76/100, MeanErr=89.954753(0.57%), 33.14M WeightUpdates/sec
Iter:77/100, MeanErr=87.746133(-2.46%), 34.52M WeightUpdates/sec
Iter:78/100, MeanErr=86.143053(-1.83%), 33.82M WeightUpdates/sec
Iter:79/100, MeanErr=85.254752(-1.03%), 31.68M WeightUpdates/sec
Iter:80/100, MeanErr=83.350050(-2.23%), 31.08M WeightUpdates/sec
Iter:81/100, MeanErr=85.123415(2.13%), 32.61M WeightUpdates/sec
Iter:82/100, MeanErr=85.837199(0.84%), 23.80M WeightUpdates/sec
Iter:83/100, MeanErr=83.372177(-2.87%), 21.99M WeightUpdates/sec
Iter:84/100, MeanErr=77.209704(-7.39%), 11.02M WeightUpdates/sec
Iter:85/100, MeanErr=81.498262(5.55%), 31.98M WeightUpdates/sec
Iter:86/100, MeanErr=82.399307(1.11%), 32.30M WeightUpdates/sec
Iter:87/100, MeanErr=81.566694(-1.01%), 33.89M WeightUpdates/sec
Iter:88/100, MeanErr=81.935024(0.45%), 25.40M WeightUpdates/sec
Iter:89/100, MeanErr=73.952480(-9.74%), 28.14M WeightUpdates/sec
Iter:90/100, MeanErr=78.612055(6.30%), 30.39M WeightUpdates/sec
Iter:91/100, MeanErr=79.791309(1.50%), 31.30M WeightUpdates/sec
Iter:92/100, MeanErr=75.940334(-4.83%), 30.78M WeightUpdates/sec
Iter:93/100, MeanErr=79.187979(4.28%), 23.43M WeightUpdates/sec
Iter:94/100, MeanErr=71.404282(-9.83%), 22.44M WeightUpdates/sec
Iter:95/100, MeanErr=78.696897(10.21%), 13.56M WeightUpdates/sec
Iter:96/100, MeanErr=75.423796(-4.16%), 26.33M WeightUpdates/sec
Iter:97/100, MeanErr=76.725671(1.73%), 29.78M WeightUpdates/sec
Iter:98/100, MeanErr=75.552711(-1.53%), 33.26M WeightUpdates/sec
Iter:99/100, MeanErr=75.233975(-0.42%), 35.29M WeightUpdates/sec
Iter:100/100, MeanErr=71.016582(-5.61%), 34.90M WeightUpdates/sec
Done!
Estimated Post-training MeanError = 67.685316
___________________________________________________________________
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.1864098
Elapsed time: 00:00:00.0167028
Beginning processing data.
Rows Read: 8, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0774545
Finished writing 8 rows.
Writing completed.
Data will be written to C:\Users\xavie\AppData\Local\Temp\rre6984810.xdf File will be overwritten if it exists.

Rows Processed: 5 
   rating      Score
0    71.0  69.165726
1    64.0  64.797623
2    74.0  71.603012
3    81.0  69.925659
4    64.0  65.523087
```



## optimizers

* [*microsoftml.adadelta_optimizer*: Adaptive learing rate method](adadelta_optimizer.md) 

* [*microsoftml.sgd_optimizer*: Stochastic Gradient Descent](sgd_optimizer.md) 


## math

* [*microsoftml.avx_math*: Options for *rx_neural_network*](avx_math.md) 

* [*microsoftml.clr_math*: Options for *rx_neural_network*](clr_math.md) 

* [*microsoftml.gpu_math*: Options for *rx_neural_network*](gpu_math.md) 

* [*microsoftml.mkl_math*: Options for *rx_neural_network*](mkl_math.md) 

* [*microsoftml.sse_math*](sse_math.md) 
