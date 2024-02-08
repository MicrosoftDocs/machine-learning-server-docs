--- 

# required metadata 
title: "rxPredict function (revoAnalytics) | Microsoft Docs" 
description: "Predicted values and residuals for model objects built using RevoScaleR" 
keywords: "(revoAnalytics), rxPredict, rxPredict.default, methods, models, regression" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.service: mlserver
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 



 # rxPredict: Predicted values and residuals for model objects built using RevoScaleR 
 ## Description

Compute predicted values and residuals using the following objects: [rxLinMod](rxLinMod.md), [rxGlm](rxGlm.md), [rxLogit](rxLogit.md), [rxDTree](rxDTree.md), [rxBTrees](rxBTrees.md), and [rxDForest](rxDForest.md).

 ## Usage

```   
  rxPredict(modelObject, data = NULL, ...)
 ## S3 method for class `default':
rxPredict  (modelObject, data = NULL, outData = NULL,     
            computeStdErrors = FALSE, interval = "none", confLevel = 0.95,
            computeResiduals = FALSE, type = c("response", "link"), 
            writeModelVars = FALSE, extraVarsToWrite = NULL, removeMissings = FALSE,
            append = c("none", "rows"), overwrite = FALSE, checkFactorLevels = TRUE,
            predVarNames = NULL, residVarNames = NULL, 
            intervalVarNames = NULL, stdErrorsVarNames = NULL, predNames = NULL, 
            blocksPerRead = rxGetOption("blocksPerRead"),
            reportProgress = rxGetOption("reportProgress"), verbose = 0,
            xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ...) 

