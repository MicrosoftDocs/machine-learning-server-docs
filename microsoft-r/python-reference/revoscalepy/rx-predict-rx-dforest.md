--- 
 
# required metadata 
title: "rx_predict_rx_dforest: Prediction for Large Data Classification and Regression Forests (revoscalepy)" 
description: "Calculate predicted or fitted values for a data set from an rx_dforest or rx_btrees object." 
keywords: "predict" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
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

# rx_predict_rx_dforest


 


## Usage



```
revoscalepy.rx_predict_rx_dforest(model_object=None,
    data: revoscalepy.datasource.RxDataSource.RxDataSource = None,
    output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    str] = None, predict_var_names: list = None,
    write_model_vars: bool = False,
    extra_vars_to_write: list = None, append: typing.Union[list,
    str] = 'none', overwrite: bool = False, type: typing.Union[list,
    str] = None, cutoff: list = None,
    remove_missings: bool = False, compute_residuals: bool = False,
    residual_type: typing.Union[list, str] = 'usual',
    residual_var_names: list = None, blocks_per_read: int = None,
    report_progress: int = None, verbose: int = 0,
    xdf_compression_level: int = None, compute_context=None, **kwargs)
```





## Description

Calculate predicted or fitted values for a data set from an rx_dforest or rx_btrees object.


## Arguments


### model_object

object returned from a call to rx_dtree.


### data

a data frame or an RxXdfData data source object to be used for predictions.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### output_data

an RxXdfData data source object or existing data frame
to store predictions.


### predict_var_names

list of strings specifying name(s) to give to the prediction results


### write_model_vars

bool value. If True, and the output data set is
different from the input data set, variables in the model will be written
to the output data set in addition to the predictions (and residuals,
standard errors, and confidence bounds, if requested). If variables from
the input data set are transformed in the model, the transformed variables
will also be included.


### extra_vars_to_write

None or list of strings of additional variables
names from the input data or transforms to include in the output_data. If
write_model_vars is True, model variables will be included as well.


### append

either “none” to create a new files or “rows” to append rows
to an existing file. If output_data exists and append is “none”, the overwrite
argument must be set to True. Ignored for data frames.


### overwrite

bool value. If True, an existing output_data will be overwritten.
overwrite is ignored if appending rows. Ignored for data frames.


### type

the type of prediction desired. Supported choices are: “response”,
“prob”, and “vote”.


### cutoff

(Classification only) a vector of length equal to the number of
classes specifying the dividing factors for the class votes. The default is
the one used when the decision forest is built.


### remove_missings

bool value. If True, rows with missing values are removed.


### compute_residuals

bool value. If True, residuals are computed.


### residual_type

Indicates the type of residual desired.


### residual_var_names

list of strings specifying name(s) to give to the residual results.


### blocks_per_read

number of blocks to read for each chunk of data read
from the data source. If the data and output_data are the same file,
blocks_per_read must be 1.


### report_progress

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


### verbose

integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


### xdf_compression_level

integer in the range of -1 to 9 indicating the
compression level for the output data if written to an .xdf file.


### compute_context

a RxComputeContext object for prediction.


### kwargs

additional parameters


## Returns

a data frame or a data source object of prediction results.


## See also

[`rx_predict`](rx-predict.md).


## Example



```
import os
from revoscalepy import rx_dforest, rx_predict_rx_dforest, rx_import, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import(input_data = ds)

# classification
formula = "Kyphosis ~ Number + Start"
method = "class"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}

dforest = rx_dforest(formula, data = kyphosis, pweights = "Age", method = method,
                     parms = parms, cost = [2, 3], max_num_bins = 100, importance = True,
                     n_tree = 3, m_try = 2, sample_rate = 0.5,
                     replace = False, seed = 0, compute_oob_error = 1)
rx_pred = rx_predict_rx_dforest(dforest, data = kyphosis)
rx_pred.head()

# regression
formula = "Age ~ Number + Start"
method = "anova"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}

dforest = rx_dforest(formula, data = kyphosis, pweights = "Kyphosis", method = method,
                     parms = parms, cost = [2, 3], max_num_bins = 100, importance = True,
                     n_tree = 3, m_try = 2, sample_rate = 0.5,
                     replace = False, seed = 0, compute_oob_error = 1)
rx_pred = rx_predict_rx_dforest(dforest, data = kyphosis)
rx_pred.head()
```

