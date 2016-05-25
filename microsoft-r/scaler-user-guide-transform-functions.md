---

# required metadata
title: "RevoScaleR User's Guide--Transform Functions"
description: "RevoScaleR transform functions."
keywords: ""
author: "richcalaway"
manager: "mblythe"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
ms.prod: "rserver"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# Transform Functions

In previous chapters, we have used the *transforms* argument and inline transformations within formulas whenever we needed to specify a data transformation, and performed row selection with the *rowSelection* argument. In this chapter, we introduce another method for performing transformations within RevoScaleR that is extremely powerful and can be used both for transforming variables and for creating row selection variables. This method is the use of transform functions.

A transform function is a function that takes as input a list of variables and returns a list of (possibly different) variables. If defined and specified (using the *transformFunc* argument to most RevoScaleR functions), a transform function is evaluated before any transformations specified in the transforms, *rowSelection*, or formula arguments. The list of variables to be passed to the transform function is specified as the *transformVars* argument.

### Creating Variables

Suppose we want to extract five variables from the *CensusWorkers* data set, but also add a factor variable based on the integer variable age. We considered this example in Chapter 4, when we used the *transforms* argument to create the new variable. We can also use a transform function to create new variables. For example, to create our factor variable, we can create the following function:

	######################################################## 
	# Chapter 18: Transform Functions
	#  Creating Variables
	Ch18Start <- Sys.time()
	
	  
	ageTransform <- function(dataList)
	{
	    dataList$ageFactor <- cut(dataList$age, breaks=seq(from = 20, to = 70, 
	                              by = 5), right = FALSE)
	    return(dataList)
	}
	

We can test the function by reading an arbitrary chunk out of the data set. For efficiency reasons, the data passed to the transformation function is stored as a list rather than a data frame, so when reading from the .xdf file we set the *returnDataFrame* argument to FALSE to emulate this behavior. Since we only use the variable *age* in our transformation function, we restrict the variables extracted to that.

	censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")	
	testData <- rxReadXdf(file = censusWorkers, startRow = 100, numRows = 10, 
	returnDataFrame = FALSE, varsToKeep = c("age"))
	
	as.data.frame(ageTransform(testData))


The resulting list of data (displayed as a data frame) shows us that our transformations are working as expected:

	> as.data.frame(ageTransform(testData))
	   age ageFactor
	1   20   [20,25)
	2   48   [45,50)
	3   44   [40,45)
	4   29   [25,30)
	5   28   [25,30)
	6   43   [40,45)
	7   20   [20,25)
	8   23   [20,25)
	9   32   [30,35)
	10  42   [40,45)

In doing a data step operation, RevoScaleR reads in a chunk of data read from the original data set, including only the variables indicated in *varsToKeep*, or omitting variables specified in *varsToDrop*. It then passes the variables needed for data transformations back to R for manipulation. We specify the variables needed to process the transformation in the *transformVars* argument. Including extra variables does not alter the analysis, but it does reduce the efficiency of the data step. In this case, since *ageFactor* depends only on the *age* variable for its creation, the *transformVars* argument needs to specify just that:

	rxDataStep(inData = censusWorkers, outFile = "newCensusWorkers",
	    varsToDrop = c("state"), transformFunc = ageTransform,
	    transformVars=c("age"), overwrite=TRUE)

The *rxGetInfo* function reveals the added and dropped variables:

	rxGetInfo("newCensusWorkers", getVarInfo = TRUE)

	  File name: C:\YourOutputPath\newCensusWorkers.xdf 
	  Number of rows: 351121 
	  Number of variables: 6 
	  Number of blocks: 6 
	  Compression type: zlib
	  Variable information: 
	  Var 1: age, Age 
			 Type: integer, Low/High: (20, 65)
	  Var 2: incwage, Wage and salary income 
			 Type: integer, Low/High: (0, 354000)
	  Var 3: perwt, Type: integer, Low/High: (2, 168)
	  Var 4: sex, Sex 
			 2 factor levels: Male Female
	  Var 5: wkswork1, Weeks worked last year 
			 Type: integer, Low/High: (21, 52)
	  Var 6: ageFactor
			 10 factor levels: [20,25) [25,30) [30,35) [35,40) [40,45) [45,50) 
	  [50,55) [55,60) [60,65) [65,70)
	  
