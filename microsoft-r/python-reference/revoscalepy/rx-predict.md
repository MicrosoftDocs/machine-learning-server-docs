--- 
 
# required metadata 
title: "rx_predict,rx_predict_default,rx_predict_rx_dforest,rx_predict_rx_dtree: Predicted Values and Residuals for rx_lin_mod, rx_logit, rx_dtree," 
description: "Generic function to compute predicted values and residuals using rx_lin_mod, rx_logit, rx_dtree, rx_dforest and rx_btrees objects.For specific functions, see:  rx_predict_default for rx_lin_mod and rx_logit objects. rx_predict_rx_dtree for rx_dtree object. rx_predict_rx_dforest for rx_dforest and rx_btrees objects." 
keywords: "predict" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "08/31/2017" 
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

# `rx_predict`


**Applies to: SQL Server 2017**


## Usage



```
revoscalepy.rx_predict(model_object=None, data=None, output_data=None, **kwargs)
```




## Description

Generic function to compute predicted values and residuals using
rx_lin_mod, rx_logit, rx_dtree, rx_dforest and rx_btrees objects.

For specific functions, see:
    rx_predict_default for rx_lin_mod and rx_logit objects.
    rx_predict_rx_dtree for rx_dtree object.
    rx_predict_rx_dforest for rx_dforest and rx_btrees objects.


## Arguments


### model_object

object returned from a call to rx_lin_mod, rx_logit, rx_dtree,
rx_dforest and rx_btrees. Objects with multiple dependent variables are not
supported.


### data

a data frame or an RxXdfData data source object to be used for predictions.


### output_data

an RxXdfData data source object or existing data frame to store predictions.


### kwargs

additional parameters


## Returns

a data frame or a data source object of prediction results.


## See also

[`rx_predict_default`](rx-predict-default.md),
[`rx_predict_rx_dtree`](rx-predict-rx-dtree.md),
[`rx_predict_rx_dforest`](rx-predict-rx-dforest.md).


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod, rx_predict, rx_data_step

sample_data_path = RxOptions.get_option("sampleDataDir")
mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))
mort_df = rx_data_step(mort_ds)

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", mort_df)
pred = rx_predict(lin_mod, data = mort_df)
print(pred.head())
```


Output:



```
Rows Read: 100000, Total Rows Processed: 100000, Total Chunk Time: 0.056 seconds 
Rows Read: 100000, Total Rows Processed: 100000, Total Chunk Time: Less than .001 seconds 
Computation time: 0.005 seconds.
Rows Read: 100000, Total Rows Processed: 100000, Total Chunk Time: 0.002 seconds 
   creditScore_Pred
0        700.089114
1        699.834355
2        699.783403
3        699.681499
4        699.783403
```



## Usage



```
revoscalepy.rx_predict_default(model_object=None, data: revoscalepy.datasource.RxDataSource.RxDataSource = None, output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, compute_standard_errors: bool = False, interval: typing.Union[list, str] = 'none', confidence_level: float = 0.95, compute_residuals: bool = False, type: typing.Union[list, str] = None, write_model_vars: bool = False, extra_vars_to_write: typing.Union[list, str] = None, remove_missings: bool = False, append: typing.Union[list, str] = None, overwrite: bool = False, check_factor_levels: bool = True, predict_var_names: typing.Union[list, str] = None, residual_var_names: typing.Union[list, str] = None, interval_var_names: typing.Union[list, str] = None, std_errors_var_names: typing.Union[list, str] = None, blocks_per_read: int = 0, report_progress: int = None, verbose: int = 0, xdf_compression_level: int = 0, compute_context: revoscalepy.computecontext.RxComputeContext.RxComputeContext = None, **kwargs)
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

a character string specifying the output ‘.xdf’ file, a
RxXdfData object, RxTextData object, a RxOdbcData data source, or a
RxSqlServerDta data source to store predictions.


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



## Usage



```
revoscalepy.rx_predict_rx_dforest(model_object=None, data: revoscalepy.datasource.RxDataSource.RxDataSource = None, output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, predict_var_names: list = None, write_model_vars: bool = False, extra_vars_to_write: list = None, append: typing.Union[list, str] = 'none', overwrite: bool = False, type: typing.Union[list, str] = None, cutoff: list = None, remove_missings: bool = False, compute_residuals: bool = False, residual_type: typing.Union[list, str] = 'usual', residual_var_names: list = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 0, xdf_compression_level: int = None, compute_context=None, **kwargs)
```




## Description

Calculate predicted or fitted values for a data set from an rx_dforest or rx_btrees object.


## Arguments


### model_object

object returned from a call to rx_dtree.


### data

a data frame or an RxXdfData data source object to be used for predictions.


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



## Usage



```
revoscalepy.rx_predict_rx_dtree(model_object=None, data: revoscalepy.datasource.RxDataSource.RxDataSource = None, output_data: typing.Union[revoscalepy.datasource.RxDataSource.RxDataSource, str] = None, predict_var_names: list = None, write_model_vars: bool = False, extra_vars_to_write: list = None, append: typing.Union[list, str] = 'none', overwrite: bool = False, type: typing.Union[list, str] = None, remove_missings: bool = False, compute_residuals: bool = False, residual_type: typing.Union[list, str] = 'usual', residual_var_names: list = None, blocks_per_read: int = None, report_progress: int = None, verbose: int = 0, xdf_compression_level: int = None, compute_context=None, **kwargs)
```




## Description

Calculate predicted or fitted values for a data set from an rx_dtree object.


## Arguments


### model_object

object returned from a call to rx_dtree.


### data

a data frame or an RxXdfData data source object to be used for predictions.


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

the type of prediction desired. Supported choices are: “vector”,
“prob”, “class”, and “matrix”.


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



## Example



```
import os
from revoscalepy import rx_dtree, rx_predict_rx_dtree, rx_import, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "kyphosis.xdf"))
kyphosis = rx_import(input_data = ds)

# classification
formula = "Kyphosis ~ Number + Start"
method = "class"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}
cost = [2,3]
dtree = rx_dtree(formula, data = kyphosis, pweights = "Age", method = method, parms = parms, cost = cost, max_num_bins = 100)
rx_pred = rx_predict_rx_dtree(dtree, data = kyphosis)
rx_pred.head()

# regression
formula = "Age ~ Number + Start"
method = "anova"
parms = {'prior': [0.8, 0.2], 'loss': [0, 2, 3, 0], 'split': "gini"}
cost = [2,3]
dtree = rx_dtree(formula, data = kyphosis, pweights = "Kyphosis", method = method, parms = parms, cost = cost, max_num_bins = 100)
rx_pred = rx_predict_rx_dtree(dtree, data = kyphosis)
rx_pred.head()
```

