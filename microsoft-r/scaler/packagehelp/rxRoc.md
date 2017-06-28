--- 
 
# required metadata 
title: " Receiver Operating Characteristic (ROC) computations and plot " 
description: " Compute and plot an ROC curve using actual and predicted values from binary classifier system " 
keywords: "RevoScaleR, rxRoc, rxRocCurve, rxAuc, as.data.frame.rxRoc, plot.rxRoc, rxAuc.rxRoc, hplot" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 
 
 
 #`rxRoc`:  Receiver Operating Characteristic (ROC) computations and plot 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Compute and plot an ROC curve using actual and predicted values from
binary classifier system
 
 
 ##Usage

```   
  rxRoc(actualVarName, predVarNames, data, numBreaks = 100, 
      removeDups = TRUE, blocksPerRead = 1, reportProgress = 0)
  
  rxRocCurve(actualVarName, predVarNames, data, numBreaks = 100, 
      blocksPerRead = 1, reportProgress = 0, computeAuc = TRUE, title = NULL,
      subtitle = NULL, xTitle = NULL, yTitle = NULL, legend = NULL,
      chanceGridLine = TRUE, ...)
  
 ## S3 method for class `rxRoc':
as.data.frame  ( x, ..., var = NULL)
  
 ## S3 method for class `rxRoc':
rxAuc  ( x )
  
 ## S3 method for class `rxRoc':
plot  (x, computeAuc = TRUE, title = NULL, subtitle, 
      xTitle = NULL, yTitle = NULL, legend = NULL, chanceGridLine = TRUE, ...)
 