### Using Additional Objects or Data in a Transform Function

It is sometimes useful to access additional information from within a transform function. For example, you might want to match additional data in the process of creating new variables. Transform functions are evaluated in a “sterilized” environment which includes the parent environment of the function closure. To provide access to additional data within the function, you can use the *transformObjects* argument.

For example, suppose you would like to estimate a linear model using wage income as the dependent variable, and want to include state-level of per capita expenditure on education as one of the independent variables. We can define a named vector to contain this state-level data as follows:

	#  Using Additional Objects or Data in a Transform Function
	  
	educExpense <- c(Connecticut=1795.57, Washington=1170.46, Indiana = 1289.66)

We can then define a transform function that uses this information as follows:

	transformFunc <- function(dataList)
	{
	      # Match each individual’s state and add the variable for educ. exp.
		dataList$stateEducExpPC = educExp[match(dataList$state, names(educExp))]
		return(dataList)
	}

We can then use the transform function and our named vector in a call to *rxLinMod* as follows:

	censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	linModObj <- rxLinMod(incwage~sex + age + stateEducExpPC, 
		data = censusWorkers, pweights = "perwt",
		transformFun = transformFunc, transformVars = "state", 
		transformObjects = list(educExp = educExpense))
	summary(linModObj)

When the transform function is evaluated, it will have access to the *educExp* object. The final results show:

	Call:
	rxLinMod(formula = incwage ~ sex + age + stateEducExpPC, data = censusWorkers, 
	    pweights = "perwt", transformObjects = list(educExp = educExp), 
	    transformFunc = transformFunc, transformVars = "state")
	
	Linear Regression Results for: incwage ~ sex + age + stateEducExpPC
	File name:
	    C:\Program Files\Microsoft\MRO-for-RRE\8.0\R-3.2.2\library\RevoScaleR\SampleData\CensusWorkers.xdf
	Probability weights: perwt
	Dependent variable(s): incwage
	Total independent variables: 5 (Including number dropped: 1)
	Number of valid observations: 351121
	Number of missing observations: 0 
	 
	Coefficients: (1 not defined because of singularities)
	                 Estimate Std. Error t value Pr(>|t|)    
	(Intercept)    -1.809e+04  4.414e+02  -40.99 2.22e-16 ***
	sex=Male        1.689e+04  1.321e+02  127.83 2.22e-16 ***
	sex=Female        Dropped    Dropped Dropped  Dropped    
	age             5.593e+02  5.791e+00   96.58 2.22e-16 ***
	stateEducExpPC  1.643e+01  2.731e-01   60.16 2.22e-16 ***
	---
	Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
	
	Residual standard error: 175900 on 351117 degrees of freedom
	Multiple R-squared: 0.07755 
	Adjusted R-squared: 0.07754 
	F-statistic:  9839 on 3 and 351117 DF,  p-value: < 2.2e-16 
	Condition number: 1.1176

### Creating a Row Selection Variable

One common use of the *transformFunc* argument is to create a logical variable to use as a row selection variable. For example, suppose you want to create a random sample from a massive data set. You can use the *transformFunc* argument to specify a transformation that creates a random binomial variable, which can then be coerced to logical and used for row selection. The object name *.rxRowSelection* is reserved for the row selection variable; if RevoScaleR finds this object, it is used for row selection.

[!Warning] If you both specify a *rowSelection* argument and define a *.rxRowSelection* variable in your transform function, the one specified in your transform function will be overwritten by the contents of the *rowSelection* argument, so that expression takes precedence.

