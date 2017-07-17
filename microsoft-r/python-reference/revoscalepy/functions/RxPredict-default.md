--- 
 
# required metadata 
title: "Predicted Values and Residuals for rx_lin_mod and rx_logit" 
description: "Compute predicted values and residuals using rx_lin_mod and rx_logit objects." 
keywords: "" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "jhubbard" 
ms.date: "07/17/2017" 
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

# rx_predict_default


**Applies to: SQL Server 2017 RC1**


## Usage



```
revoscalepy.functions.RxPredict.rx_predict_default(model_object=None, data: revoscalepy.datasource.RxDataSource.RxDataSource = None, output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, compute_standard_errors: bool = False, interval: typing.Union[list, str] = 'none', confidence_level: float = 0.95, compute_residuals: bool = False, type: typing.Union[list, str] = None, write_model_vars: bool = False, extra_vars_to_write: typing.Union[list, str] = None, remove_missings: bool = False, append: typing.Union[list, str] = None, overwrite: bool = False, check_factor_levels: bool = True, predict_var_names: typing.Union[list, str] = None, residual_var_names: typing.Union[list, str] = None, interval_var_names: typing.Union[list, str] = None, std_errors_var_names: typing.Union[list, str] = None, blocks_per_read: int = 0, report_progress: int = 0, verbose: int = 0, xdf_compression_level: int = 0, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None, **kwargs)
```




## Description

Compute predicted values and residuals using rx_lin_mod and
rx_logit objects.


## Arguments


### model_object

object returned from a call to rx_lin_mod and rx_logit.
Objects with multiple dependent variables are not supported.


### data

a data frame or an RxXdfData data source object to be used for predictions.


### output_data

an RxXdfData data source object or existing data frame
to store predictions.


### compute_standard_errors

bool value. If True, the standard errors
for each dependent variable are calculated.


### interval

character string defining the type of interval calculation
to perform. Supported values are “none”, “confidence”, and “prediction”.


### confidence_level

numeric value representing the confidence level on
the interval [0, 1].


### compute_residuals

bool value. If True, residuals are computed.


### type

the type of prediction desired. Supported choices are: “response”
and “link”. If type = “response”, the predictions are on the scale of the
response variable. For instance, for the binomial model, the predictions
are in the range (0,1). If type = “link”, the predictions are on the scale
of the linear predictors. Thus for the binomial model, the predictions are
of log-odds.


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


### remove_missings

bool value. If True, rows with missing values are removed.


### append

either “none” to create a new files or “rows” to append rows
to an existing file. If output_data exists and append is “none”, the overwrite
argument must be set to True. Ignored for data frames.


### overwrite

bool value. If True, an existing output_data will be overwritten.
overwrite is ignored if appending rows. Ignored for data frames.


### check_factor_levels

bool value.


### predict_var_names

list of strings specifying name(s) to give to the prediction results.


### residual_var_names

list of strings specifying name(s) to give to the residual results.


### interval_var_names

None or a list of strings defining low and high
confidence interval variable names, respectively. If None, the strings
“_Lower” and “_Upper” are appended to the dependent variable names to
form the confidence interval variable names.


### std_errors_var_names

None or a list of strings defining variable
names corresponding to the standard errors, if calculated. If None, the
string “_StdErr” is appended to the dependent variable names to form the
standard errors variable names.


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


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also

[`rx_predict`](RxPredict.md).


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod, rx_predict_default, rx_data_step

sample_data_path = RxOptions.get_option("sampleDataDir")
mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))
mort_df = rx_data_step(mort_ds)

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", mort_df)
pred = rx_predict_default(lin_mod, data = mort_df, compute_residuals = True, write_model_vars = True)
pred.head()
```

