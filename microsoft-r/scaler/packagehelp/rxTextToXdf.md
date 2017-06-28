--- 
 
# required metadata 
title: "Text File Data Import (to .xdf)" 
description: " Import text data into to an .xdf file using `fastText` mode. Can also use rxImport. " 
keywords: "RevoScaleR, rxTextToXdf, file, connection" 
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
 
 
 #`rxTextToXdf`: Text File Data Import (to .xdf)

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Import text data into to an .xdf file using `fastText` mode. Can also
use rxImport.
 
 
 ##Usage

```   
  rxTextToXdf(inFile, outFile, rowSelection = NULL, rowsToSkip = 0,
              transforms = NULL, transformObjects = NULL, 
              transformFunc = NULL, transformVars = NULL,
              transformPackages = NULL, transformEnvir = NULL, 
              append = "none", overwrite = FALSE, numRows = -1,
              stringsAsFactors = FALSE, colClasses = NULL, colInfo = NULL,
              missingValueString = "NA",
              rowsPerRead = 500000, columnDelimiters = NULL, 
              autoDetectColNames = TRUE, firstRowIsColNames = NULL, 
              rowsToSniff = 10000, defaultReadBufferSize = 10000,
              defaultDecimalColType = rxGetOption("defaultDecimalColType"),
              defaultMissingColType = rxGetOption("defaultMissingColType"),
              reportProgress = rxGetOption("reportProgress"),
              xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
              createCompositeSet = FALSE,
  			blocksPerCompositeFile = 3)
 
```
 
 ##Arguments

   
    
 ### `inFile`
 character string specifying the input comma or whitespace delimited text file. 
  
  
    
 ### `outFile`
 either an RxXdfData object or a character string specifying the output .xdf file. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `rowsToSkip`
 integer indicating number of leading rows to ignore. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `append`
 either `"none"` to create a new .xdf file or `"rows"` to append rows to an existing .xdf file. If `outFile` exists and `append` is `"none"`,  then `overwrite` argument must be set to `TRUE`. Note that appending to a factor column normally extends its levels with whatever values are in the appended data, but if levels are explicitly specified using `colInfo` then it will be extended only with the specified levels - any other values will be added as missings. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, an existing `outFile` will be overwritten, or if appending columns existing columns with the same name will be overwritten. `overwrite` is ignored if appending rows. 
  
  
    
 ### `numRows`
 integer value specifiying the maximum number of rows to import. If set to -1, all rows will be imported. 
  
  
    
 ### `stringsAsFactors`
 logical indicating whether or not to automatically convert strings to factors on import. This can be overridden by specifying `"character"` in `colClasses` and `colInfo`. If `TRUE`, the factor levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`. 
  
  
    
 ### `colClasses`
 character vector specifying the column types to use when converting the data. The element names for the vector are used to identify which column should be converted to which type.   
*   Allowable column types are:  
   *   "logical" (stored as `uchar`), 
   *   "integer" (stored as `int32`), 
   *   "factor" (stored as `uint32`), 
   *   "ordered" (an ordered factor stored as `uint32`.  Ordered factors are treated the same as factors in RevoScaleR analysis functions.          ), 
   *   "float32" (the default for floating point data for .xdf files), 
   *   "numeric" (stored as `float64` as in R), 
   *   "character" (stored as `string`),  
   *   "int16" (alternative to integer for smaller storage space), 
   *   "uint16" (alternative to unsigned integer for smaller storage space) 
 
*   Note for `"factor"` and `"ordered"` type, the levels will  be coded in the order encountered. Since this factor level ordering is row dependent,  the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`. 
*   Note that equivalent types share the same bullet in the list above; for some types we allow both 'R-friendly' typenames, as well as our own, more specific type names for .xdf data. 
*   Note also that specifying the column as a "factor" type is currently equivalent to "string" - for the moment, if you wish to import a column as factor data you must use the `colInfo` argument, documented below. 
  
  
  
    
 ### `colInfo`
 list of named variable information lists. Each variable information list contains one or more of the named elements given below. The information supplied for `colInfo` overrides that supplied for `colClasses`.   
