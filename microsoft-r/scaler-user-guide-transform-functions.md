---

# required metadata
title: "Data transformations in Microsoft R"
description: "RevoScaleR transform functions."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "06/04/2017"
ms.topic: "article"
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
# Data transformations using RevoScaleR functions (Microsoft R)

Loading an initial dataset is typically just the first step in a continuum of data manipulation tasks that persist until your project is completed. Common examples of data manipulation include isolating a subset, slicing a dataset by variables or rows,and  converting or merging variables into new forms, often resulting in new data structures along the way.

There are two main approaches to data transformation using the RevoScaleR library:

+ Define an external based R *function* and reference it. 
+ Define an embedded transformation as an input to a *transforms* argument on another RevoScaleR function. 

Embedded transformations are supported in **rxImport**, **rxDataStep**, and in analysis functions like **rxLinMod** and **rxCube**, to name a few. Embedded transformations are easier to work with, but external functions allow for a greater degree of complexity and reuse. The following section provides an illustration of both techniques.

## Compare approaches

Embedded transformations provide instructions within a formula, through arguments on a function. Using just arguments, you can manipulate data using *transformations*, or select a rowset with the *rowSelection* argument.

Externally defined functions provide data manipulation instructions in an outer *function*, which is then referenced by a RevoScaleR function. An external transformation function is one that takes as input a list of variables and returns a list of (possibly different) variables. Due to the external definition, the object can assume a complex structure and be repurposed across multiple functions supporting transformation.

	# Load data
	> censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")

	# Option 1: Construct a new variable using an embedded transformation argument
	> NewDS <- rxDataStep (inData = censusWorkers, outFile = "c:/temp/newCensusWorkers.xdf",
			transforms = list(ageFactor = cut(age, breaks=seq(from = 20, to = 70, by = 5), 
			right = FALSE)), overwrite=TRUE)


	# Return variable metadata; ageFactor is a new variable
	> rxGetVarInfo(NewDS)

	# Option 2: Construct a new variable using an external function and rxDataStep
	> ageTransform <- function(dataList)
			{
				dataList$ageFactor <- cut(dataList$age, breaks=seq(from = 20, to = 70, 
										by = 5), right = FALSE)
				return(dataList)
			}

	> NewDS <- rxDataStep(inData = censusWorkers, outFile = "c:/temp/newCensusWorkers.xdf",
	    transformFunc = ageTransform, transformVars=c("age"), overwrite=TRUE)

	# Return variable metadata; it is identical to that of option 1
	> rxGetVarInfo(NewDS)

For more examples of both approaches, see [How to transform and subset data](scaler-user-guide-transform-functions.md).

## Arguments used in transformations

To specify an external function, use the *transformFunc* argument to most RevoScaleR functions. To pass a list of variables to the function, use the *transformVars* argument.

The following table numerates the arguments used in data manipulation tasks.

| Argument | Usage |
|----------|-------|
| *transforms* |  A formula for manipulating or transforming data, passed as an argument to a RevoScaleR function like **rxImport** or **rxDataStep**. This expression can be defined outside of the function call using the expression function. |
| *rowSelection* | Criteria for selecting a subset of rows in the current data frame. |
| *function* | A base R class used to create functions in R script.| 
| *transformFunc* | Assigns a function to a RevoScaleR function supporting data manipulation and transformations. If this argument is specified, it is evaluated before any other transformations specified in the *transforms*, *rowSelection*, or *formula* arguments. The list of variables to be passed to the transform function is specified as the *transformVars* argument.|
| *transformVars* | A list of variables to pass to your external transform function. |
| *transformObjects* | A named list of  objects that can be referenced by *transforms*, *transformFunc*, or *rowSelection* arguments. |
| *transformPackages* | A list of packages, in addition to those specified in **rxGetOption("transformPackages")**, to be preloaded, that provide libraries and functions used in *transforms*, *transformFunc*, or *rowSelection* arguments. | 
| *transformEnvir* | A user-defined environment to serve as a parent to all environments developed internally and used for variable data transformation.| 


