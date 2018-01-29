--- 
 
# required metadata 
title: "rxSetVarInfo function (revoAnalytics) | Microsoft Docs" 
description: " Set the variable information for an .xdf file, including variable names, descriptions, and value labels, or set attributes for variables in a data frame  " 
keywords: "(revoAnalytics), rxSetVarInfo, rxSetVarInfoXdf, attribute" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 
 #rxSetVarInfo: Set Variable Information for .xdf File or Data Frame 
 ##Description
 
Set the variable information for an .xdf file, including variable
names, descriptions, and value labels, or set attributes for variables
in a data frame 
 
 
 ##Usage

```   
  rxSetVarInfo(varInfo, data)
  rxSetVarInfoXdf(varInfo, file)
 
```
 
 ##Arguments

   
    
 ### `varInfo`
 list containing lists of variable information for variables in the XDF data source or data frame.  
  
    
 ### `data`
 a data frame, a character string specifying the .xdf file, or an [RxXdfData](RxXdfData.md) object.  
  
    
 ### `file`
 character string specifying the .xdf file or   an [RxXdfData](RxXdfData.md) object.  
  
 
 
 ##Details
 
The list for each variable contained in `varInfo` can have the following
elements:


* 
 `position`: integer indicating the position of the variable in
the data set. If present, will be used instead of the list name.

* 
 `newName`: character string giving a new name for the variable.

* 
 `description`: character string giving a description of the
variable.

* 
 `low`: numeric value specifying the low value used in
*on the fly* factor conversions using the `F()` transformation.
Ignored for data frames.

* 
 `high`: numeric value specifying the high value used in
*on the fly* factor conversions using the `F()` transformation.
Ignored for data frames.

* 
 `levels`: character vector of factor levels. This will re-label
an existing factor, but not change the underlying values.

* 
 `valueInfoCodes`: character vector of value codes for informational 
purposes only.

* 
 `valueInfoLabels`: character vector of value labels the same
length as valueInfoCodes, used for informational purposes only.

* 
 `tzone`: character string giving `tzone` attribute for a
POSIXct variable.  


 
 
 ##Value
 
If the input data is a data frame, a data frame is returned containing variables
with the new attributes.  If the input data represents an .xdf file or
composite file, an [RxXdfData](RxXdfData.md) object representing the modified 
file is returned.
 
 ##Note
 
`rxSetVarInfoXdf` and `rxSetVarInfo` do not change the underlying data 
so the user cannot set the variable type and storage type using this function, or recode the
levels of a factor variable.  To recode factors, use [rxFactors](rxFactors.md).
`rxSetVarInfo` is not supported for single .xdf files in HDFS.
 
 
 ##See Also
 
[rxDataStep](rxDataStep.md),
[rxFactors](rxFactors.md),
   
 ##Examples

 ```
   
  # Rename the factor levels for a variable
  
  
                   
  # Create a sample data set
  # We will change the names of the factor levels, after the fact
  set.seed(100)
  myData1 <- data.frame(y = rnorm(30),
                        month = factor(c(0:11, as.integer(runif(18, 0, 12)))))
                        
  # Plot the data
  rxLinePlot(y ~ month, type = "p", data = myData1)
  
  # Set the labels for the month variable
  monthLabels <- c("Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep",
                   "Oct", "Nov", "Dec")
                   
  # Get the variable information for the data frame
  varInfo <- rxGetVarInfo(myData1)
  
  # Reset the names of the factor levels
  varInfo$month$levels <- monthLabels
  myData2 <- rxSetVarInfo( varInfo, myData1)
  
  # Plot the new data
  rxLinePlot(y ~ month, type = "p", data = myData2)
  
  # Redo, this time putting the data in an .xdf file
  # Specify name of output file
  tempFile <- file.path(tempdir(), "rxTempTestFile.xdf")
  
  # Convert the original data to an XDF file
  rxDataStep(myData1, outFile = tempFile, overwrite = TRUE)
  
  # Get the varInfo of the new XDF file
  varInfo <- rxGetVarInfo(tempFile)
  
  varInfo$month$levels <- monthLabels
  
  # Write the modified varInfo back to the file
  rxSetVarInfo(varInfo, tempFile)
  
  # Read the data and plot it, showing the new factor levels
  myData2 <- rxDataStep(inData = tempFile)
  rxLinePlot(y ~ month, type = "p", data = myData2)
  
  ## Change variable names and add descriptions
  varInfo <- list(y = list(newName = "Rainfall",
                           description = "Rainfall per Month"),
                  list(position = 2, description = "Month of Year"))
  rxSetVarInfo(varInfo, tempFile)
  rxGetVarInfo(tempFile)
  file.remove(tempFile)
 
```
 
 
