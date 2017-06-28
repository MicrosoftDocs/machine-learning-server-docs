--- 
 
# required metadata 
title: " Generator for an PemaByGroup reference class object " 
description: " Generator for an PemaByGroup reference class object to be used with pemaCompute. This will compute arbitrary statistics by a grouping variable in a data set. " 
keywords: "RevoPemaR, PemaByGroup,  models " 
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
 
 
 #`PemaByGroup`:  Generator for an PemaByGroup reference class object 

 Applies to version 8.0.3 of package RevoPemaR.
 
 ##Description
 
Generator for an PemaByGroup reference class object to be used with pemaCompute. This
will compute arbitrary statistics by a grouping variable in a data set.
 
 
 ##Usage

```   
  PemaByGroup(...)
 
```
 
 
 ##Arguments

   
    
 ### ` ...`
  Arguments used in the constructor. See Details section. 
  
 
 
 ##Details
 
This is an example of a parallel external memory algorithms for use with
[pemaCompute](pemacompute.md). 
 User-specified arguments for the `initalize` method are:

groupByVar = "character", 
computeVars = "character",
fnList = "ANY",
sep = "character",  

* 
 `groupByVar`: character string containing name of grouping variable.  It can be a factor, character,
or integer vector.

* 
 `computeVars`: character vector containing the name of one or more variables on which to compute
the by-group statistics.

* 
 `fnList`: a named list of lists specifying functions and argument values.  
The first element of each function list should be the function. The second element of the function list should be named
 the name of the argument for the data vector or list of data vectors to process.  
 The value should be set to `NULL` or the names of additional variables to be included in an input data list.

* 
 `sep`: the separator to use in creating the variable name(s) for the computed statistic(s).  It will
 take the form of `varName.fnName` where the `.` is replaced by `sep`.


 
 
 ##Value
 
An `PemaByGroup` reference class object.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 
 ##See Also
 
setRefClass,
[PemaBaseClass](pemabaseclass.md)
   
 ##Examples

 ```
   
  
  # Show help for methods
  PemaByGroup$help(initialize)
  PemaByGroup$help(processData)
  PemaByGroup$help(updateResults)
  PemaByGroup$help(processResults)
  
 
```
 
 
 
 
