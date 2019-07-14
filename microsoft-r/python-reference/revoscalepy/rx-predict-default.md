--- 
 
# required metadata 
title: "rx_predict_default: Predicted Values and Residuals for rx_lin_mod and rx_logit (revoscalepy)" 
description: "Compute predicted values and residuals using rx_lin_mod and rx_logit objects." 
keywords: "predict" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
---

# rx_predict_default


 


## Usage



```
revoscalepy.rx_predict_default(model_object=None,
    data: revoscalepy.datasource.RxDataSource.RxDataSource = None,
    output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource,
    str] = None, compute_standard_errors: bool = False,
    interval: typing.Union[list, str] = 'none',
    confidence_level: float = 0.95, compute_residuals: bool = False,
    type: typing.Union[list, str] = None,
    write_model_vars: bool = False,
    extra_vars_to_write: typing.Union[list, str] = None,
    remove_missings: bool = False, append: typing.Union[list,
    str] = None, overwrite: bool = False,
    check_factor_levels: bool = True,
    predict_var_names: typing.Union[list, str] = None,
    residual_var_names: typing.Union[list, str] = None,
    interval_var_names: typing.Union[list, str] = None,
    std_errors_var_names: typing.Union[list, str] = None,
    blocks_per_read: int = 0, report_progress: int = None,
    verbose: int = 0, xdf_compression_level: int = 0,
    compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None,
    **kwargs)
```





## Description

Compute predicted values and residuals using rx_lin_mod and
rx_logit objects.


## Arguments


### model_object

Object returned from a call to rx_lin_mod and rx_logit.
Objects with multiple dependent variables are not supported.


### data

a data frame or an RxXdfData data source object to be used for predictions.
If a Spark compute context is being used, this argument may also be an RxHiveData,
RxOrcData, RxParquetData or RxSparkDataFrame object or a Spark data frame object from pyspark.sql.DataFrame.


### output_data

A character string specifying the output ‘.xdf’ file, a
RxXdfData object, RxTextData object, a RxOdbcData data source, or a
RxSqlServerData data source to store predictions.


### compute_standard_errors

Bool value. If True, the standard errors
for each dependent variable are calculated.


### interval

Character string defining the type of interval calculation
to perform. Supported values are “none”, “confidence”, and “prediction”.


### confidence_level

Numeric value representing the confidence level on
the interval [0, 1].


### compute_residuals

bool Value. If True, residuals are computed.


### type

The type of prediction desired. Supported choices are: “response”
and “link”. If type = “response”, the predictions are on the scale of the
response variable. For instance, for the binomial model, the predictions
are in the range (0,1). If type = “link”, the predictions are on the scale
of the linear predictors. Thus for the binomial model, the predictions are
of log-odds.


### write_model_vars

Bool value. If True, and the output data set is
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

Bool value. If True, rows with missing values are removed.


### append

either “none” to create a new file or “rows” to append rows
to an existing file. If output_data exists and append is “none”, the overwrite
argument must be set to True. Ignored for data frames.


### overwrite

Bool value. If True, an existing output_data will be overwritten.
overwrite is ignored if appending rows. Ignored for data frames.


### check_factor_levels

Bool value.


### predict_var_names

List of strings specifying name(s) to give to the prediction results.


### residual_var_names

List of strings specifying name(s) to give to the residual results.


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

Number of blocks to read for each chunk of data read
from the data source. If the data and output_data are the same file,
blocks_per_read must be 1.


### report_progress

Integer value with options:
0: No progress is reported.
1: The number of processed rows is printed and updated.
2: Rows processed and timings are reported.
3: Rows processed and all timings are reported.


### verbose

Integer value. If 0, no additional output is printed. If 1,
additional summary information is printed.


### xdf_compression_level

Integer in the range of -1 to 9 indicating the
compression level for the output data if written to an .xdf file.


### compute_context

A RxComputeContext object for prediction.


### kwargs

Additional parameters


## Returns

A data frame or a data source object of prediction results.


## See also

[`rx_predict`](rx-predict.md).


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

