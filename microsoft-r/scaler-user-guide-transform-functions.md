---

# required metadata
title: "Transform functions in Microsoft R"
description: "RevoScaleR transform functions."
keywords: ""
author: "richcalaway"
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

+ Define and reference an external function that performs a transformation. 
+ Define transformation statements inline using a function's *transforms* argument. Embedded transformations are supported in **rxImport**, **rxDataStep**, and in analysis functions like **rxLinMod** and **rxCube**, to name a few.

Embedded transformations are easier, but external functions allow for a greater degree of complexity and reuse. The following section uses an example to illustrate both approaches.

## Compare approaches

Embedded transformations provide instructions within a formula, through arguments on a function. Using just arguments, you can manipulate data using *transformations*, or select a rowset with the *rowSelection* argument.

Externally defined functions provide data manipulation instructions in an outer *function*, which is then referenced by a RevoScaleR function. Due to the external definition, the object can assume a complex structure. 



An external transformation function is one that takes as input a list of variables and returns a list of (possibly different) variables. 

To specify an external function, use the *transformFunc* argument to most RevoScaleR functions. 
To pass a list of variables to the function, use the *transformVars* argument.

If defined and specified (using the *transformFunc* argument to most RevoScaleR functions), a transform function is evaluated before any transformations specified in the transforms, *rowSelection*, or formula arguments. The list of variables to be passed to the transform function is specified as the *transformVars* argument.

## Arguments used in transformations

The following table numerates the arguments used in data manipulation tasks.

| Argument | Usage |
|----------|-------|
| *transforms* |  A formula for manipulating or transforming data, passed as an argument to a RevoScaleR function like **rxImport** or **rxDataStep**. |
| *rowSelection* | Criteria for selecting a subset of rows in the current data frame. |
| *function* | A base R class used to create functions in R script.| 
| *transformFunc* | Assigns a function to a RevoScaleR function supporting data manipulation and transformations. |
| *transformVars* | A list of variables to pass to your external transform function. |

## Functions supporting transformations

The following functions can be used for data manipulation tasks.

| Function | Use case|
|----------|-----------|
| **rxDataStep** | Create a subset rows or variables, or create new variables by transforming existing variables. Also used for easy conversion between data in data frames and .xdf files. 
| **rxImport** | Invoke a transformation while loading data into a data frame or .xdf file.
| **rxSetVarInfo** | Change variable information, such as the name or description, in an .xdf file or data frame. |
| **rxSetInfo** | Add or change a data set description. |
| **rxCube** | |
| **rxLinMod** | |
| **rxLogit** | |

| **rxFactors** | Create or modify factors (categorical variables) based on existing variables. |
| **rxSort** | Sort a data set by one or more key variables. |
| **rxMerge** | Merge two data sets by one or more key variables. |

## How transforms are evaluated

User-defined transforms and transform functions are evaluated in essentially the same way. User-defined transforms are combined into an implicit transform function for evaluation purposes. At evaluation time, the process is the same whether the function is predefined or user-defined.

An evaluation environment is constructed from the base environment together with the utils, stats, methods packages, any packages specified with the *transformPackages* argument, and any specified *transformObjects*, and the closure of the transform function. 

Functions are then evaluated in the context of this envrionment. Function that are in packages but not part of the evaluation environment can be used if fully qualified (specifically, prefixed by the package name and two or three colons, depending on whether the function is exported).

If you are using methods packages, you can modify the basic list by setting the **rxOption** *transformPackages* argument.

## Transformations to avoid

Transform functions are very powerful, and we recommend their use. However, there are four types of transformation that should be avoided:

1. Transformations that change the length of a variable. This includes, naturally, most model-fitting functions.
2. Transformations that depend upon all observations simultaneously. Because RevoScaleR works on chunks of data, such transformations will not have access to all the data at once. Examples of such transformations are matrix operations such as poly or solve.
3. Transformations that have the possibility of creating different mappings of factor codes to factor labels.
4. Transformations that involve sampling with replacement. Again, this is because RevoScaleR works on chunks of data, and sampling with replacement chunk by chunk is not equivalent to sampling with replacement from the full data set.

If you change the length of one variable, you will get errors from subsequent analysis functions that not all variables are the same length. If you try to change the length of all variables (essentially, performing some sort of row selection), you need to pass all of your data through the transform function, and this can be very inefficient. We therefore recommend the row selection procedures described in previous chapters.

If you create a factor within a transformation function, you may get unexpected results because all of the data is not in memory at one time. It is recommended that if creating a factor within a transformation function, you always explicitly set the values and labels. For example,

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
   
 [RevoScaleR Functions](scaler/scaler.md)   
 [Tutorial: data import and exploration](scaler-getting-started-data-import-exploration.md)
 [Tutorial: data visualization and analysis](scaler-getting-started-data-visualization-analysis.md) 