```
 
 
 ##Arguments

   
    
 ### `actualVarName`
  A character string with the name of the variable containing actual (observed) binary values.  
  
    
 ### `predVarNames`
  A character string or vector of character strings with the name(s) of the variable  containing predicted values in the [0,1] interval.  
  
    
 ### `data`
  data frame, character string containing an .xdf file name (with path), or  [RxXdfData](RxXdfData.md) object representing an .xdf file containing the actual and observed variables.  
  
    
 ### `numBreaks`
  integer specifying the number of breaks to use to determine thresholds for computing the true and false positive rates.   
  
    
 ### `removeDups`
  logical; if `TRUE`, rows containing duplicate entries for sensitivity and specificity will be removed from the returned data frame. If performing computations for more than one prediction variable, this implies that there may be a different number of rows for each prediction variable.  
  
    
 ### `blocksPerRead`
  number of blocks to read for each chunk of data read from the data source.  
  
    
 ### `reportProgress`
  integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `computeAuc`
 logical value. If `TRUE`, the AUC is computed for each prediction variable and printed  in the subtitle or legend text.  
  
    
 ### `title`
 main title for the plot.  Alternatively `main` can be used. If `NULL` a default title will be created.  
  
  
    
 ### `subtitle`
 subtitle (at the bottom) for the plot.   If `NULL` and `computeAuc` is `TRUE`, the AUC for a single prediction variable will be computed and printed in the subtitle.    
  
  
    
 ### `xTitle`
 title for the X axis. Alternatively `xlab` can be used. If `NULL`, a default X axis title will be used.  
  
  
    
 ### `yTitle`
 title for the Y axis. Alternatively `ylab` can be used. If `NULL`, a default Y axis title will be used.  
  
    
 ### `legend`
 logical value. If `TRUE` and more than one prediction variable is specified, a legend is is created. If `computeAuc` is `TRUE`, the AUC is computed for each prediction variable and printed in the legend text.  
  
  
    
 ### `chanceGridLine`
 logical value. If `TRUE`, a grid line from (0,0) to (1,1) is added to represent a pure chance model.  
  
  
    
 ### `x`
 an rxRoc object.  
  
   
     
 ### `var`
 an integer or character string specifying the prediction variable for which to extract data frame containing the ROC computations. If an integer is specified, it will use that as an index to an alphabetized list of `predictionVarNames`. If `NULL`, all of the computed data will be returned in a data frame.  
  
    
 ### ` ...`
 additional arguments to be passed directly to an underlying function. For plotting functions, these are passed to the  xyplot function.  
  
  
 
 
 ##Details
 
`rxRoc` computes the sensitivity (true positive rate) and specificity 
(true negative rate) using a variable containing actual (observed) zero and one 
values and a variable containing predicted values in the unit interval as the 
discrimination threshold is varied. The thresholds are determined by the
`numBreaks` argument. The computations are done on chunks of data, so
that they can be performed on very large data sets. If more than one
prediction variable is specified, the computations will be performed for
each prediction variable. Observations that have a missing value for
the actual value or any of the prediction values are removed before
computations are performed.

`rxRocCurve` and the S3 `plot` method for an `rxRoc` object plot
the computed sensitivity (true positive rate) versus 1 - specificity 
(false positive rate). ROC curves were first used during World War II for 
detecting enemy objects in battle fields.
 
 
 ##Value
 
`rxRoc` returns a data frame of class `"rxRoc"` containing four
variables: `threshold`, `sensitivity`, `specificity`, and
`predVarName` (a factor variable containing the prediction variable name).

The `rxAuc` S3 method for an `rxRoc` object returns the AUC (area
under the curve) summary statistic.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 ##See Also
 
[rxPredict](../../r-reference/microsoftml/rxpredict.md),
[rxLogit](rxLogit.md),
[rxGlm](../../r-reference/revoscaler/rxglm.md), 
[rxLinePlot](rxLinePlot.md).
   
 ##Examples

 ```
   
  ########################################################################
  # Example using simple created actual and prediction data
  ########################################################################
  # Create a data frame with made-up actual and predicted values
  sampleDF <- data.frame(actual = c(0,0,0,0,0, 1,1,1,1,1))
  sampleDF$prediction <- c(.6, .5, .4, .3, .2, .8, .7, .6, .5, .4)
  # Add predictions that are all wrong and all right
  sampleDF$wrongPrediction <- c(.99, .99, .99, .99, .99, .01, .01, .01, .01, .01)
  sampleDF$rightPrediction <- c( .01, .01, .01, .01, .01,.99, .99, .99, .99, .99)
  # Compute the ROC information for all three prediction variables
  rocOut <- rxRoc(actualVarName = "actual", predVarNames = 
  	c("prediction", "wrongPrediction", "rightPrediction"), 
  	data = sampleDF, numBreaks = 10)
  # View the computed sensitivity and specificity
  rocOut
  # Plot the results
  plot(rocOut, title = "ROC Curve for Simple Data",
  	lineStyle = c("solid", "twodash", "dashed"))
  ########################################################################
  #  Example using data frame with one predicted variable
  ########################################################################
  # Estimate a logistic regression model using the internal 'infert' data
  rxLogitOut <- rxLogit(case ~ spontaneous + induced, data=infert )
  
  # Compute predictions for the model, creating a new data frame with
  # predictions and the original data used to estimate the model
  rxPredOut <- rxPredict(modelObject = rxLogitOut, data = infert, 
  	writeModelVars = TRUE, predVarNames = "casePred1")
  
  # Compute the ROC data for the default number of thresholds	
  rxRocObject <- rxRoc(actualVarName = "case", predVarNames = c("casePred1"), 
      data = rxPredOut)
  
  # Draw the ROC curve
  plot(rxRocObject)
  
  #########################################################################
  #  Example using a data frame with two predicted variables and rxRocCurve
  #########################################################################
  
  # As in first example, estimate a logistic regression model and
  # compute predictions
  
  logitOut1 <- rxLogit(case ~ spontaneous + induced, data=infert )
  
  predOut <- rxPredict(modelObject = logitOut1, data = infert, 
  	writeModelVars = TRUE, predVarNames = "Model1")
  
  # Estimate another model, and add predictions to prediction data frame
  logitOut2 <- rxLogit(case ~ spontaneous + induced + parity, data=infert )
  predOut <- rxPredict(modelObject = logitOut2, data = infert,
  		outData = predOut, predVarNames = "Model2")
  		
  # Do computations and plot ROC curve
  rxRocCurve(actualVarName = "case", predVarNames = c("Model1", "Model2"),
    data = predOut,
    title = "ROC Curves for 'case', including 'parity' in Model2") 
    
  #########################################################################
  #  Example using xdf files 
  #########################################################################  
  mortXdf <- file.path(rxGetOption("sampleDataDir"), "mortDefaultSmall")
  
  logitOut1 <- rxLogit(default ~ creditScore + yearsEmploy + ccDebt, 
          data = mortXdf, blocksPerRead = 5)
  
  predFile <- tempfile(pattern = ".rxPred", fileext = ".xdf")
   
  # predOutXdf will be a data source object representing the
  # prediction xdf file (predFile)
  predOutXdf <- rxPredict(modelObject = logitOut1, data = mortXdf, 
      writeModelVars = TRUE, predVarNames = "Model1", outData = predFile)
  
  # Estimate a second model without ccDebt
  logitOut2 <- rxLogit(default ~ creditScore + yearsEmploy, 
      data = predOutXdf, blocksPerRead = 5)
  
  # Add predictions to prediction data file
  predOutXdf <- rxPredict(modelObject = logitOut2, data = predOutXdf, 
      predVarNames = "Model2")
  
  rxRocCurve(actualVarName = "default", 
      predVarNames = c("Model1", "Model2"), 
      data = predOutXdf)
  
  # Remove temporary file storing predictions		
  file.remove(predFile)  
  
 
```
 
 
 
