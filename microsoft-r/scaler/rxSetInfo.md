---

# required metadata
title: "rxSetInfo ScaleR Function (RevoScaleR package)"
description: "rxSetInfo function in the RevoScaleR package in Microsoft R."
keywords: "RevoScaleR, ScaleR, rxSetInfo"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "08/13/2016"
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

#rxSetInfo function (RevoScaleR)
Set .xdf file or data frame information, such as a description.

## Usage
~~~~
rxSetInfo(data, description = "")
rxSetInfoXdf(file, description = "")
~~~~

## Argument

The following table shows the arguments to rxSetInfo in order and their default values.

|Parameter | Description|
| --------- | --------- |
|data |An .xdf file name, an [RxXdfData](RxXdfData.md) object, or a data frame. |
|file |An .xdf file name or an RxXdfData object. |
|description |Character string containing the file or data frame description. |

## Return Value
An [RxXdfData](RxXdfData.md) object representing the .xdf file, or a new data frame with the `.rxDescription` attribute set to the specified description.

## Examples
~~~~
# Create a sample data frame and .xdf file
outFile <- tempfile(pattern= ".rxTempFile", fileext = ".xdf")
if (file.exists(outFile)) file.remove(outFile)

origData <- data.frame(x=1:10)
rxDataStep(inData = origData, outFile = outFile, rowsPerRead = 5)

# Set a description in the data file
myDescription <- "Hello Data File!"
rxSetInfo( data = outFile, description = myDescription)
rxGetInfo(outFile)

# Read a data frame out of the output file    
myData <- rxReadXdf(outFile )
# Look at its attribute
attr(myData, ".rxDescription")    

file.remove(outFile)

myNewDescription = "Good-by data."
myData <- rxSetInfo(data=myData, description = myNewDescription)
rxGetInfo(myData)
~~~~

## See Also

[ScaleR Functions (RevoScaleR package)](scaler.md)

[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)