The following code creates a random selection variable to create a data frame with a random 10% subset of the census workers file:

	#  Creating a Row Selection Variable
	  
	createRandomSample <- function(data)
	{
	    data$.rxRowSelection <- as.logical(rbinom(length(data[[1]]), 1, .10))
		return(data)
	}
	censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	df <- rxXdfToDataFrame(file = censusWorkers, transformFunc = createRandomSample, 
		transformVars = "age")

The resulting data frame, *df*, has approximately 35,000 rows. You can look at the first few rows using *head* as follows:

	rxGetInfo(df)
	head(df)

		age incwage perwt    sex wkswork1   state
	  1  50    9000    30   Male       48 Indiana
	  2  41   35000    20 Female       48 Indiana
	  3  55   40400    21   Male       52 Indiana
	  4  56   45000    30 Female       52 Indiana
	  5  46   17200    60 Female       52 Indiana
	  6  49   35000    21 Female       52 Indiana
	  
Equivalently, we could create the temporary row selection variable using the *rowSelection* argument with the internal *.rxNumRows* variable, which provides the number of rows in the current chunk of data being processed:

	df <- rxDataStep(inData = censusWorkers, 
		rowSelection = as.logical(rbinom(.rxNumRows, 1, .10)) == TRUE)

### Using Internal Variables in a Transformation Function: An Example Computing Moving Averages

Four additional variables providing information on the RevoScaleR processing are available for use in your transform functions:

-   .rxStartRow: The row number from the original data that was read as the first row of the current chunk.
-   .rxChunkNum: The current chunk being processed.
-   .rxReadFileName: The name of the .xdf file currently being read.
-   .rxIsTestChunk: Whether the chunk being processed is being processed as a test sample of data.

These are particularly useful if you need to access additional rows of data when processing a chunk. This is the case, for example, when you want to include lagged data in a calculation. The following example is a transformation function “generator” to compute a simple moving average. By creating this “function within a function” we are able to easily pass arguments into a transformation function. This one takes as arguments the number of days (or time units) for the moving average, the name of the variable that will be used to compute the moving average, and the name of the new variable to create. The transformation function computes the number of lagged observations to read in, then reads in that data to create a long data vector with the lags. The simple moving average calculations are performed and put into a new variable. The additional lagged rows are removed from the original data vector before returning from the function.

	#  An Example Computing Moving Averages
	  
	makeMoveAveTransFunc <- function(numDays = 10, varName="", newVarName="")
	{
		function (dataList)
		{
			numRowsToRead <- 0
			varForMoveAve <- 0
			# If no variable is named, use the first one sent 
			# to the transformFunc
			if (is.null(varName) || nchar(varName) == 0)
			{
				varForMoveAve <- names(dataList)[1]
			} else {
				varForMoveAve <- varName
			}   
		 
			# Get the number of rows in the current chunk
			numRowsInChunk <- length(dataList[[varForMoveAve]])
		
			# .rxStartRow is the starting row number of the
			# chunk of data currently being processed
		  # Read in previous data if we are not starting at row 1
		 if (.rxStartRow > 1)
		 {
			 # Compute the number of lagged rows we'd like
				numRowsToRead <- numDays - 1
			 # Check to see if enough data is available
			 if (numRowsToRead >= .rxStartRow)
			 {
				numRowsToRead <- .rxStartRow - 1
			 }
				# Compute the starting row of previous data to read
			 startRow <- .rxStartRow - numRowsToRead
			# Read previous rows from the .xdf file
			 previousRowsDataList <- RevoScaleR::rxReadXdf(
					file=.rxReadFileName, 
			   varsToKeep=names(dataList), 
					startRow=startRow, numRows=numRowsToRead,
			   returnDataFrame=FALSE)
				# Concatenate the previous rows with the existing rows 
			   dataList[[varForMoveAve]] <- 
					c(previousRowsDataList[[varForMoveAve]], 
						dataList[[varForMoveAve]])
		 }
		 # Create variable for simple moving average
			# It will be added as the last variable in the data list
			newVarIdx <- length(dataList) + 1

			# Initialize with NA's
		  dataList[[newVarIdx]] <- rep(as.numeric(NA), times=numRowsInChunk)

		  for (i in (numRowsToRead+1):(numRowsInChunk + numRowsToRead))
		  {	
			  j <- i - numRowsToRead
			  lowIdx <- i - numDays + 1
			  if (lowIdx > 0 && ((lowIdx == 1) || 
					(j > 1 && (is.na(dataList[[newVarIdx]][j-1])))))
			{
					# If it's the first computation or the previous value 
					# is missing, take the mean of all the relevant lagged data
				 dataList[[newVarIdx]][j] <- 
						 mean(dataList[[varForMoveAve]][lowIdx:i])
			 } else if (lowIdx > 1)
			 {
					# Add and subtract from the last computation
				dataList[[newVarIdx]][j] <- dataList[[newVarIdx]][j-1] -
							dataList[[varForMoveAve]][lowIdx-1]/numDays +
					dataList[[varForMoveAve]][i]/numDays
			 }
			 }
			# Remove the extra rows we read in from the original variable
		   dataList[[varForMoveAve]] <- 
				
			   dataList[[varForMoveAve]][(numRowsToRead + 1):(numRowsToRead + 
				   numRowsInChunk)]
			
			# Name the new variable
			if (is.null(newVarName) || (nchar(newVarName) == 0))
			{
				# Use a default name if no name specified
			   names(dataList)[newVarIdx] <- 
					  paste(varForMoveAve, "SMA", numDays, sep=".")
			} else {
				names(dataList)[newVarIdx] <- newVarName
			}
		   return(dataList)
		}
	}

