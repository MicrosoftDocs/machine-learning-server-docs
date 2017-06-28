--- 
 
# required metadata 
title: "Line Plot" 
description: " Line or scatter plot using data from an .xdf file or data frame - a wrapper function for xyplot. " 
keywords: "RevoScaleR, rxLinePlot, hplot" 
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
 
 
 #`rxLinePlot`: Line Plot

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Line or scatter plot using data from an .xdf file or data frame - a wrapper
function for xyplot.
 
 
 ##Usage

```   
  rxLinePlot(formula, data, rowSelection = NULL, 
             transforms = NULL, transformObjects = NULL,
             transformFunc = NULL, transformVars = NULL,
             transformPackages = NULL, transformEnvir = NULL, 
             blocksPerRead = rxGetOption("blocksPerRead"), type = c("l"),
             title = NULL, subtitle = NULL, xTitle = NULL, yTitle = NULL,
             xNumTicks = 10, yNumTicks = 10, legend = TRUE, lineColor = NULL,
             lineStyle = "solid", lineWidth = 2, symbolColor = NULL,
             symbolStyle = "solid circle", symbolSize = NULL,
             plotAreaColor = "gray90", gridColor = "white", gridLineWidth = 1,
             gridLineStyle = "solid",
             reportProgress = rxGetOption("reportProgress"), print = TRUE, ...) 
 
```
 
 
 ##Arguments

   
    
 ### `formula`
 formula for plot, taking the form of `y~x|g1 + g2`where `g1` and `g2` are optional conditioning factor variables. 
  
  
    
 ### `data`
 either an RxXdfData object, a character string specifying the .xdf file, or a data frame containing the data to plot. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing variable transformations required for the formula or rowSelection. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from  the data source. 
  
  
    
 ### `type`
 character vector specifying the type of plot. Use `"l"` for line, `"p"` for points, `"b"` for both, `"smooth"` for a  loess fit, `"s"` for stair steps, or `"r"` for a regression line. 
  
  
    
 ### `title`
 main title for the plot.  Alternatively `main` can be used. 
  
  
    
 ### `subtitle`
 subtitle (at the bottom) for the plot.  Alternatively `sub` can be used. 
  
  
    
 ### `xTitle`
 title for the X axis. Alternatively `xlab` can be used. 
  
  
    
 ### `yTitle`
 title for the Y axis. Alternatively `ylab` can be used. 
  
  
    
 ### `xNumTicks`
 number of tick marks for numeric X axis. 
  
  
    
 ### `yNumTicks`
 number of tick marks for numeric Y axis. 
  
  
    
 ### `legend`
 logical value. If `TRUE` and more than one line or set of symbols is plotted, a legend is is created. 
  
  
    
 ### `lineColor`
 character or integer vector specifying line colors for the plot. See colors for a list of available colors. 
  
  
    
 ### `lineStyle`
 line style for line plot: `"blank"`, `"solid"`, `"dashed"`, `"dotted"`, `"dotdash"`, `"longdash"`, or `"twodash"`. Specify `"blank"` for no line, or set `type` to `"p"`. 
  
  
    
 ### `lineWidth`
 a positive number specifiying the line width for line plot.  The interpretation is device-specific. 
  
  
    
 ### `symbolColor`
 character or integer vector denoting the symbol colors. See colors for a list of available colors. 
  
  
    
 ### `symbolStyle`
 character or integer vector denoting the symbol style(s): `"circle"`, `"solid circle"`, `"square"`, `"solid square"`, `"triangle"`, `"solid triangle"`, `"diamond"`, `"solid diamond"`, `"plus"` (3), `"cross"` (4), `"down triangle"` (6), `"square x"` (7), `"star"` (8), `"diamond +"` (9), `"circle +"` (10), `"up down triangle"` (11), `"square +"` (12), `"circle x"` (13), or any single character. 
  
  
    
 ### `symbolSize`
 numerical value giving the amount by which symbols should be magnified. 
  
  
    
 ### `plotAreaColor`
 background color for the plot area and legend. 
  
  
    
 ### `gridColor`
 color for grid lines. See colors for a list of available colors. 
  
  
    
 ### `gridLineWidth`
 line width for grid lines. 
  
  
    
 ### `gridLineStyle`
 line style for grid lines: `"blank"`, `"solid"`, `"dashed"`, `"dotted"`, `"dotdash"`, `"longdash"`, or `"twodash"`. 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
    
 ### `print`
 logical. If `TRUE`, the plot is printed. If `FALSE`, and the **lattice** package is loaded, an xyplot object is returned invisibly and can be printed later.  Note that the printing of the legend or key will depend on the trellis.par.getsettings at the time of printing. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the underlying xyplot function if loaded. 
  
 
 
 ##Details
 
`rxLinePlot` is enhanced by the recommended **lattice** package.
 
 
 
 ##Value
 
an xyplot object if the **lattice** is loaded.
Otherwise `NULL`.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
xyplot,
[rxHistogram](rxhistogram.md).
   
 ##Examples

 ```
   
  
  # Create a simple scatter plot using a data frame
  rxLinePlot(Ozone ~ Temp, data=airquality, type="p") 
  		
  # Create the file + path name for the sample data set
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers")
  
  # Compute the weighted counts for age and sex by state
  ageSex <- rxCube(~ F(age) : sex : state, data = censusWorkers, pweights = "perwt")
  
  # Convert the results to a data frame ready for plotting
  ageSex <- rxResultsDF(ageSex)
  	
  # Draw a line plot of the results in 3 panels (with lattice package loaded)
  rxLinePlot(Counts ~ age | state, groups = sex, data = ageSex,
  	title = "2000 U.S. Workers by Age and Sex for 3 States")
  
  # Use transformation expressions to scale the 
  # Counts and age variables
  rxLinePlot(Counts ~ age | state, groups = sex, data = ageSex,
  	transforms=list(Counts = log(Counts), age = age/10), 
  	xlab="age / 10", ylab="log(Counts)",
  	title = "2000 U.S. Workers by Age and Sex for 3 States")
  
  # Use both a line and symbols in the plot
  rxLinePlot(Counts ~ age | state, groups = sex, data = ageSex, type = "b",
  	title = "2000 U.S. Workers by Age and Sex for 3 States")
  
  # Create a plot using a subselection of data from an .xdf file
  # Look at Arrival Delay be Departure Time for flights more
  # than an hour late and less than 3 hours late on Tuesday
  airlineData <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall")
  rxLinePlot(ArrDelay~CRSDepTime, data=airlineData, 
     rowSelection= ArrDelay > 60.0 & ArrDelay <= 180.00 & DayOfWeek == "Tuesday",
     type=c( "p", "smooth"), lineColor="red")
     
  # Customize a plot with a panel function (using the lattice package)
  if ("lattice" %in% .packages())	
  {
  	rxLinePlot(Ozone ~ Temp, data = airquality, type = "p",
    		panel = function(x, y, ...)
   		{
  			panel.spline(x,y, lwd = 3, col = 3)
  		})  
  }
  		 
 
```
 
 
