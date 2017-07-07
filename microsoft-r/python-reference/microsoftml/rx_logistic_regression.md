--- 
 
# required metadata 
title: "Logistic Regression" 
description: "Machine Learning Logistic Regression" 
keywords: "models, classification" 
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

## ``rx_logistic_regression``: Logistic Regression


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.rx_logistic_regression(formula: str, data: [<class ‘revoscalepy.datasource.RxDataSource.RxDataSource’>, <class ‘pandas.core.frame.DataFrame’>], method: [‘binary’, ‘multiClass’] = ‘binary’, l2_weight: float = 1, l1_weight: float = 1, opt_tol: float = 1e-07, memory_size: int = 20, init_wts_diameter: float = 0, max_iterations: int = 2147483647, show_training_stats: bool = False, sgd_init_tol: float = 0, train_threads: int = None, dense_optimizer: bool = False, normalize: [‘No’, ‘Warn’, ‘Auto’, ‘Yes’] = ‘Auto’, ml_transforms: list = None, ml_transform_vars: list = None, row_selection: str = None, transforms: dict = None, transform_objects: dict = None, transform_function: str = None, transform_variables: list = None, transform_packages: list = None, transform_environment: dict = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, ensemble: dict = None, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None)
```




### Description

Machine Learning Logistic Regression


### Details

Logistic Regression is a classification method used to predict
the value of a categorical dependent variable from its relationship to one
or more independent variables assumed to have a logistic distribution. If
the dependent variable has only two possible values (success/failure),
then the logistic regression is binary. If the dependent variable has
more than two possible values (blood type given diagnostic test results),
then the logistic regression is multinomial.

The optimization technique used for ``rx_logistic_regression`` is the
limited memory Broyden-Fletcher-Goldfarb-Shanno (L-BFGS). Both the L-BFGS
and regular BFGS algorithms use quasi-Newtonian methods to estimate the
computationally intensive Hessian matrix in the equation used by Newton’s
method to calculate steps. But the L-BFGS approximation uses only a limited
amount of memory to compute the next step direction, so that it is especially
suited for problems with a large number of variables. The ``memory_size``
parameter specifies the number of past positions and gradients to store for
use in the computation of the next step.

This learner can use elastic net regularization: a linear combination of L1
(lasso) and L2 (ridge) regularizations. Regularization is a method that
can render an ill-posed problem more tractable by imposing constraints that
provide information to supplement the data and that prevents overfitting by
penalizing models with extreme coefficient values. This can improve the
generalization of the model learned by selecting the optimal complexity
in the bias-variance tradeoff. Regularization works by
adding the penalty that is associated with coefficient values to the error
of the hypothesis. An accurate model with extreme coefficient values would
be penalized more, but a less accurate model with more conservative values
would be penalized less. L1 and L2 regularization have different effects
and uses that are complementary in certain respects.

* ``l1_weight``: can be applied to sparse models, when working with high-dimensional data. It pulls small weights associated features that are relatively unimportant towards 0. 

* ``l2_weight``: is preferable for data that is not sparse. It pulls large weights towards zero. 

Adding the ridge penalty to the regularization overcomes some of lasso’s
limitations. It can improve its predictive accuracy, for example, when
the number of predictors is greater than the sample size.
If ``x = l1_weight`` and ``y = l2_weight``, ``ax + by = c``
defines the linear span of the regularization terms. The default values
of x and y are both ``1``. An agressive regularization can harm predictive
capacity by excluding important variables out of the model. So choosing the
optimal values for the regularization parameters is important for the
performance of the logistic regression model.


### Arguments


##### formula

The formula as described in ``rx_formula``.
Interaction terms and ``F()`` are not currently supported in the
.


##### data

A data source object or a character string specifying a
*.xdf* file or a data frame object.


##### method

A character string that specifies the type of Logistic Regression:
``"binary"`` for the default binary classification logistic regression or
``"multiClass"`` for multinomial logistic regression.


##### l2_weight

The L2 regularization weight. Its value must be greater than
or equal to ``0`` and the default value is set to ``1``.


##### l1_weight

The L1 regularization weight. Its value must be greater than
or equal to ``0`` and the default value is set to ``1``.


##### opt_tol

Threshold value for optimizer convergence. If the improvement
between iterations is less than the threshold, the algorithm stops and
returns the current model. Smaller values are slower, but more accurate.
The default value is ``1e-07``.


##### memory_size

Memory size for L-BFGS, specifying the number of past
positions and gradients to store for the computation of the next step. This
optimization parameter limits the amount of memory that is used to compute
the magnitude and direction of the next step. When you specify less memory,
training is faster but less accurate. Must be greater than or equal to
``1`` and the default value is ``20``.


##### max_iterations

Sets the maximum number of iterations. After this number
of steps, the algorithm stops even if it has not satisfied convergence
criteria.


##### show_training_stats

Specify ``True`` to show the statistics of
training data and the trained model; otherwise, ``False``. The
default value is ``False``. For additional information about model
statistics, see ``summary.ml_model()``.


##### sgd_init_tol

Set to a number greater than 0 to use Stochastic
Gradient Descent (SGD) to find the initial parameters. A non-zero value
set specifies the tolerance SGD uses to determine convergence.
The default value is ``0`` specifying that SGD is not used.


##### init_wts_diameter

Sets the initial weights diameter that specifies
the range from which values are drawn for the initial weights. These
weights are initialized randomly from within this range. For
example, if the diameter is specified to be ``d``, then the weights
are uniformly distributed between ``-d/2`` and ``d/2``. The
default value is ``0``, which specifies that allthe  weights are
initialized to ``0``.


##### train_threads

The number of threads to use in training the model.
This should be set to the number of cores on the machine. Note that
L-BFGS multi-threading attempts to load dataset into memory. In case of
out-of-memory issues, set ``train_threads`` to ``1`` to turn off
multi-threading. If *None* the number of threads to use is
determined internally. The default value is *None*.


##### dense_optimizer

If ``True``, forces densification of the internal
optimization vectors. If ``False``, enables the logistic regression
optimizer use sparse or dense internal states as it finds appropriate.
Setting ``denseOptimizer`` to ``True`` requires the internal
optimizer to use a dense internal state, which may help alleviate load
on the garbage collector for some varieties of larger problems.


##### normalize

Specifies the type of automatic normalization used:

* ``"Auto"``: if normalization is needed, it is performed automatically. This is the default choice. 

* ``"No"``: no normalization is performed. 

* ``"Yes"``: normalization is performed. 

* ``"Warn"``: if normalization is needed, a warning message is displayed, but normalization is not performed. 

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
and [``categorical_hash``](categorical_hash.md), for transformations that aresupported.
These transformations are performed after any specified Python transformations.
The default avlue is *None*.


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

NOT SUPPORTED. An expression of the form that represents
the first round of variable transformations. As with
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

A [``LogisticRegression``](learners_object.md) object
with the trained model.


### Note

This algorithm will attempt to load the entire dataset into memory
when ``train_threads > 1`` (multi-threading).


### See also

[``rx_predict``](rx_predict.md)


### References

[Wikipedia: L-BFGS](http://en.wikipedia.org/wiki/L-BFGS.md)

[Wikipedia: Logistic
regression](http://en.wikipedia.org/wiki/Logistic_regression.md)

[Scalable
Training of L1-Regularized Log-Linear Models](http://research.microsoft.com/apps/pubs/default.aspx?id=78900.md)

[Test Run - L1
and L2 Regularization for Machine Learning](https://msdn.microsoft.com/en-us/magazine/dn904675.aspx.md)


### Example



```
'''
Binary Classification.
'''
import numpy
import pandas
from microsoftml import rx_logistic_regression, rx_predict
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