We could use this function with *rxDataStep* to add a variable to our data set, or on-the-fly, for example for plotting. Here we will use the sample data set containing daily information on the Dow Jones Industrial Average. We compute a 360-day moving average for the adjusted close (adjusted for dividends and stock splits) and plot it:

	DJIAdaily <- file.path(rxGetOption("sampleDataDir"), "DJIAdaily.xdf")
	rxLinePlot(Adj.Close+Adj.Close.SMA.360~YearFrac, data=DJIAdaily,  
	    transformFunc=makeMoveAveTransFunc(numDays=360),
	    transformVars=c("Adj.Close"))

![](media/rserver-scaler-user-guide-18-transform-functions/image25.png)

### How Transforms Are Evaluated

User-defined transforms and transform functions are evaluated in essentially the same way. An evaluation environment is constructed from the base environment together with the utils, stats, and methods packages (you can modify this basic list by setting the rxOption *transformPackages*), packages specified with the *transformPackages* argument, and any specified *transformObjects*, and the closure of the transform function. (User-defined transforms are combined into a single, “hidden” transform function for evaluation.) The transform function is then evaluated in this evaluation environment. Functions that are in packages that are not part of the evaluation environment can be used, but must be prefixed by their package name and two or three colons, depending on whether the function is exported.

### Transformations to Avoid

Transform functions are very powerful, and we recommend their use. However, there are four types of transformation that should be avoided:

1.  transformations that change the length of a variable (this includes, naturally, most model-fitting functions)
2.  transformations that depend upon all observations simultaneously (because RevoScaleR works on chunks of data, such transformations will not have access to all the data at once). Examples of such transformations are matrix operations such as poly or solve.
3.  transformations that have the possibility of creating different mappings of factor codes to factor labels.
4.  transformations that involve sampling with replacement. Again, this is because RevoScaleR works on chunks of data, and sampling with replacement chunk by chunk is not equivalent to sampling with replacement from the full data set.

If you change the length of one variable, you will get errors from subsequent analysis functions that not all variables are the same length. If you try to change the length of all variables (essentially, performing some sort of row selection), you need to pass all of your data through the transform function, and this can be very inefficient. We therefore recommend the row selection procedures described in previous chapters.

If you create a factor within a transformation function, you may get unexpected results because all of the data is not in memory at one time. It is recommended that if creating a factor within a transformation function, you always explicitly set the values and labels. For example,

	dataList$xfac <- as.factor(dataList$x, levels = c(1, 2,3), 
		labels = c("One", "Two", "Three")) 
