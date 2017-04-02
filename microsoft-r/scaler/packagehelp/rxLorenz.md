--- 
 
# required metadata 
title: " Lorenz Curve and Gini Coefficient " 
description: " Compute and plot an empirical Lorenz curve from a variable in a data set, optionally specifiying a separate variable from which to compute the y-values for the curve. Compute the Gini Coefficient from the Lorenz curve data. Appropriate for big data sets since data is binned with computations performed in one pass, rather than sorting the data as part of the computation process. " 
keywords: "RevoScaleR, rxLorenz, rxGini, rxGini.rxLorenz, plot.rxLorenz,  univar " 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 
 
 #`rxLorenz`:  Lorenz Curve and Gini Coefficient 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Compute and plot an empirical Lorenz curve from a variable in a data set, optionally
specifiying a separate variable from which to compute the y-values for the curve.
Compute the Gini Coefficient from the Lorenz curve data. Appropriate for
big data sets since data is binned with computations performed in one pass,
rather than sorting the data as part of the computation process.
 
 
 ##Usage

```   
  rxLorenz(orderVarName, valueVarName = orderVarName, data, numBreaks = 1000,
  	pweights = NULL, fweights = NULL, blocksPerRead = 1, 
  	reportProgress = 1, verbose = 0)
  	
 ## S3 method for class `rxLorenz':
rxGini  ( x )
  
 ## S3 method for class `rxLorenz':
plot  (x, title = NULL, subtitle = NULL, 
  	xTitle = NULL, yTitle = NULL, lineColor = NULL,
  	lineStyle = "solid", lineWidth = 2, equalityGridLine = TRUE, 
  	equalityColor = "grey75", equalityStyle = NULL, equalityWidth = 2, ...)	
 
```
 
 
 ##Arguments

   
    
 ### `orderVarName`
  A character string with the name of the variable to use in computing approximate quantiles.  
  
    
 ### `valueVarName`
  A character string with the name of the variable to use to compute the mean values per quantile. Can be the same as `orderVarName`.  
  
    
 ### `data`
  data frame, character string containing an .xdf file name (with path), or  [RxDataSource-class](RxDataSource-class.md) object representing a data set containing the actual and observed variables.  
  
    
 ### `numBreaks`
  integer specifiying the number of breaks to use in comuting approximate quantiles.   
  
    
 ### `pweights`
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### `fweights`
 character string specifying the variable to use as frequency weights for the observations. 
  
  
  
 ### `blocksPerRead`
  number of blocks to read for each chunk of data read from the data source.  
  
    
 ### `reportProgress`
  integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional information is printed as summary statistics are computed. 
  
  
  
 ### `x`
 output object from rxLorenz function. 
  
  
    
 ### `title`
 main title for the plot.    
  
  
    
 ### `subtitle`
 subtitle (at the bottom) for the plot.   
  
  
    
 ### `xTitle`
 title for the X axis.  
  
  
    
 ### `yTitle`
 title for the Y axis.  
  
  
    
 ### `lineColor`
 character or integer vector specifying line color for the Lorenz curve. See colors for a list of available colors.  
  
  
    
 ### `lineStyle`
 line style for line plot: `"blank"`, `"solid"`, `"dashed"`, `"dotted"`, `"dotdash"`, `"longdash"`, or `"twodash"`. Specify `"blank"` for no line, or set `type` to `"p"`.  
  
  
    
 ### `lineWidth`
 a positive number specifiying the line width for line plot.  The interpretation is device-specific.  
  
  
    
 ### `equalityGridLine`
 logical value.  If `TRUE`, a diagonal grid line will be drawn representing complete equality.  
  
  
    
 ### `equalityColor`
 character or integer vector specifying line color for the equality grid line. If `NULL`, the color of other grid lines will be used.  
  
  
    
 ### `equalityStyle`
 line style for the equality grid line: `"blank"`, `"solid"`, `"dashed"`, `"dotted"`, `"dotdash"`, `"longdash"`, or `"twodash"`. If `NULL`, the style of other gride lines will be used.  
  
  
    
 ### `equalityWidth`
 a positive number specifiying the line width for line plot.  If `NULL`, the width of other grid lines will be used.  
  
  
    
 ### ` ...`
  Additional arguments to be passed to `xyplot`.  
  
 
 
 ##Details
 
`rxLorenz` computes the cumulative percentage values of the variable
specified in `valueVarName` for groups binned by the `orderVarname`. The
size of the bins is determined by `numBreaks`.

When plotted, the cumulative percentage values are plotted against the quantile percentages.

The Gini coefficient is computed by estimating the ratio of the area between the line of equality 
and the Lorenz curve to the total area under the line of equality (using trapezoidal integration). 
The Gini coefficient can range from 0 to 1, with 0 representing perfect equality.

Precision can be increased by increasing `numBreaks`. 
 
 
 ##Value
 
`rxLorenz` returns a data frame of class `"rxLorenz"` containing two
variables: `cumVals` and `percent`.  It also may
have a `"description"` attribute containing the value variable name or
description.

`rxGini` returns a numeric vector of length one containing the approximate Gini coefficient.
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 ##See Also
 
[rxPredict](rxPredict.md),
[rxLogit](rxLogit.md),
[rxGlm](rxGLM.md), 
[rxLinePlot](rxLinePlot.md),
[rxQuantile](rxQuantile.md),
[rxRoc](rxRoc.md).
   
 ##Examples

 ```
   
  
  ########################################################################
  # Example using simple data frames for extreme distributions
  ########################################################################
  # Lorenz curve for complete equality
  testData <- data.frame(income = rep(100, times=10))
  lorenzOut1 <- rxLorenz("income", data = testData, numBreaks = 100)
  plot(lorenzOut1)
  rxGini(lorenzOut1)
  	
  # Extreme inequality
  testData <- data.frame(income = c(rep(0, times=99), 100))
  lorenzOut2 <- rxLorenz("income", data = testData, numBreaks = 100)
  plot(lorenzOut2, equalityWidth = 3, equalityColor = "black")
  rxGini(lorenzOut2)
  
  ########################################################################
  # Example using xdf file from sample data
  ########################################################################
  
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers") 
  
  # Compute Lorenz data using probability weights
  lorenzOut <- rxLorenz(orderVarName = "incwage", data = censusWorkers,
  	pweights = "perwt")
  
  # Plot the Lorenz Curve
  lorenzPlot <- plot(lorenzOut, 
  	title = "Lorenz Curve for Workers from Three States",
  	subtitle = "Data Source: 5 Percent Sample of U.S. 2000 Census",
  	lineWidth = 3, equalityColor = "black", equalityStyle = "longdash")
  
  # Compute the Gini Coefficient
  giniCoef <- rxGini(lorenzOut)
 
```
 
 
 