model = rx_logistic_regression(
    formula=" isCase ~ age + parity + education + spontaneous + induced ",
    data=data_train)

print(model.coef_)
    
# RuntimeError: The type (RxTextData) for file is not supported.
score_ds = rx_predict(model, data=data_test,
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
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
LBFGS multi-threading will attempt to load dataset into memory. In case of out-of-memory issues, turn off multi-threading by setting trainThreads to 1.
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning optimization
num vars: 6
improvement criterion: Mean Improvement
L1 regularization selected 5 of 6 weights.
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.1274826
Elapsed time: 00:00:00.0296187
OrderedDict([('(Bias)', -1.3789191246032715), ('spontaneous', 1.882440447807312), ('induced', 0.3716955780982971), ('parity', -0.16196750104427338), ('education', 0.013873458839952946)])
Beginning processing data.
Rows Read: 62, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.1025930
Finished writing 62 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre1477646.xdf File will be overwritten if it exists.

Rows Processed: 5 
  isCase PredictedLabel     Score  Probability
0  False          False -0.504809     0.376411
1  False          False -1.240124     0.224414
2  False          False -0.477815     0.382768
3   True          False -1.047339     0.259736
4  False          False -1.392040     0.199082
```



### Example



```
'''
MultiClass Classification
'''
import numpy
import pandas
from microsoftml import rx_logistic_regression, rx_predict
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

model = rx_logistic_regression(
    formula="  Species ~ Sepal_Length + Sepal_Width + Petal_Length + Petal_Width ",
    method="multiClass",
    data=data_train)

print(model.coef_)
    
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
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
LBFGS multi-threading will attempt to load dataset into memory. In case of out-of-memory issues, turn off multi-threading by setting trainThreads to 1.
Beginning processing data.
Rows Read: 112, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning optimization
num vars: 15
improvement criterion: Mean Improvement
L1 regularization selected 11 of 15 weights.
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.1283819
Elapsed time: 00:00:00.0280703
OrderedDict([('setosa+(Bias)', 2.0276567935943604), ('versicolor+(Bias)', 0.6149432063102722), ('virginica+(Bias)', -2.642597198486328), ('setosa+Petal_Width', -2.7751617431640625), ('setosa+Petal_Length', -2.4810328483581543), ('setosa+Sepal_Width', 0.28857436776161194), ('setosa+Sepal_Length', 1.0937617389572551e-07), ('versicolor+Sepal_Width', -0.6138118505477905), ('virginica+Petal_Width', 2.663684844970703), ('virginica+Petal_Length', 1.600286602973938), ('virginica+Sepal_Length', 0.08190837502479553)])
Beginning processing data.
Rows Read: 38, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.1026499
Finished writing 38 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre1477657.xdf File will be overwritten if it exists.

Rows Processed: 5 
      Species   Score.0   Score.1   Score.2
0   virginica  0.072361  0.411070  0.516569
1   virginica  0.012098  0.207713  0.780188
2  versicolor  0.157034  0.475651  0.367314
3      setosa  0.778927  0.198099  0.022974
4  versicolor  0.321431  0.490487  0.188082
```

