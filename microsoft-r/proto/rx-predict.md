--- 
 
# required metadata 
title: "rx_predict (microsoftml package for Python, Microsoft Machine Learning Server) | Microsoft Docs" 
description: "Reports per-instance scoring results in a data frame or revoscalepy data source using a trained Microsoft ML Machine Learning model with a revoscalepydata source." 
keywords: "models, prediction" 
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

# rx_predict

 Applies to: [microsoftml package for Python](./python-reference/microsoftml/microsoftml-package.md), version 0.0.0 


## Description

Reports per-instance scoring results in a data frame or revoscalepy data source using a trained Microsoft ML Machine Learning model with a revoscalepydata source.

This is what the metadata looks like:

 ```
title: "rx_predict (microsoftml package for Python, Microsoft Machine Learning Server) | Microsoft Docs" 
description: "Reports per-instance scoring results in a data frame or revoscalepy data source using a trained Microsoft ML Machine Learning model with a revoscalepydata source." 
 ```

## Usage



```
microsoftml.rx_predict(model, data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, pandas.core.frame.DataFrame], output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, write_model_vars: bool = False, extra_vars_to_write: list = None, suffix: str = None, overwrite: bool = False, data_threads: int = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 1, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None, **kargs)
```




## Description

Reports per-instance scoring results in a data frame or revoscalepy data source
using a trained Microsoft ML Machine Learning model with arevoscalepydata
source.


## Details

The following items are reported in the output by default: scoring on three
variables for the binary classifiers: PredictedLabel, Score, and Probability;
the Score for oneClassSvm and regression classifiers; PredictedLabel for
Multi-class classifiers, plus a variable for each category prepended by the
Score.


## Arguments


### model

A model information object returned from a microsoftml model.
For example, an object returned from `rx_fast_trees` or `rx_logistic_regression`.


### data

A [revoscalepy](/python-reference/revoscale.py/index.md) data source object, a data frame, or the path
to a `.xdf` file.


### output_data

Output text or xdf file name or an `RxDataSource` with
write capabilities in which to store transformed data. If *None*, a data
frame is returned. The default value is *None*.


### write_model_vars

If `True`, variables in the model are written
to the output data set in addition to the scoring variables.
If variables from the input data set are transformed in the model, the
transformed variables are also included. The default value is `False`.


### extra_vars_to_write

`None` or character vector of additional
variables names from the input data to include in the `output_data`. If
`write_model_vars` is `True`, model variables are included as
well. The default value is `None`.


### suffix

A character string specifying suffix to append to the created
scoring variable(s) or `None` in there is no suffix. The default
value is `None`.


### overwrite

If `True`, an existing `output_data` is overwritten;
if `False` an existing `output_data` is not overwritten. The default
value is `False`.


### data_threads

An integer specifying the desired degree of parallelism in
the data pipeline. If *None*, the number of threads used is determined
internally. The default value is *None*.


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

The default value is `1`.


### verbose

An integer value that specifies the amount of output wanted.
If `0`, no verbose output is printed during calculations. Integer
values from `1` to `4` provide increasing amounts of information.
The default value is `1`.


### compute_context

Sets the context in which computations are executed,
specified with a valid [revoscalepy.RxComputeContext](/python-reference/revoscale.py/api/RxComputeContext.md).
Currently local and [revoscalepy.RxInSqlServer](/python-reference/revoscale.py/api/RxInSqlServer.md) compute contexts
are supported.


### kargs

Additional arguments sent to compute engine.


## Returns

A data frame or an [revoscalepy.RxDataSource](/python-reference/revoscale.py/api/RxDataSource.md) object
representing the created output data. By default, output from scoring binary
classifiers include three variables: `PredictedLabel`,
`Score`, and `Probability`; `rx_oneclass_svm` and regression
include one variable: `Score`; and multi-class classifiers include
`PredictedLabel` plus a variable for each category prepended by
`Score`. If a `suffix` is provided, it is added to the end
of these output variable names.


## See also

[`rx_featurize`](rx-featurize.md),
[revoscalepy.rx_data_step](/python-reference/revoscale.py/api/rx-data-step.md),
[revoscalepy.rx_import](/python-reference/revoscale.py/api/rx-import.md).


## Example



```
'''
Binary Classification.
'''
import numpy
import pandas
from microsoftml import rx_fast_linear, rx_predict
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

forest_model = rx_fast_linear(
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
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning processing data.
Rows Read: 186, Read Time: 0, Transform Time: 0
Beginning processing data.
Using 2 threads to train.
Automatically choosing a check frequency of 2.
Auto-tuning parameters: maxIterations = 8064.
Auto-tuning parameters: L2 = 2.666837E-05.
Auto-tuning parameters: L1Threshold (L1/L2) = 0.
Using best model from iteration 570.
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.8088403
Elapsed time: 00:00:00.0239236
Beginning processing data.
Rows Read: 62, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0915609
Finished writing 62 rows.
Writing completed.
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre8880911.xdf File will be overwritten if it exists.

Rows Processed: 5 
  isCase PredictedLabel     Score  Probability
0  False          False -2.049673     0.114085
1  False           True  0.530216     0.629534
2  False          False -2.280835     0.092723
3   True           True  2.195549     0.899849
4  False          False -0.860523     0.297230
```



## Example



```
'''
Regression.
'''
import numpy
import pandas
from microsoftml import rx_fast_trees, rx_predict
from revoscalepy.etl.RxDataStep import rx_data_step
from microsoftml.datasets.datasets import airquality
import sklearn
if sklearn.__version__ < "0.18":
    from sklearn.cross_validation import train_test_split
else:
    from sklearn.model_selection import train_test_split

airquality = airquality.as_df()


######################################################################
# Estimate a regression fast forest
# Use the built-in data set 'airquality' to create test and train data

df = airquality[airquality.Ozone.notnull()]
df["Ozone"] = df.Ozone.astype(float)

data_train, data_test, y_train, y_test = train_test_split(df, df.Ozone)

airFormula = " Ozone ~ Solar_R + Wind + Temp "

# Regression Fast Forest for train data
ff_reg = rx_fast_trees(airFormula, method="regression", data=data_train)

# Put score and model variables in data frame
score_df = rx_predict(ff_reg, data=data_test, write_model_vars=True)
print(score_df.head())

# Plot actual versus predicted values with smoothed line
# Supported in the next version.
# rx_line_plot(" Score ~ Ozone ", type=["p", "smooth"], data=score_df)
```


Output:



```
'unbalanced_sets' ignored for method 'regression'
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
Not adding a normalizer.
Making per-feature arrays
Changing data from row-wise to column-wise
Beginning processing data.
Rows Read: 87, Read Time: 0, Transform Time: 0
Beginning processing data.
Warning: Skipped 4 instances with missing features during training
Processed 83 instances
Binning and forming Feature objects
Reserved memory for tree learner: 21840 bytes
Starting to train ...
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.0820105
Elapsed time: 00:00:00.0235356
Beginning processing data.
Rows Read: 29, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.0652775
Finished writing 29 rows.
Writing completed.
   Solar_R  Wind  Temp      Score
0     77.0   7.4    82  16.359118
1    220.0  10.3    78  40.989021
2    276.0   5.1    88  91.789886
3    260.0   6.9    81  94.888527
4    212.0   9.7    79  37.633297
```