```

 ## Arguments

 ### `modelObject`
 object returned from a RevoScaleR model fitting function. Valid values include `rxLinMod`, `rxLogit`, `rxGlm`, `rxDTree`, `rxBTrees`, and `rxDForest`. Objects with multiple dependent variables are not supported in rxPredict. 



 ### `data`
 An [RxXdfData](RxXdfData.md) data source object to be used for predictions. If not using a distributed compute context such as RxHadoopMR, a data frame,  or a character string specifying the input .xdf file can also be used.  



 ### `outData`
 file or existing data frame to store predictions; can be same as the input file or `NULL`. If not `NULL`, a character string specifying  the output �.xdf� file, a RxXdfData object, a RxOdbcData data source, or a RxSqlServerData  data source. `outData` can also be a delimited [RxTextData](RxTextData.md) data source if using a native file system and not appending. 



 ### `computeStdErrors`
 logical value. If `TRUE`, the standard errors  for each dependent variable are calculated. 



 ### `interval`
 character string defining the type of interval calculation to perform. Supported values are `"none"`, `"confidence"`, and `"prediction"`. 



 ### `confLevel`
 numeric value representing the confidence level on the interval [0, 1]. 



 ### `computeResiduals`
 logical value. If `TRUE`, residuals are computed. 



 ### `type`
Applies to [rxGlm](rxGLM.md) and [rxLogit](rxLogit.md), used to set the type of prediction. Valid values are `"response"`and `"link"`. If `type = "response"`, the predictions are on the scale of the response variable. For instance, for  the binomial model, the predictions are in the range (0,1). If `type = "link"`, the predictions are on the scale of the linear predictors. Thus for the binomial model, the predictions are of log-odds. 

 ### `writeModelVars`
 logical value. If `TRUE`, and the output data set is different from the input data set, variables in the model will be written to the output data set in addition to the predictions (and residuals, standard errors, and confidence bounds, if requested). If variables from the input data set are transformed in the model, the transformed variables will also be included. 



 ### `extraVarsToWrite`
 `NULL` or character vector of additional variables names from the input data or transforms to include in the `outData`.  If `writeModelVars` is `TRUE`, model variables will be included as well. 



 ### `removeMissings`
 logical value. If `TRUE`, rows with missing values are removed. 



 ### `append`
  either `"none"` to create a new files or `"rows"` to append rows to an existing file.  If `outData` exists and `append` is `"none"`, the `overwrite` argument must be set to `TRUE`.  You can append only to [RxTeradata](RxTeradata.md) data source. Ignored for data frames.    



 ### `overwrite`
  logical value. If `TRUE`, an existing `outData` will be overwritten.  `overwrite` is ignored if appending rows. Ignored for data frames.  



 ### `checkFactorLevels`
 logical value. If `TRUE`, up to 1000 factor levels  for the data will be verified against factor levels in the model. Setting to `FALSE` can speed up computations if using lots of factors.  



 ### `predVarNames`
 character vector specifying name(s) to give to the prediction results 



 ### `residVarNames`
 character vector specifying name(s) to give to the residual results. 



 ### `intervalVarNames`
 `NULL` or a character vector defining low and high confidence interval variable names, respectively. If `NULL`, the strings `"_Lower"` and `"_Upper"` are appended to the dependent variable names to form the  confidence interval variable names. 



 ### `stdErrorsVarNames`
 `NULL` or a character vector defining variable names corresponding to the standard errors, if calculated. If `NULL`, the string `"_StdErr"` is appended to the dependent variable names to form the  standard errors variable names. 



 ### `predNames`
 character vector specifying name(s) to give to the prediction and residual results; if length is 2, the second name is used for residuals. This argument is deprecated and `predVarNames` and `residVarNames` should be used instead. 



 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. If the `data` and `outData` are the same file, blocksPerRead must be 1. 



 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 




 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 



 ### `xdfCompressionLevel`
 integer in the range of -1 to 9 indicating the compression  level for the output data if written to an `.xdf` file.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 



 ### ` ...`
 additional arguments to be passed directly to the Revolution Compute Engine. 




 ## Details

`rxPredict` computes predicted values and/or residuals from an existing
model type. The most common way to call rxPredict is `rxPredict(modelObject, data, outData)`. Typically, all the other arguments are left at their defaults.

For rxLogit, the residuals are equivalent to those
computed for `glm` with `type` set to `"response"`, e.g.,
`residuals(glmObj, type="response")`.

If the `data` is the same data used to create the `modelObject`, the
predicted values are the fitted values for the original model.

If the `data` specified is an .xdf file, the `outData` must
be `NULL` or an .xdf file. If `outData` is an .xdf
file, the computed data will be appended as columns. If `outData` is
`NULL`, the computed columns will be appended to the `data`
.xdf file.

If the `data` specified is a data frame, the `outData` must be
`NULL` or a data frame. If `outData` is a data frame, a copy of the
data frame with the new columns appended will be returned. If `outData`
is `NULL`, a vector or list of the computed values will be returned.

If a transformation function is being used for the model estimation,
the information variable `.rxIsPrediction` can be used to
exclude computations for the dependent variable when running
`rxPredict`.  See [rxTransform](rxTransform.md) for an example.



 ## Value

If a data frame is specified as the input `data`, a data frame is returned.
If a data frame is specified as the `outData`, variables containing the
results are added to the data frame and it is returned. 
If `outData` is `NULL`, a data frame containing
the predicted values (and residuals and standard errors, if requested) is returned.

If an .xdf file is specified as the input `data`, an [RxXdfData](RxXdfData.md)
data source object is returned that can be used in subsequent RevoScaleR analyses.
If `outData` is an .xdf file, the [RxXdfData](RxXdfData.md) 
data source represents the `outData` file.  If `outData` is `NULL`,
the predicted values (and, if requested, residuals) are appended to the original
`data` file. The returned [RxXdfData](RxXdfData.md) object represents this file.


 ## Computing Standard Errors of Predicted Values 



Use `computeStdErrors` to control whether or not prediction standard errors are computed.

Use `interval` to control whether confidence or prediction (tolerance) intervals are computed at the specified level (`confLevel`). 
These are sometimes referred to as *narrow* and *wide* intervals, respectively.

Use `stdErrorsVarNames` to name the standard errors output variable 
and `intervalVarNames` to specify the output variable names
of the lower and upper confidence/tolerance intervals. 

In calculating the prediction standard errors, keep the following in mind:




* 
 Prediction standard errors are available for both [rxLinMod](rxLinMod.md) and [rxLogit](rxLogit.md) models.


* 
 Standard errors are computationally intensive for large models, i.e., those involving a large number of model parameters.


* 
 [rxLinMod](rxLinMod.md) and [rxLogit](rxLogit.md) must be called with `covCoef = TRUE` because the variance-covariance
matrix of the coefficients must be available.


* 
 Cube regressions are not supported (`cube = TRUE`).


* 
 Multiple dependent variables are currently not supported.


* 
 For [rxLogit](rxLogit.md), `interval = "confidence"` is supported (unlike predict.glm, 
which does not support confidence bounds), but `interval = "prediction"` is not supported.


* 
 If residuals are requested, and if there are missing values in the dependent variable, 
then all computed values (prediction, standard errors, confidence levels) will be
assigned the value missing, and will be removed if `removeMissings = TRUE`. 
If no residuals are requested, then missings in the dependent variable (which need not exist
in the data) have no effect.




 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[rxLinMod](rxLinMod.md),
[rxLogit](rxLogit.md),
[rxGlm](rxGLM.md),
[rxPredict.rxDTree](rxPredict.rxDTree.md),
[rxPredict.rxDForest](rxPredict.rxDForest.md),
[rxPredict.rxNaiveBayes](rxPredict.rxNaiveBayes.md).

 ## Examples

 ```

  # Load the built-in iris data set and predict sepal length
  myIris <- iris
  myIris[1:5,]
  form <- Sepal.Length ~ Sepal.Width + Petal.Length + Petal.Width + Species
  irisLinMod <- rxLinMod(form, data = myIris)
  myIrisPred <- rxPredict(modelObject = irisLinMod, data = myIris)
  myIris$SepalLengthPred <- myIrisPred$Sepal.Length_Pred
  myIris[1:5,]
  irisResiduals<- rxPredict(modelObject = irisLinMod, data = myIris, computeResiduals = TRUE)
  names(irisResiduals)

  # Use sample data to compare lm and glm results with rxPredict
  sampleDataDir <- rxGetOption("sampleDataDir")
  mortFile <- file.path(sampleDataDir, "mortDefaultSmall.xdf")
  linModPredictFile <- file.path(tempdir(), "mortPredictLinMod.xdf")
  logitPredictFile <- file.path(tempdir(), "mortPredictLogit.xdf")
  mortDF <- rxDataStep(inData = mortFile)

  # Compare residuals from rxLinMod with lm
  linMod <- rxLinMod(creditScore ~ yearsEmploy, data = mortFile)
  rxPredict(modelObject = linMod, data = mortFile, outData = linModPredictFile,
            computeResiduals = TRUE)
  residDF <- rxDataStep(inData = linModPredictFile)
  mortLM <- lm(creditScore ~ yearsEmploy, data = mortDF)
  # Sum of differences should be very small
  sum(mortLM$residuals - residDF$creditScore_Resid)

  # Create logit model object and compute predictions and residuals
  logitModObj <- rxLogit(default ~ creditScore, data = mortFile)
  rxPredict(modelObject = logitModObj, data = mortFile,
            outData = logitPredictFile, computeResiduals = TRUE)
  residDF <- rxDataStep(inData = logitPredictFile)
  mortGLM <- glm(default ~ creditScore, data = mortDF, family = binomial())

  # maximum differences should be very small
  max(abs(mortGLM$fitted.values - residDF$default_Pred)) 
  max(abs(residuals(mortGLM, type = "response") - residDF$default_Resid))
```




