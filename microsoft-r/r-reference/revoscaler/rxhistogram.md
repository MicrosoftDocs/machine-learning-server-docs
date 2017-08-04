--- 
 
# required metadata 
title: "Histogram" 
description: " Histogram plot for a variable in an .xdf file or data frame " 
keywords: "RevoScaleR, rxHistogram, hplot" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 #rxHistogram: Histogram

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Histogram plot for a variable in an .xdf file or data frame
 
 
 ##Usage

```   
  rxHistogram(formula, data, pweights = NULL, fweights = NULL, numBreaks = NULL,
              startVal = NULL, endVal = NULL, levelsToDrop = NULL,
              levelsToKeep = NULL, rowSelection = NULL, transforms = NULL,
              transformObjects = NULL, transformFunc = NULL, transformVars = NULL, 
              transformPackages = NULL, transformEnvir = NULL,
              blocksPerRead = rxGetOption("blocksPerRead"), 
              histType = "Counts", 
              title = NULL, subtitle = NULL, xTitle = NULL, yTitle = NULL,
              xNumTicks = NULL, yNumTicks = NULL, xAxisMinMax = NULL,
              yAxisMinMax = NULL, fillColor = "cyan", lineColor = "black",
              lineStyle = "solid", lineWidth = 1, plotAreaColor = "gray90",
              gridColor = "white", gridLineWidth = 1, gridLineStyle = "solid",
              maxNumPanels = 100, reportProgress = rxGetOption("reportProgress"),
              print = TRUE, ...) 
 
```
   	
 ##Arguments

   
    
 ### formula
 formula describing the data to plot. It should take the  form of `~x|g1 + g2` where `g1` and `g2` are optional  conditioning factor variables and x is the name of a variable or an on-the-fly factorization F(x). Other expressions of x are not supported. 
  
  
    
 ### data
 either an RxXdfData object, a character string specifying the .xdf file, or a data frame containing the variable to plot. 
  
  
    
 ### pweights
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### fweights
 character string specifying the variable to use as frequency weights for the observations. 
  
  
    
 ### numBreaks
 number of breaks to use to cut numeric data, including  the upper and lower bounds. 
  
  
    
 ### startVal
 low value used for cutting numeric data. 
  
  
    
 ### endVal
 high value used for cutting numeric data. 
  
  
    
 ### levelsToDrop
 levels to exclude if the histogram variable is a factor. 
  
  
    
 ### levelsToKeep
 levels to keep if the histogram variable is a factor. 
  
  
    
 ### rowSelection
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### transforms
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### transformObjects
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### transformFunc
 variable transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### transformVars
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### transformPackages
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### transformEnvir
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### blocksPerRead
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### histType
 character string specifying `"Counts"` or `"Percent"`. 
  
  
    
 ### title
 main title for the plot.  Alternatively `main` can be used. 
  
  
    
 ### subtitle
 subtitle (at the bottom) for the plot.  Alternatively `sub` can be used. 
  
  
    
 ### xTitle
 title for the X axis. Alternatively `xlab` can be used. 
  
  
    
 ### yTitle
 title for the Y axis. Alternatively `ylab` can be used. 
  
  
    
 ### xNumTicks
 number of tick marks on X axis (ignored for factor variables). 
  
  
    
 ### yNumTicks
 number of tick marks on Y axis. 
  
  
    
 ### xAxisMinMax
 numeric vector of length 2 containing a minimum and maximum value  for the X axis. Alternatively `xlim` can be used. 
  
  
    
 ### yAxisMinMax
 numeric vector of length 2 containing a minimum and maximum value  for the Y axis. Alternatively `ylim` can be used. 
  
  
    
 ### fillColor
 fill color for histogram. Use colors to see color names. 
  
  
    
 ### lineColor
 line color for border of histogram. 
  
  
    
 ### lineStyle
 line style for border of histogram: `"blank", "solid", "dashed",  ``"dotted", "dotdash", "longdash",` or `"twodash"`. 
  
  
    
 ### lineWidth
 line width for border of histogram. Alternatively `lwd` can be used. 
  
  
    
 ### plotAreaColor
 background color for the plot area. 
  
  
    
 ### gridColor
 color for grid lines. 
  
  
    
 ### gridLineWidth
 line width for grid lines. 
  
  
    
 ### gridLineStyle
 line style for grid lines. 
  
  
    
 ### maxNumPanels
 integer specifying the maximum number of panels to plot. The number of panels is determined by the product of the number of levels of each conditioning variable.  If the number of panels exceeds the maxNumPanels an error is given and the plot is not drawn. If maxNumPanels is NULL, it is ignored. 
  
  
    
 ### reportProgress
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### print
 logical. If `TRUE`, the plot is printed. If `FALSE`, and the **lattice** package is loaded, an **lattice** plot object   is returned invisibly and can be printed later.  
  
  
    
 ###  ...
 additional arguments to be passed directly to the underlying `barchart` or `xyplot` function. 
  
 
 
 
 ##Details
 
`rxHistogram` calls [rxCube](rxcube.md) to perform computations and uses
the **lattice** graphics package (barchart or
xyplot) to create the plot. The `rxHistogram` 
function will attempt bin continuous data in reasonable intervals.  For
faster computation (using a bin for every integer value), use
the F() function around the variable. Descriptive argument names
are used to facilitate quick and easy plotting and self-documenting code
for new R users.
 
 
 ##Value
 
An object of class "trellis". It is automatically printed within the function.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxLinePlot](rxlineplot.md),
[rxCube](rxcube.md),
histogram.
   
 ##Examples

 ```
   
  # Examples using airline data
  airlineData <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.xdf")
  # Use the F() function to quickly compute bins for each integer level
  rxHistogram(~F(CRSDepTime), data = airlineData)
  # Specify the approximate number of breaks
  rxHistogram(~CRSDepTime, numBreaks=11, data = airlineData)
  
  # Examples using census data subsample
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
  # Create panels for each of the 3 states
  rxHistogram(~ sex | state, data = censusWorkers)
  # Repeat, printing x axis labels at an angle, and all panels in a row
  rxHistogram(~ sex | state, scales = list(x = list(rot = 30)), 
      data = censusWorkers, layout = c(3,1))
  # Create panels for age for each sex for each state
  rxHistogram(~ age | sex + state, data = censusWorkers)
  # Specify how wage income should be broken into bins
  rxHistogram(~ incwage | state + sex, title="Wage Income Up To 100,000", 
    endVal = 100000, numBreaks=21, data = censusWorkers)
  
  # Show panels for each state on a separate page
  numCols <- 1
  numRows <- 2
  ## Not run:
 
par(ask=TRUE) # Set ask to pause between each plot
 ## End(Not run) 
  
  rxHistogram(~ age | sex + state, data = censusWorkers, layout=c(numCols, numRows)) 
  
  # Create a jpeg file for each page, named myplot001.jpeg, etc
  ## Not run:
 
jpeg(file="myplot
rxHistogram(~ age | sex + state, data = censusWorkers, 
   blocksPerRead=6, layout=c(numCols, numRows)) 
dev.off()
 ## End(Not run) 
  
 
```
 
 