*   Currently available properties for a column information list are:  
* `type(character)` - character string specifying the data type for the variable. See `colClasses` argument description for the available types.  
* `newName(character)` - character string specifying a new name for the variable.   
* `description(character)` - character string specifying a description  for the variable.   
* `levels(character vector)` - levels specified for a column of type "factor".  If the levels property is not provided, factor levels will be determined by the entries in the source column. Blanks or empty strings will be set to missing values. If levels are provided, any source column entry that does not match a provided level will be converted to a missing value. If an ".rxOther" level is included, all missings will be assigned to that level.  
* `newLevels(character vector)` - new or replacement levels  specified for a column of type "factor". Must be used in conjunction with the `levels` component of `colClasses`.  After reading in the original data, the labels for each level will be replaced with the `newLevels`.  
* `low` - the minimum data value in the variable (used in computations using the `F()` function.  
* `high` - the maximum data value in the variable (used in computations using the `F()` function.  
 
  
  
  
    
 ### `missingValueString`
 character string containing the missing value code. 
  
  
    
 ### `rowsPerRead`
 number of rows to read for each chunk of data; if `NULL`, all rows are read. This is the number of rows written per block to the .xdf file unless the number of rows in the read chunk is modified through code in a [transformFunc](rxTransform.md). 
  
  
    
 ### `columnDelimiters`
 character string containing characters to be used as delimiters. If `NULL`, the file is assumed to be either comma or tab delimited. 
  
  
    
 ### `autoDetectColNames`
 DEPRECATED. Use `firstRowIsColNames`. 
  
  
    
 ### `firstRowIsColNames`
 logical indicating if the first row represents column names. If `firstRowIsColNames` is `NULL`, then column names are auto- detected. The logic for auto-detection is: if the first row contains all values that are interpreted as character and the second row contains at least one value that is interpreted as numeric, then the first row is considered to be column names; else the first row is considered to be the first data row. 
  
  
    
 ### `rowsToSniff`
 number of rows to use in determining column type. 
  
  
    
 ### `defaultReadBufferSize`
 number of rows to read into a temporary buffer. This value could affect the speed of import. 
  
  
    
 ### `defaultDecimalColType`
 Used to specify a column's data type when  only decimal values (possibly mixed with missing (`NA`) values) are encountered upon first read of the data and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are "float32" and "numeric", for 32-bit floating point and 64-bit  floating point values, respectively. 
  
  
    
 ### `defaultMissingColType`
 Used to specify a given column's data type when  only missings (`NA`s) or blanks are encountered upon first read of the data  and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are "float32", "numeric", and "character" for 32-bit floating point, 64-bit floating point and string values, respectively. 
  
  
    
 ### `reportProgress`
 integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
  
  
  
     
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
  
  
     
 ### `createCompositeSet`
 logical value. EXPERIMENTAL. If `TRUE`, a composite set of files will be created instead of a single .xdf file. A directory will be created whose name is the same as the .xdf file that would otherwise be created, but with no extension. Subdirectories data and metadata will be created. In the data subdirectory, the data will be split across a set of .xdfd files (see `blocksPerCompositeFile` below for determining how many blocks of data will be in each file). In the metadata subdirectory  there is a single .xdfm file, which contains the meta data for all of the  .xdfd files in the  data subdirectory. When the `fileSystem` is "hdfs" a composite set of files is always created. 
  
  
    
 ### `blocksPerCompositeFile`
 integer value. EXPERIMENTAL. If `createCompositeSet=TRUE`, and if the compute context is not `RxHadoopMR`, this will be the number of blocks put into each .xdfd file in the composite set. When importing is being done on Hadoop using MapReduce, the number of rows per .xdfd file is determined by the rows assigned to each MapReduce task, and the number of blocks per .xdfd file is therefore determined by `rowsPerRead`. 
   
  
 
 
 
 ##Details
 
Decimal data in text files can be imported into .xdf files and 
stored as either 32-bit floats or 64-bit doubles. 
The default for this is 32-bit floats, which can be changed using
[rxOptions](rxOptions.md).
 
If stored in 32-bit floats, they are converted into 64-bit
doubles whenever they are brought into R. Because there may be no exact binary
representation of a particular decimal number, the resulting double may be
(slightly) different from a double created by directly converting a decimal
value to a double. Thus exact comparisons of values may result in unexpected
behavior.  For example, if `x` is imported from a text file with a decimal
value of `"1.2"` and stored as a float in an .xdf file, the closest
decimal representation of the stored value displayed as a double is
`1.2000000476837158`. So, bringing it into R as a double and comparing with
a decimal constant of `1.2`, e.g. `x == 1.2`, will result in
`FALSE` because the decimal constant `1.2` in the code is being
converted directly to a double.

To store imported text decimal data as 64-bit doubles in an .xdf file,
set the `"defaultDecimalColType"` to `"numeric"`. Doing so will increase the size
of the .xdf file, since the size of a double is twice that of a float.

An error message will be issued if the data being appended is of a different
type than the existing data and the conversion might result in a loss of data.  
For example, appending a float column to an integer column will result in an 
error unless the column type is explicitly set to `"integer"` using the
`colClasses` argument. If explicitly set, the float data will then be 
converted to integer data when imported.

`Date` is not currently supported in colClasses or colInfo, but `Date`
variables can be created by importing string data and converting to a
`Date` variable using as.Date in a `transforms`
argument.

**Encoding Details**

If the input data source or file contains non-ASCII character information, please use 
the function `rxImport` with the encoding settings recommended in the RxImport documentation 
under 'Encoding Details'
 
 
 ##Note
 
For reasons of performance, `rxTextToXdf` does not properly handle text
files that contain the delimiter character inside a quoted string (for
example, the entry `"Wade, John"` inside a comma delimited file. See
[rxImport](rxImport.md) with `type = "text"` for importing this type of data.

In addition `rxTextToXdf` currently requires that all rows of data in the
text file contain the same number of entries. Date, time, and currency data 
types are not currently supported and are imported as character data.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxImport](rxImport.md),
[rxDataStep](../../r-reference/revoscaler/rxdatastep.md),
[rxFactors](rxFactors.md),
[rxTransform](rxTransform.md).
   
 ##Examples

 ```
   
  ###
  # Example 1: Exploration of data types
  ###
  
  # 1a) Target column types automatically determined
  # New .xdf file has one integer column, three character columns,
  # and two 32-bit single precision floating point columns
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "basicClaims.xdf")
  rxTextToXdf(inFile = inputFile, outFile = outputFile, overwrite = TRUE)
  rxGetInfo(data = outputFile, getVarInfo = TRUE, numRows = 5)
  file.remove(outputFile)
  
  # 1b) Convert strings columns to factors
  # Result is similar to 1a) with the character columns now being stored
  # as factors
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "claimsWithFactors.xdf")
  rxTextToXdf(inFile = inputFile, outFile = outputFile, stringsAsFactors = TRUE, overwrite = TRUE)
  rxGetVarInfo(data = outputFile, getValueLabels = TRUE)
  file.remove(outputFile)
  
  # 1c) Interpret numeric columns as 64-bit doubles
  # Result is similar to 1a) with the floating point columns now being
  # stored as 64-bit double precision floating point columns
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "claimsWithDoubles.xdf")
  rxTextToXdf(inFile = inputFile, outFile = outputFile,
              colClasses = c(cost = "numeric", number = "numeric"),
              overwrite = TRUE)
  rxGetInfo(data = outputFile, getVarInfo = TRUE, numRows = 5)
  file.remove(outputFile)
  
  # 1d) Combination of 1b) and 1c)
  # Result is similar to what is produced by utils::read.csv
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "claimsWithFactorsAndDoubles.xdf")
  
  # Set options so that the default decimal type is numeric
  myDefaultColType <- rxGetOption( "defaultDecimalColType" )
  rxOptions(defaultDecimalColType = "numeric")
  rxTextToXdf(inFile = inputFile, outFile = outputFile, stringsAsFactors = TRUE,
              overwrite = TRUE)
  # Reset options to original settings
  rxOptions( defaultDecimalColType = myDefaultColType )            
  rxGetInfo(data = outputFile, getVarInfo = TRUE, numRows = 5)
  claimsWithFactorsAndDoubles <- rxDataStep(inData = outputFile, reportProgress = 0)
  
  claimsFromReadCsv <- read.csv(inputFile)
  levels(claimsFromReadCsv[["car.age"]])
  claimsFromReadCsv[["car.age"]] <-
    factor(claimsFromReadCsv[["car.age"]],
           levels = c("0-3", "4-7", "8-9", "10+"))
  all.equal(claimsWithFactorsAndDoubles, claimsFromReadCsv)
  file.remove(outputFile)
  
  # 1e) Build off of 1d), convert "number" column to a 16-bit integer
  # After reading into R, results still similar to utils::read.csv output
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "claimsWithFactorsDoublesAnd16BitInt.xdf")
  rxTextToXdf(inFile = inputFile, outFile = outputFile, stringsAsFactors = TRUE,
              colClasses =
              c(number = "int16", cost = "numeric", number = "numeric"),
              overwrite = TRUE)
  rxGetInfo(data = outputFile, getVarInfo = TRUE, numRows = 5)
  claimsWithFactorsDoublesAnd16BitInt <- rxDataStep(inData = outputFile, reportProgress = 0)
  all.equal(claimsWithFactorsDoublesAnd16BitInt, claimsFromReadCsv)
  file.remove(outputFile)
  
  
  ###
  # Example 2: Exploration of factor levels
  ###
  
  # 2a) For column "type" convert all non "A" and "C" values to missing values
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "claimsFactorExploration.xdf")
  rxTextToXdf(inFile = inputFile, outFile = outputFile,
              colInfo = list(type = list(type = "factor", levels = c("A", "C"))),
              overwrite = TRUE)
  rxGetInfo(data = outputFile, getVarInfo = TRUE, numRows = 5)
  
  # 2b) Append new data to .xdf file, where new data will extend the factor levels
  appendFile <- file.path(sampleDataDir, "claimsExtra.txt")
  rxTextToXdf(inFile = appendFile, outFile = outputFile,
              colInfo =
              list(type = list(type = "factor", levels = c("E", "F", "G"))),
              append = "rows")
  rxGetInfo(data = outputFile, getVarInfo = TRUE, numRows = 5)
  file.remove(outputFile)
  
  # 2c) Reimport claims data, making car.age an ordered factor
  # Then extract a data frame with cars more than 3 years old
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "claimsFactorExploration.xdf")
  rxTextToXdf(inFile = inputFile, outFile = outputFile,
              colInfo = list(car.age = list(type = "ordered", 
              levels = c("0-3", "4-7", "8-9", "10+"))),
              overwrite = TRUE)
  oldCars <- rxDataStep(inData = outputFile, rowSelection = car.age > "0-3")
   
  
  ###
  # Example 3: Exploring dynamic variable transformation and row selection
  ###
  
  # Create local data source and write as a .csv file
  tinyData <- data.frame(a = 1:4, b = c(4.1, 5.2, 6.3, 1.2),
                         c = c("one", "two", "three", "four"),
                         stringsAsFactors = FALSE)
  tinyText <- file.path(tempdir(), "rxTiny.txt")
  if (file.exists(tinyText)) file.remove(tinyText)
  write.csv(tinyData, file = tinyText, row.names = FALSE)
  
  # Create paths for .xdf file
  tinyXdf <- file.path(tempdir(), "rxTiny.xdf")
  if (file.exists(tinyXdf)) file.remove(tinyXdf)
  
  # Create a transformation list.
  # New variables : bSquared, bHalved, bIncremented
  # Alter existing variables : a, c
  # Delete existing variables : b
  transforms <- expression(list(
      bSquared = b^2,
      bHalved = b / 2,
      bIncremented = b + 1,
      c = toupper(c),
      a = rev(a),
      b = NULL))
  
  # Convert the existing text file to tinyXdf and transform variables along the way.
  # Read the tinyXdf data into a data frame and compare to the original.
  # Column "b" is explicitly typed as a 64-bit floating point number to show
  # equality with the original data
  rxTextToXdf(inFile = tinyText, outFile = tinyXdf, colClasses = c(b = "numeric"),
              firstRowIsColNames = TRUE, transforms = transforms)
  tinyDataFromXdf <- rxDataStep(inData = tinyXdf, reportProgress = 0)
  
  # Do a similar conversion but use a row selection expression to filter the rows.
  # * The row selection expression is based on a variable that is derived from
  #   column "b". In general, any column created using the transforms and
  #   transformFunc arguments can be used in the row selection process.
  # * Column "b" is explicitly typed as a 64-bit floating point number so one
  #   of its derived variables can be compared with an R numeric value in the
  #   row selection criterion.
  rowSelection <- expression(bHalved > 2.05)
  if (file.exists(tinyXdf)) file.remove(tinyXdf)
  rxTextToXdf(inFile = tinyText, outFile = tinyXdf, firstRowIsColNames = TRUE,
              transforms = transforms, rowSelection = rowSelection)
  tinyDataSubset <- rxDataStep(inData = tinyXdf, reportProgress = 0)
  tinyDataSubset
  file.remove(tinyText)
  file.remove(tinyXdf)
  
  ###
  # Example 4: Importing character data into a Date variable
  ###
  # The following can be used in as.Date format strings:
  # %b abbreviated month name
  # %B full month name
  # %m month as number (1-12)
  # %d day of the month
  # %y year, last two digits
  # %Y year, 
  
  txtFile <- file.path(rxGetOption("unitTestDataDir"), "AirlineSampleDate.csv")
  xdfFile <- file.path(tempdir(), ".rxTemp.xdf")
  	
  # Import as character data
  rxTextToXdf( inFile = txtFile, outFile = xdfFile, overwrite=TRUE)
  rxDataStep(inData = xdfFile, numRows = 5)
  # Example of Date variable is 1996-08-18
  	
  # Reimport as Date variable
  rxTextToXdf( inFile = txtFile, outFile = xdfFile,
  	transforms = list(Date = as.Date(Date, "%Y-%m-%d")),
  	overwrite = TRUE)
  
  # Look at variable information	
  rxGetVarInfo( data = xdfFile )
  
  # Read first 10 rows of the file into a data frame
  myData <- rxDataStep(inData = xdfFile, numRows = 10)
  myData
  
  # Clean up
  file.remove( xdfFile )
  
 
```
 
 
 
