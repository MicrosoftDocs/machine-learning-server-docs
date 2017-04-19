--- 
 
# required metadata 
title: "Generate SPSS Data Source Object" 
description: " Generate an RxSpssData object that contains information about an SPSS data set to be imported or analyzed. RxSpssData is an S4 class, which extends  RxDataSource." 
keywords: "RevoScaleR, RxSpssData, head.RxSpssData, tail.RxSpssData, file, connection" 
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
 
 
 
 
 #`RxSpssData`: Generate SPSS Data Source Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Generate an RxSpssData object that contains information about an SPSS data
set to be imported or analyzed. RxSpssData is an S4 class, which extends 
RxDataSource. 
 
 ##Usage

```   
  RxSpssData(file, stringsAsFactors = FALSE, colClasses = NULL, colInfo = NULL,
             rowsPerRead = 500000, labelsAsLevels = TRUE, labelsAsInfo = TRUE,
             mapMissingCodes = "all", varsToKeep = NULL, varsToDrop = NULL, 
             checkVarsToKeep = FALSE)
             
  
 ## S3 method for class `RxSpssData':
head  (x, n = 6L, reportProgress = 0L, ...)
 ## S3 method for class `RxSpssData':
tail  (x, n = 6L, addrownums = TRUE, reportProgress = 0L, ...) 
             
 
```
 
 
 ##Arguments

   
    
 ### `file`
 character string specifying an SPSS data file of type .sav. 
  
  
    
 ### `stringsAsFactors`
 logical indicating whether or not to automatically convert strings to factors on import. This can be overridden by specifying `"character"` in `colClasses` and `colInfo`. If `TRUE`, the factor levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`. 
  
  
    
 ### `colClasses`
 character vector specifying the column types to use when converting the data. The element names for the vector are used to identify which column should be converted to which type.  
*   Allowable column types are:  
   *   `"logical"` (stored as `uchar`), 
   *   `"integer"` (stored as `int32`), 
   *   `"float32"` (the default for floating point data for .xdf files), 
   *   `"numeric"` (stored as `float64` as in R), 
   *   `"character"` (stored as `string`), 
   *   `"factor"` (stored as `uint32`), 
   *   `"int16"` (alternative to integer for smaller storage space), 
   *   `"uint16"` (alternative to unsigned integer for smaller storage space) 
   *   `"Date"` (stored as Date, i.e. `float64`) 
 
*   Note for `"factor"` type, the levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`. 
*   Note that equivalent types share the same bullet in the list above; for some types we allow both 'R-friendly' type names, as well as our own, more specific type names for .xdf data. 
*   Note also that specifying the column as a "factor" type is currently equivalent to "string" - for the moment, if you wish to import a column as factor data you must use the `colInfo` argument, documented below. 
  
  
  
    
 ### `colInfo`
 list of named variable information lists. Each variable information list contains one or more of the named elements given below. The information supplied for `colInfo` overrides that supplied for `colClasses`.   
*   Currently available properties for a column information list are:  
* `type` - character string specifying the data type for the column. See `colClasses` argument description for the available types.  
* `newName` - character string specifying a new name for the variable.   
* `description` - character string specifying a description  for the variable.   
* `levels` - character vector containing the levels when `type = "factor"`.  If the levels property is not provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value.  
* `newLevels` - new or replacement levels  specified for a column of type "factor". It must be used in conjunction with the `levels` argument.  After reading in the original data, the labels for each level will be replaced with the `newLevels`.  
* `low` - the minimum data value in the variable (used in computations using the `F()` function.  
* `high` - the maximum data value in the variable (used in computations using the `F()` function.  
 
  
  
  
    
 ### `rowsPerRead`
 number of rows to read at a time. This will determine the size of a block in the .xdf file if using `rxImport`. 
  
  
 ### `labelsAsLevels`
 logical.  If `TRUE`, variables containing value labels in the SPSS file will be converted to factors, using the value labels as factor levels. 
  
  
    
 ### `labelsAsInfo`
 logical.  If `TRUE`, variables containing value labels in the SPSS file that are not converted to factors will retain the information as valueInfoCodes and valueInfoLabels in the .xdf file.   This information can be obtained using [rxGetVarInfo](rxGetVarInfoXdf.md).  This information will also be returned as attributes for the columns in a  dataframe when using [rxDataStep](rxDataStep.md). 
  
  
    
 ### `mapMissingCodes`
 character string specifying how to handle SPSS variables with multiple missing value codes.  If `"all"`, all of the values set as missing in SPSS will be treated as `NA`.  If `"none"`, the missing value specification in SAS will be ignored and the original values will be imported.  If `"first"`, the values equal to the first missing value code will be imported as `NA`, while any other missing value codes will be treated as the original values.  
  
  
    
 ### `varsToKeep`
 character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `checkVarsToKeep`
 logical value.  If `TRUE` variable names specified in `varsToKeep` will be checked against variables in the data set to make sure they exist.  An error will be reported if not found. Ignored if more than 500 variables in the data set. 
  
  
     
 ### `x`
 an `RxSpssData` object 
  
   
     
 ### `n`
 positive integer. Number of rows of the data set to extract. 
  
   
     
 ### `addrownums`
 logical. If `TRUE`, row numbers will be created to match the original data set. 
  
   
     
 ### `reportProgress`
 integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
  
   
  
     
 ### ` ...`
 arguments to be passed to underlying functions 
  
 
 
 ##Details
 
The `tail` method is not functional for this data source type and will report an error.
 
 
 ##Value
 
object of class RxSpssData.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxSpssData-class](RxSpssData-class.md),
[rxNewDataSource](rxNew.md),
[rxImport](rxImport.md).
   
 
 ##Examples

 ```
   
  # Create a SPSS data source
  claimsSpssFileName <- file.path(rxGetOption("sampleDataDir"), "claims.sav")
  claimsSpssSource <- RxSpssData(claimsSpssFileName)
  
  # Specify an xdf data source
  claimsXdfFileName <- file.path(tempdir(), "importedClaims.xdf")
  
  # Import the data into the xdf file
  myXdfDataSource <- rxImport(claimsSpssSource, claimsXdfFileName, overwrite = TRUE)
  
  # Instead, import the (small) data set into a data frame
  claimsIn <- rxImport(claimsSpssSource)
  head(claimsIn)
  
  # Clean up
  file.remove(claimsXdfFileName)
  
  
     
 
```
 
 
 
