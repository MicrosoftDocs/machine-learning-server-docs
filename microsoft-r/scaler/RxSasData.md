---

# required metadata
title: "RxSasData ScaleR Function (RevoScaleR package)"
description: "RxSasData ScaleR function in the RevoScaleR package in Microsoft R."
keywords: "RevoScaleR, ScaleR, RxSasData ScaleR"
author: "HeidiSteen"
manager: "paulettm"
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

#RxSasData ScaleR function (RevoScaleR)

Generate an RxSasData object that contains information about a SAS data set to be imported or analyzed. RxSasData is an S4 class, which extends **RxDataSource**.

## Usage
~~~~
RxSasData(file, stringsAsFactors = FALSE, colClasses = NULL, colInfo = NULL,
          rowsPerRead = 500000, formatFile = NULL, labelsAsLevels = TRUE,
          labelsAsInfo = TRUE, mapMissingCodes = "all",
          varsToKeep = NULL, varsToDrop = NULL,
          checkVarsToKeep = FALSE)

## S3 method for class 'RxSasData'
head(x, n = 6L, reportProgress = 0L, ...)
## S3 method for class 'RxSasData'
tail(x, n = 6L, addrownums = TRUE, reportProgress = 0L, ...)     
~~~~

## Arguments

The following table shows the arguments to RxSasData in order and their default values.

|Parameter | Description|
| --------- | --------- |
| file | Character string specifying a SAS data file of type ‘.sas7bdat’ (‘.sd7’).|
| formatFile|  Character string specifying a ‘.sas7cat’ file containing value labels for the variables stored in file.|
| stringsAsFactors | Logical indicating whether or not to automatically convert strings to factors on import. This can be overridden by specifying "character" in `colClasses` and `colInfo`. If `TRUE`, the factor levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified "levels".|
| colClasses | Character vector specifying the column types to use when converting the data. The element names for the vector are used to identify which column should be converted to which type. <br/> <br/> Allowable column types are: <br/> - "logical" (stored as uchar)<br/>  - "integer" (stored as int32)<br/> - "float32" (the default for floating point data for ‘.xdf’ files)<br/>  - "numeric" (stored as float64 as in R)<br/>  - "character" (stored as string)<br/>  - "factor" (stored as uint32)<br/> - "int16" (alternative to integer for smaller storage space)<br/>  - "uint16" (alternative to unsigned integer for smaller storage space)<br/>  - "Date" (stored as Date, i.e. float64) <br/> <br/> Note for "factor" type, the levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified "levels". Note that equivalent types share the same bullet in the list above; for some types we allow both ‘R-friendly’ type names, as well as our own, more specific type names for ‘.xdf’ data. <br/> <br/> Note also that specifying the column as a “factor” type is currently equivalent to “string” - for the moment, if you wish to import a column as factor data you must use the `colInfo` argument, documented below. <br/>
| colInfo | List of named variable information lists. Each variable information list contains one or more of the named elements given below. The information supplied for `colInfo` overrides that supplied for `colClasses`. <br/> <br/>  Currently available properties for a column information list are: <br/> - "type" (character string specifying the data type for the column. See `colClasses` argument description for the available types)<br/> - "newName" (character string specifying a new name for the variable)<br/> - "description" (character string specifying a description for the variable)<br/> - "levels" (character vector containing the levels when "type" = "factor". If the levels property is not provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value)<br/> - "newLevels" (new or replacement levels specified for a column of type “factor”. It must be used in conjunction with the levels argument. After reading in the original data, the labels for each level will be replaced with the `newLevels`)<br/> - "low" (the minimum data value in the variable (used in computations using the `F()` function)<br/> - "high" (the maximum data value in the variable (used in computations using the `F()` function)<br/>
| rowsPerRead | Number of rows to read at a time.|
| labelsAsLevels | Logical. If `TRUE`, variables containing value labels in the ‘SAS’ format file will be converted to factors, using the value labels as factor levels.|
| labelsAsInfo | Logical. If `TRUE`, variables containing value labels in the ‘SAS’ format file that are not converted to factors will retain the information as `valueInfoCodes` and `valueInfoLabels` in the ‘.xdf’ file. This information can be obtained using rxGetVarInfo. This information will also be returned as attributes for the columns in a `dataframe` when using rxReadXdf or rxXdfToDataFrame.|
| mapMissingCodes | Character string specifying how to handle SAS variables with multiple missing value codes. If "all", all of the values set as missing in SAS will be treated as NA. If "none", the missing value specification in SAS will be ignored and the original values will be imported. If "first", the values equal to the first missing value code will be imported as NA, while any other missing value codes will be treated as the original values. |
| varsToKeep | Character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`.|
| varsToDrop | Character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`.|
| checkVarsToKeep | Logical value. If `TRUE` variable names specified in `varsToKeep` will be checked against variables in the data set to make sure they exist. An error will be reported if not found. Ignored if more than 500 variables in the data set.|
| x | An RxSasData object.|
| n | Positive integer. Number of rows of the data set to extract.|
| addrownums | Logical. If TRUE, row numbers will be created to match the original data set.|
| reportProgress | Integer value with options: <br/>- "0" (no progress is reported)<br/>- "1" (the number of processed rows is printed and updated)<br/>- "2" (rows processed and timings are reported)<br/>- "3" (rows processed and all timings are reported)<br/> |
| ... | Arguments to be passed to underlying functions.| 

## Remarks
The `tail` method is not functional for this data source type and will report an error.

## Return Value
Object of class RxSasData.

## Example
~~~~
################################################################################
# Importing a SAS file
################################################################################
# SAS file name
claimsSasFileName <- file.path(rxGetOption("sampleDataDir"), "claims.sas7bdat")

# Import the SAS data into a data frame
claimsDF <- rxImport(claimsSasFileName)
rxGetInfo(claimsDF, getVarInfo = TRUE, numRows = 5)

# XDF file name
claimsXdfFileName <- file.path(tempdir(), "importedClaims.xdf")

# Import the data into the xdf file
rxImport(claimsSasFileName, claimsXdfFileName, overwrite = TRUE)

# Instead, import SAS file into a data frame
claimsIn <- rxImport(claimsSasFileName)
rxGetInfo(claimsIn, getVarInfo = TRUE, numRows = 5)

# Clean up
file.remove(claimsXdfFileName)

################################################################################
# Using a SAS data source
################################################################################
sasDataFile <- file.path(rxGetOption("sampleDataDir"),"claims.sas7bdat")

# Create a SAS data source with information about variables and
# rows to read in each chunk
sasDS <- RxSasData(sasDataFile, stringsAsFactors = TRUE,
	    colClasses = c(RowNum = "integer"),
        rowsPerRead = 50)

# Compute and draw a histogram directly from the SAS file
rxHistogram( ~cost|type, data = sasDS)

# Compute summary statistics
rxSummary(~., data = sasDS)

# Estimate a linear model
linModObj <- rxLinMod(cost~age + car_age + type, data = sasDS)
summary(linModObj)

# Import a subset into a data frame for further inspection
subData <- rxImport(inData = sasDS, rowSelection = cost > 400,
	varsToKeep = c("cost", "age", "type"))
subData
~~~~

## See Also
[ScaleR Functions (RevoScaleR package)](scaler.md)

[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)
