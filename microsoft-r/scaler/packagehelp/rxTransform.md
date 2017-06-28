--- 
 
# required metadata 
title: "Dynamic Variable Transformation Functions" 
description: " Description of the recommended method for creating *on the fly* variables. " 
keywords: "RevoScaleR, rxTransform, transformFunc, transformVars, models" 
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
 
 
 
 
 #`rxTransform`: Dynamic Variable Transformation Functions

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Description of the recommended method for creating *on the fly*
variables.
 
 
 ##Details
 
Analytical functions within the **RevoScaleR** package use a formal
transformation function framework for generating *on the fly* variables
as opposed to the traditional R method of including transformations directly
in a `formula` statement. The **RevoScaleR** approach is to use the
following analytical function arguments to create new named variables that can
be referenced by name within a `formula` statement:


* `transformFunc`: R function whose first argument and return value are named R lists with equal length vector elements. The output list can contain modifications to the input elements as well as newly named elements. R lists, instead of R data frames, are used to minimize function execution timings, implying that row name information will not be available during calculations.


* `transformVars`: character vector selecting the names of the variables to be represented as list elements for the input to `transformFunc`.




In application, the `transformFunc` argument is analogous to the
`FUN` argument in R's apply,
lapply, etc. functions. Just as with the `*apply`
functions, the `transformFunc` function does not receive results from
previous chunks as input and so an external mechanism is required to carry
over results from one chunk to the next.
Additional information variables are available for use within a
transformation function when working in a local compute context:


* `.rxStartRow`: the row number from the original data set of the first row in the chunk of data that is being processed.


* `.rxChunkNum`: the chunk number of the data being  processed, set to 0 if a test sample of data is being processed. For example, if there are 10 blocks of data in an .xdf file, and `blocksPerRead` is set to 2, there will be 5 chunks of data.


* `.rxNumRows`: the number of rows in the current chunk.


* `.rxReadFileName`: character string containing the name of the .xdf file being processed.


* `.rxIsTestChunk`: logical set to `TRUE` if the chunk of data being processed is a test sample of data.


* `.rxIsPrediction`: logical set to `TRUE` if the chunk of data being processed being used in a prediction rather than a model estimation.


* `.rxTransformEnvir`: environment containing the data specified in `transformObjects`, which is also the parent environment to the transformation functions.




Three functions are also available for use within transformation functions that facilitate
the interaction with `transformObjects` objects packed into transformation function environment 
(`.rxTransformEnvir`):


* `.rxGet(objName)`: Get the value of an object in the transform environment,  e.g., `.rxGet("myObject")`.



* `.rxSet(objName, objValue)`: Set the value of an object in the transform environment,  e.g., `.rxSet("myObject", pi)`.



* `.rxModify(objName, ..., FUN = "+")`: Modify the value of an object in the transform environment by applying to the current value a specified function, `FUN`. The first argument passed (behind the scenes)  to `FUN` is always the current value of `"objName"`. The ellipsis argument ( ...) denotes additional  arguments passed to `FUN`. By default, `FUN = "+"`, the addition operator.  e.g.,   
   * Increment the current value of "myObject" by 2:`.rxModify("myObject", 2)`  
   * Decrement the current value of "myObject" by 4:`.rxModify("myObject", 4, FUN = "-")`  
   * Convert "myObject", assumed to be a vector, to a factor with  specified levels:`.rxModify("myObject", levels = 1:5, exclude = 6:10, FUN = "factor")`  
 











 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxFormula](../../r-reference/revoscaler/rxformula.md),
