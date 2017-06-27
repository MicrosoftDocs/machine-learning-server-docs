--- 
 
# required metadata 
title: "Predicted Values and Residuals for rx_lin_mod and rx_logit" 
description: "Compute predicted values and residuals using rx_lin_mod and" 
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
# rx_predict_default
=======
## rx_predict_default


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.functions.RxPredict.rx_predict_default(model_object=None, data: revoscalepy.datasource.RxDataSource.RxDataSource = None, output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, compute_standard_errors: bool = False, interval: typing.Union[list, str] = ‘none’, confidence_level: float = 0.95, compute_residuals: bool = False, type: typing.Union[list, str] = None, write_model_vars: bool = False, extra_vars_to_write: typing.Union[list, str] = None, remove_missings: bool = False, append: typing.Union[list, str] = None, overwrite: bool = False, check_factor_levels: bool = True, predict_var_names: typing.Union[list, str] = None, residual_var_names: typing.Union[list, str] = None, interval_var_names: typing.Union[list, str] = None, std_errors_var_names: typing.Union[list, str] = None, blocks_per_read: int = 0, report_progress: int = 0, verbose: int = 0, xdf_compression_level: int = 0, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None, **kwargs)
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Compute predicted values and residuals using rx_lin_mod and
rx_logit objects.


<<<<<<< HEAD
## Parameters


### model_object
=======
### Arguments


##### model_object
>>>>>>> heidist-revoscalepy

object returned from a call to rx_lin_mod and rx_logit.
Objects with multiple dependent variables are not supported.


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
### compute_standard_errors
=======
##### compute_standard_errors
>>>>>>> heidist-revoscalepy

logical value. If True, the standard errors
for each dependent variable are calculated.


<<<<<<< HEAD
### interval
=======
##### interval
>>>>>>> heidist-revoscalepy

character string defining the type of interval calculation
to perform. Supported values are “none”, “confidence”, and “prediction”.


<<<<<<< HEAD
### confidence_level
=======
##### confidence_level
>>>>>>> heidist-revoscalepy

numeric value representing the confidence level on
the interval [0, 1].


<<<<<<< HEAD
### compute_residuals
=======
##### compute_residuals
>>>>>>> heidist-revoscalepy

logical value. If True, residuals are computed.


<<<<<<< HEAD
### type
=======
##### type
>>>>>>> heidist-revoscalepy

the type of prediction desired. Supported choices are: “response”
and “link”. If type = “response”, the predictions are on the scale of the
response variable. For instance, for the binomial model, the predictions
are in the range (0,1). If type = “link”, the predictions are on the scale
of the linear predictors. Thus for the binomial model, the predictions are
of log-odds.


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
### remove_missings
=======
##### remove_missings
>>>>>>> heidist-revoscalepy

logical value. If True, rows with missing values are removed.


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

logical value. If True, an existing output_data will be overwritten.
overwrite is ignored if appending rows. Ignored for data frames.


<<<<<<< HEAD
### check_factor_levels
=======
##### check_factor_levels
>>>>>>> heidist-revoscalepy

logical value.


<<<<<<< HEAD
### predict_var_names
=======
##### predict_var_names
>>>>>>> heidist-revoscalepy

character vector specifying name(s) to give to the prediction results


<<<<<<< HEAD
### residual_var_names
=======
##### residual_var_names
>>>>>>> heidist-revoscalepy

character vector specifying name(s) to give to the residual results.


<<<<<<< HEAD
### interval_var_names
=======
##### interval_var_names
>>>>>>> heidist-revoscalepy

None or a character vector defining low and high
confidence interval variable names, respectively. If None, the strings
“_Lower” and “_Upper” are appended to the dependent variable names to
form the confidence interval variable names.


<<<<<<< HEAD
### std_errors_var_names
=======
##### std_errors_var_names
>>>>>>> heidist-revoscalepy

None or a character vector defining variable
names corresponding to the standard errors, if calculated. If None, the
string “_StdErr” is appended to the dependent variable names to form the
standard errors variable names.


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
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod, rx_predict, rx_data_step

sample_data_path = RxOptions.get_option("sampleDataDir")
mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))
mort_df = rx_data_step(mort_ds)

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", mort_df)
pred = rx_predict_default(lin_mod, data = mort_df, compute_residuals = True, write_model_vars = True)
pred.head()
```