## Functions supporting transformations

The following functions support embedded transformations or reference an external transformation function.

| Function | Use case|
|----------|-----------|
| **rxDataStep** | Create a subset rows or variables, or create new variables by transforming existing variables. Also used for easy conversion between data in data frames and .xdf files. |
| **rxImport** | Invoke a transformation while loading data into a data frame or .xdf file. |
| **rxSummary** | Transform data inline, while computing summary statistics. |
| **rxLinMod** | Create or transform variables used in the data set of a linear regression.|
| **rxLogit** | Create or transform variables used in the data set of a logistic regression. |
| **rxCube** | Create new variables or transform an existing variable used to create the list of variables in **rxCube** output.|

Other functions don't accept transformation logic, but are used for data manipulation operations:

+ **rxSetVarInfo** Change variable information, such as the name or description, in an .xdf file or data frame. 
+ **rxSetInfo**  Add or change a data set description.
+ **rxFactors** Add or change a factor levels
+ **rxMerge** Combines two or more datasets.
+ **rxSort** Orders a dataset based on some criteria

## How transforms are evaluated

User-defined transforms and transform functions are evaluated in essentially the same way. User-defined transforms are combined into an implicit transform function for evaluation purposes. At evaluation time, the process is the same whether the function is predefined or user-defined.

An evaluation environment is constructed from the base environment together with the utils, stats, methods packages, any packages specified with the *transformPackages* argument, and any specified *transformObjects*, and the closure of the transform function. 

Functions are then evaluated in the context of this environment. Function that are in packages but not part of the evaluation environment can be used if fully qualified (specifically, prefixed by the package name and two or three colons, depending on whether the function is exported).

If you are using methods packages, you can modify the basic list by setting the **rxOption** *transformPackages* argument.

## Transformations to avoid

Transform functions are very powerful, but there are four types of transformation that should be avoided:

1. Transformations that change the length of a variable. This includes, naturally, most model-fitting functions.
2. Transformations that depend upon all observations simultaneously. Because RevoScaleR works on chunks of data, such transformations will not have access to all the data at once. Examples of such transformations are matrix operations such as poly or solve.
3. Transformations that have the possibility of creating different mappings of factor codes to factor labels.
4. Transformations that involve sampling with replacement. Again, this is because RevoScaleR works on chunks of data, and sampling with replacement chunk by chunk is not equivalent to sampling with replacement from the full data set.

If you change the length of one variable, you will get errors from subsequent analysis functions that not all variables are the same length. If you try to change the length of all variables (essentially, performing some sort of row selection), you need to pass all of your data through the transform function, and this can be very inefficient. To avoid this problem, use row selection to work on data one slice at a time..

If you create a factor within a transformation function, you may get unexpected results because all of the data is not in memory at one time. When creating a factor within a transformation function, you should always explicitly set the values and labels. For example:

	dataList$xfac <- as.factor(dataList$x, levels = c(1, 2,3), 
		labels = c("One", "Two", "Three")) 


## Next Steps

Continue on to the following data-related articles to learn more about XDF, data source objects, and other data formats:

+ [How to transform and subset data](scaler-user-guide-data-transform.md)	
+ [XDF files](scaler-data-xdf.md)	
+ [Data Sources](scaler-user-guide-data-source.md)	
+ [Import text data](scaler-user-guide-data-import.md)
+ [Import ODBC data](scaler-data-odbc.md)
+ [Import and consume data on HDFS](scaler-data-hdfs.md)

## See Also
   
 [RevoScaleR Functions](r-reference/revoscaler/revoscaler.md)   
 [Tutorial: data import and exploration](scaler-getting-started-data-import-exploration.md)
 [Tutorial: data visualization and analysis](scaler-getting-started-data-visualization-analysis.md) 