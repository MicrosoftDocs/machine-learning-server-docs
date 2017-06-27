--- 
 
# required metadata 
title: "Prediction for Large Data Classification and Regression Trees" 
description: "Calculate predicted or fitted values for a data set from an rx_dtree object." 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# rx_predict_rx_dtree
=======
## rx_predict_rx_dtree


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.functions.RxPredict.rx_predict_rx_dtree(model_object=None, data: revoscalepy.datasource.RxDataSource.RxDataSource = None, output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, predict_var_names: list = None, write_model_vars: bool = False, extra_vars_to_write: list = None, append: typing.Union[list, str] = ‘none’, overwrite: bool = False, type: typing.Union[list, str] = None, remove_missings: bool = False, compute_residuals: bool = False, residual_type: typing.Union[list, str] = ‘usual’, residual_var_names: list = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 0, xdf_compression_level: int = None, compute_context=None, **kwargs)
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Calculate predicted or fitted values for a data set from an rx_dtree object.


<<<<<<< HEAD
## Parameters


### model_object
=======
### Arguments


##### model_object
>>>>>>> heidist-revoscalepy

object returned from a call to rx_dtree.


<<<<<<< HEAD
### data
=======
##### data
>>>>>>> heidist-revoscalepy

a data frame or an RxXdfData data source object to be used for predictions.


<<<<<<< HEAD
### output_data
=======
##### output_data
>>>>>>> heidist-revoscalepy

an RxXdfData data source object or existing data frame
to store predictions.


<<<<<<< HEAD
### predict_var_names
=======
##### predict_var_names
>>>>>>> heidist-revoscalepy

character vector specifying name(s) to give to the prediction results


<<<<<<< HEAD
### write_model_vars
=======
##### write_model_vars
>>>>>>> heidist-revoscalepy

logical value. If True, and the output data set is
different from the input data set, variables in the model will be written
to the output data set in addition to the predictions (and residuals,
standard errors, and confidence bounds, if requested). If variables from
the input data set are transformed in the model, the transformed variables
will also be included.


<<<<<<< HEAD
### extra_vars_to_write
=======
##### extra_vars_to_write
>>>>>>> heidist-revoscalepy

None or character vector of additional variables
names from the input data or transforms to include in the outData. If
writeModelVars is True, model variables will be included as well.


<<<<<<< HEAD
### append
=======
##### append
>>>>>>> heidist-revoscalepy

either “none” to create a new files or “rows” to append rows
to an existing file. If outData exists and append is “none”, the overwrite
argument must be set to True. You can append only to RxTeradata data source.
Ignored for data frames.


<<<<<<< HEAD
### overwrite
=======
##### overwrite
>>>>>>> heidist-revoscalepy

logical value. If True, an existing outData will be overwritten.
overwrite is ignored if appending rows. Ignored for data frames.


<<<<<<< HEAD
### type
=======
##### type
>>>>>>> heidist-revoscalepy

the type of prediction desired. Supported choices are: “vector”,
“prob”, “class”, and “matrix”.


<<<<<<< HEAD
### remove_missings
=======
##### remove_missings
>>>>>>> heidist-revoscalepy

logical value. If True, rows with missing values are removed.


<<<<<<< HEAD
### compute_residuals
=======
##### compute_residuals
>>>>>>> heidist-revoscalepy

logical value. If True, residuals are computed.


<<<<<<< HEAD
### residual_type
=======
##### residual_type
>>>>>>> heidist-revoscalepy

Indicates the type of residual desired.


<<<<<<< HEAD
### residual_var_names
=======
##### residual_var_names
>>>>>>> heidist-revoscalepy

character vector specifying name(s) to give to the residual results.


<<<<<<< HEAD
### blocks_per_read
=======
##### blocks_per_read
>>>>>>> heidist-revoscalepy

number of blocks to read for each chunk of data read
from the data source. If the data and outData are the same file,
blocksPerRead must be 1.


<<<<<<< HEAD
### report_progress
=======
##### report_progress
>>>>>>> heidist-revoscalepy

integer value with options:
0: no progress is reported.
1: the number of processed rows is printed and updated.
2: rows processed and timings are reported.
3: rows processed and all timings are reported.


<<<<<<< HEAD
### verbose
=======
##### verbose
>>>>>>> heidist-revoscalepy

integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


<<<<<<< HEAD
### xdf_compression_level
=======
##### xdf_compression_level
>>>>>>> heidist-revoscalepy

integer in the range of -1 to 9 indicating the
compression level for the output data if written to an .xdf file.


<<<<<<< HEAD
### compute_context
=======
##### compute_context
>>>>>>> heidist-revoscalepy

a RxComputeContext object for prediction.


<<<<<<< HEAD
### kwargs
=======
##### kwargs
>>>>>>> heidist-revoscalepy

additional parameters


<<<<<<< HEAD
## Returns
=======
### Returns
>>>>>>> heidist-revoscalepy

a data frame or a data source object of prediction results.


<<<<<<< HEAD
## Author
=======
### Author
>>>>>>> heidist-revoscalepy

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


<<<<<<< HEAD
## See also


## Example
=======
### See also


### Example
>>>>>>> heidist-revoscalepy



```
import os
from revoscalepy import rx_dtree, rx_predict, rx_import_datasource, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import_datasource(input_data = ds)

# classification
formula = "Kyphosis ~ Number + Start"
method = "class"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}
cost = [2,3]
dtree = rx_dtree(formula, data = kyphosis, pweights = "Age", method = method, parms = parms, cost = cost, max_num_bins = 100)
rx_pred = rx_predict(dtree, data = kyphosis)

# regression
formula = "Age ~ Number + Start"
method = "anova"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}
cost = [2,3]
dtree = rx_dtree(formula, data = kyphosis, pweights = "Kyphosis", method = method, parms = parms, cost = cost, max_num_bins = 100)
rx_pred = rx_predict(dtree, data = kyphosis)
```