[rxTransform](rxTransform.md),
[rxCrossTabs](../../r-reference/revoscaler/rxcrosstabs.md),
[rxCube](../../r-reference/revoscaler/rxcube.md),
[rxLinMod](../../r-reference/revoscaler/rxlinmod.md),
[rxLogit](rxLogit.md),
[rxSummary](rxSummary.md),
[rxDataStep](../../r-reference/revoscaler/rxdatastep.md),
[rxImport](../../r-reference/revoscaler/rximport.md),
[rxTextToXdf](rxTextToXdf.md).
   
 ##Examples

 ```
   
  # Develop a data.frame to serve as a local data source
  set.seed(100)
  N <- 100
  feet <- round(runif(N, 4, 6))
  inches <- floor(runif(N, 1, 12))
  sportLevels <- c("tennis", "golf", "cycling", "running")
  myDF <- data.frame(sex = sample(c("Male", "Female"), N, replace = TRUE),
                     age = round(runif(N, 15, 40)),
                     height = paste(feet, "ft", inches, "in", sep = ""),
                     pounds = rnorm(N, 140, 20),
                     sport = factor(sample(sportLevels, N, replace = TRUE),
                                    levels = sportLevels))
  
  # Take a peek at the data source
  head(myDF)
  
  # Develop a transformation function
  xform <- function(dataList)
  {
      # Create a new continuous variable from an existing continuous variable: 
      # convert weight in units of pounds to kilograms.
      dataList$kilos <- dataList$pounds * 0.45359237
  
      # Remove an existing variable: 'pounds'
      dataList$pounds <- NULL
  
      # Create a new continuous variable from an existing factor variable:
      # convert 'height' categories (character strings) to a continuous 
      # variable named 'heightInches' containing the height in inches.
      heightStrings <- as.character(dataList$height)
      feet <- as.numeric(sub("ft.*", "", heightStrings))
      inches <- as.numeric(sub("in", "", sub(".*ft", "", heightStrings)))
      dataList$heightInches <- feet * 12 + inches
  
      # Alter an existing factor variable: for plotting purposes, 
      # coerce the 'sport' factor levels to be in alphabetical order.
      dataList$sport <- factor(dataList$sport,
                               levels = sort(levels(dataList$sport)))
  
      # Return the adapted variable list
  	dataList
  }
  
  # Perform a cube. Note the use of the 'kilos' variable in the formula, which
  # is created in the variable transformation function.
  cubeWeight <- rxCube(kilos ~ sport : sex, data = myDF, transformFunc = xform,
                       transformVars = c("pounds", "height", "sport"))
  
  # Analyze the cube results
  cubeWeight
  if ("lattice" %in% .packages())
  {
  	barchart(kilos ~ sport | sex, data = cubeWeight, xlab = "Sport",
           ylab = "Mean Weight (kg)")
  }
  
  # Perform a cube with multiple dependent variables using a formula string
  cubeHW <- rxCube(cbind(kilos, heightInches) ~ sport : sex, data = myDF,
                   transformFunc = xform,
                   transformVars = c("pounds", "height", "sport"))
  cubeHW
  if ("lattice" %in% .packages())
  {
  	barchart(heightInches + kilos ~ sex | sport, data = cubeHW, xlab = "Sex",
           ylab = "Mean Height (in) | Mean Weight (kg)")
  }
           
  # Use a transformation function to match external group data to
  # individual observations
           
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
  
  # Create a function that creates a transformation function
  makeTransformFunc <- function()
  {
  	# Put average per capita educational expenditures into a named vector
  	educExp <- c(Connecticut=1795.57, Washington=1170.46, Indiana = 1289.66)
  			
   	function( dataList )
  	{
  		dataList$stateEducExpPC <- educExp[match(dataList$state, names(educExp))]
  		return( dataList)
  	}
  }
  
  linModObj <- rxLinMod(incwage~sex + age + stateEducExpPC, data=censusWorkers, 
  	transformFun=makeTransformFunc(), transformVars="state")
  summary(linModObj)         
  
  
  ###
  # Illustrate the use of .rxGet, .rxSet and .rxModify
  ###
  
  # This file has five blocks and so our data step will loop through the
  # transformation function (defined below) six times: once as a test
  # check performed by rxDataStep and once per block in the file.
  # The test step is skipped if returnTransformObjects is TRUE
  
  inFile <- file.path(rxGetOption("unitTestDataDir"), "test100r5b.xdf") 
  
  # Basic example
  "myTestFun1" <- function(dataList) 
  { 
      # If returnTransformObjects is TRUE, we won't get a test chunk (0)
      print(paste("Processing chunk", .rxChunkNum))
      
      numRows <- length(dataList[[1]])
      # Increment numRows by the number of rows in this chunk
      .rxSet("numRows", .rxGet("numRows") + numRows)
      
      # Increment numChunks by 1
      .rxModify("numChunks", 1L, FUN = "+")
      
      # Multiply current count3 by twice its value
      sumxnum1 <- sum(dataList$xnum1)
      print(paste("Sum of chunk xnum1:", sumxnum1))
      .rxModify("sumxnum1", sumxnum1, FUN = "+")
      
      # Don't return any data, since were just want transformObjects
      return( NULL )  
  }
  
  newValues <- rxDataStep( inData = inFile, transformFunc = myTestFun1,
      transformObjects = list(numRows = 0L, numChunks = 0L, sumxnum1 = 0L),
      returnTransformObjects = TRUE)
  newValues
  
  # More complicated example using transformEnv
  "myFunction" <- function() 
  { 
      count1 <- 100L
      
      "myTestFun" <- function(dataList) 
      { 
          if (!.rxIsTestChunk)
          {
              # Ladder update: this only updates local count1: better to use .rxSet or .rxModify
              # You should get a warning about two variables with the name 'count1'
              # Chunk 0: 100
              # Chunk 1: 101
              # Chunk 2: 102
              # Chunk 3: 103
              # Chunk 4: 104
              # Chunk 5: 105         
              count1 <<- count1 + 1L
              print(count1)
              
              # Targeted update: increment count2
              .rxSet("count2", .rxGet("count2") + 1L)
              
              # Targeted update with .rxModify: increment count3
              .rxModify("count3", 1L, FUN = "+")
              
              # Targeted update with .rxModify: multiply current count4 by twice its value
              .rxModify("count4", 2L, FUN = "*")
              
              # Targeted update with .rxModify: permute second term of series, add nothing
              # Chunk 0: 1 2 3 4 5
              # Chunk 1: 1 3 4 5 2
              # Chunk 2: 1 4 5 2 3
              # Chunk 3: 1 5 2 3 4
              # Chunk 4: 1 2 3 4 5
              # Chunk 5: 1 3 4 5 2
              .rxModify("series", n = 2L, bias = 0L, FUN = "permute")
          }
  
          return(dataList) 
      }
  
      myEnv <- new.env(hash = TRUE, parent = baseenv())
      assign("permute", function(x, n = 1L, bias = 10) c(x[-n], x[n]) + bias, envir = myEnv)
      rxDataStep( inData = inFile, transformFunc = myTestFun, 
                  transformObjects = list(count1 = 0L, count2 = 0L, count3 = 0L, count4 = 1L, series = 1:5),
                  transformEnv = myEnv)
                  
      myEnv
  }
  
  # Obtain the transform environment containing the results
  transformEnvir <- myFunction()
  
  # Check results
  all.equal(transformEnvir[["count1"]], 0L)
  all.equal(transformEnvir[["count2"]], 5L)
  all.equal(transformEnvir[["count3"]], 5L)
  all.equal(transformEnvir[["count4"]], 32L)
  all.equal(transformEnvir[["series"]], c(1L, 3L, 4L, 5L, 2L))
  
  #############################################################################################
  # Exclude computations for dependent variable when using rxPredict
  
  myTransform <- function(dataList)
  {
      if (!.rxIsPrediction)
      {
          dataList$SepalLengthLog <- log(dataList$Sepal.Length)
      }
      dataList$SepalWidthLog <- log(dataList$Sepal.Width)
      return( dataList )
  }
  linModOut <- rxLinMod(SepalLengthLog~SepalWidthLog, data = iris,
     transformFunc = myTransform, transformVars = c("Sepal.Length", "Sepal.Width"))
     
  # Copy the iris data and remove Sepal.Length - used for the dependent variable
  iris1 <- iris
  iris1$Sepal.Length <- NULL
  # Run a prediction using the smaller data set
  predOut <- rxPredict(linModOut, data = iris1)
  
 
```
 
 
