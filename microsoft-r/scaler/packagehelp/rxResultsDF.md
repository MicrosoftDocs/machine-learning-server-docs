--- 
 
# required metadata 
title: "Crosstab Counts or Sums Data Frame" 
description: " Obtain a counts, sums, or means data frame from an analysis object. " 
keywords: "RevoScaleR, rxResultsDF, rxResultsDF.rxCrossTabs, rxResultsDF.rxCube, rxResultsDF.rxLinMod, rxResultsDF.rxLogit, rxResultsDF.rxSummary, category, models" 
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
 
 
 
 
 
 
 
 #`rxResultsDF`: Crosstab Counts or Sums Data Frame

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Obtain a counts, sums, or means data frame from an analysis object.
 
 
 ##Usage

```   
 ## S3 method for class `rxCrossTabs':
rxResultsDF  (object, output = "counts", element = 1,
     integerLevels = NULL, integerCounts = TRUE, ...)
 ## S3 method for class `rxCube':
rxResultsDF  (object, output = "counts",
     integerLevels = NULL, integerCounts = TRUE, ...)
 ## S3 method for class `rxLinMod':
rxResultsDF  (object, output = "counts", 
     integerLevels = NULL, integerCounts = TRUE, ...)
 ## S3 method for class `rxLogit':
rxResultsDF  (object, output = "counts", 
     integerLevels = NULL, integerCounts = TRUE, ...)
 ## S3 method for class `rxSummary':
rxResultsDF  (object, output = "stats", element = 1,
     integerLevels = NULL, integerCounts = TRUE, useRowNames = TRUE, ...)            
 
```
 
 ##Arguments

   
    
 ### `object`
 object of class rxCrossTabs, rxCube, rxLinMod, rxLogit, or rxSummary. 
  
  
    
 ### `output`
 character string specifying the type of output to display.  Typically this is `"counts"`, or for `rxSummary` `"stats"`. If there is a  dependent variable in the `rxCrossTabs` formula, `"sums"` and `"means"`can be used.  
  
  
    
 ### `element`
 integer specifying the element number from the object list to extract. Currently only `1` is supported. 
  
  
    
 ### `integerLevels`
 logical scalar or `NULL`. If `TRUE`, the  first column of the returned data frame will be converted to integers.  If `FALSE`, it will be a factor column. If `NULL`, it will be returned as an integer column if it was wrapped in an `F()` in the formula. 
  
  
    
 ### `integerCounts`
 logical scalar. If `TRUE`, the  counts or sums in the returned data frame will be converted to integers.  If `FALSE`, they will be numeric doubles. 
  
  
    
 ### `useRowNames`
 logical scalar. If `TRUE`, the names of the variables for  which there are results will be put into the row names for the data frame   rather than in a separate `Names` variable. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the underlying print method for the output list object. 
  
  
 
 
 ##Details
 
Method to access counts, sums, and means data frames from
RevoScaleR analysis objects.
 
 
 ##Value
 
a data frame containing the computed counts, sums, or means.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxCrossTabs](../../r-reference/revoscaler/rxcrosstabs.md)
[rxCube](../../r-reference/revoscaler/rxcube.md)
[rxLinMod](rxLinMod.md)
[rxLogit](rxLogit.md)
[rxSummary](rxSummary.md)
[rxMarginals](rxMarginals.md)
   
 ##Examples

 ```
   
  admissions <- as.data.frame(UCBAdmissions)
  myCrossTab1 <- rxCrossTabs(~Gender : Admit, data = admissions)
  countsDF1 <- rxResultsDF(myCrossTab1)
  
  # If a dependent variable is used, "sums" and "means" can be used
  myCrossTab2 <- rxCrossTabs(Freq ~ Gender : Admit, data = admissions)
  countsDF <- rxResultsDF(myCrossTab2)
  sumsDF <- rxResultsDF(myCrossTab2, output = "sums")
  meansDF <- rxResultsDF(myCrossTab2, output = "means")
 
```
 
 
 
