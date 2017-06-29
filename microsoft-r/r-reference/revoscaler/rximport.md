--- 
 
# required metadata 
title: "Import Data to .xdf or data frame" 
description: " Import data into an .xdf file or `data.frame`. " 
keywords: "RevoScaleR, rxImport, file, connection" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 #`rxImport`: Import Data to .xdf or data frame

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Import data into an .xdf file or `data.frame`.
 
 
 ##Usage

```   
  rxImport(inData, outFile = NULL, varsToKeep = NULL, 
          varsToDrop = NULL, rowSelection = NULL, 
          transforms = NULL, transformObjects = NULL,
          transformFunc = NULL, transformVars = NULL,  
          transformPackages = NULL, transformEnvir = NULL,
          append = "none", overwrite = FALSE, numRows = -1,
          stringsAsFactors = NULL, colClasses = NULL, colInfo = NULL,
          rowsPerRead = NULL, type = "auto", maxRowsByCols = NULL, 
          reportProgress = rxGetOption("reportProgress"),
          verbose = 0,
          xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
          createCompositeSet = NULL,
          blocksPerCompositeFile = 3,
          ...)
 
```
 
 ##Arguments

   
    
 ### `inData`
 a character string with the path for the data  to import (delimited, fixed format, SPSS, SAS, ODBC, or XDF). Alternatively, a  data source object representing the input data source can be  specified. (See [RxTextData](rxtextdata.md), [RxSasData](rxsasdata.md), [RxSpssData](rxspssdata.md), and [RxOdbcData](rxodbcdata.md).) If a character  string is supplied and `type` is set to `"auto"`, the type of file is  inferred from its extension, with the default being a text file. A  `data.frame` can also be used for `inData`. 
  
  
    
 ### `outFile`
 a character string representing the output .xdf file, a  [RxHiveData](rxsparkdata.md) data source, a[RxParquetData](rxsparkdata.md) data source or   a [RxXdfData](rxxdfdata.md) object.   If `NULL`, a data frame will be returned in memory. 
  
   
    
 ### `varsToKeep`
 character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. Not supported for ODBC or fixed format text files. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. Not supported for ODBC or fixed format text files. 
   
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `append`
 either `"none"` to create a new .xdf file or `"rows"` to append rows to an existing .xdf file. If `outFile` exists and `append` is `"none"`, the `overwrite` argument must be set to `TRUE`. Ignored if a data frame is returned. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, the existing `outData` will be overwritten. Ignored if a dataframe is returned. 
  
  
    
 ### `numRows`
 integer value specifying the maximum number of rows to import. If set to -1, all rows will be imported. 
  
  
     
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
   *   `"ordered"` (ordered factor stored as `uint32`. Ordered factors are treated the same as factors in RevoScaleR analysis functions.),           
   *   `"int16"` (alternative to integer for smaller storage space), 
   *   `"uint16"` (alternative to unsigned integer for smaller storage space), 
   *   `"Date"` (stored as Date, i.e. `float64`. Not supported for import types `"textFast"`, `"fixedFast"`, or `"odbcFast"`.) 
   *   `"POSIXct"` (stored as POSIXct, i.e. `float64`. Not supported for import types `"textFast"`, `"fixedFast"`, or `"odbcFast"`.) 
 
*   Note for `"factor"` and `"ordered"` types, the levels will be coded in the order encountered. Since this factor level ordering is row dependent, the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`. 
*   Note that equivalent types share the same bullet in the list above; for some types we allow both 'R-friendly' type names, as well as our own, more specific type names for .xdf data. 
*   Note also that specifying the column as a "factor" type is currently equivalent to "string" - for the moment, if you wish to import a column as factor data you must use the `colInfo` argument, documented below. 
  
  
  
    
 ### `colInfo`
 list of named variable information lists. Each variable information list contains one or more of the named elements given below. When importing fixed format data, either `colInfo` or an an .sts schema file should be supplied. For fixed format text files, only the variables specified will be imported. For all text types, the information supplied for `colInfo`overrides that supplied for `colClasses`.   
*   Currently available properties for a column information list are:  
* `type` - character string specifying the data type for the column. See `colClasses` argument description for the available types. If the `type` is not specified for fixed format data, it will be read as character data.  
* `newName` - character string specifying a new name for the variable.   
* `description` - character string specifying a description  for the variable.   
* `levels` - character vector containing the levels when `type = "factor"`.  If the levels property is not provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value.  
* `newLevels` - new or replacement levels  specified for a column of type "factor". It must be used in conjunction with the `levels` argument.  After reading in the original data, the labels for each level will be replaced with the `newLevels`.  
* `low` - the minimum data value in the variable (used in computations using the `F()` function.)  
* `high` - the maximum data value in the variable (used in computations using the `F()` function.)  
* `start` - the left-most position, in bytes, for the column  of a fixed format file respectively. When all elements of `colInfo` have  `start`, the text file is designated as a fixed format file.  When none of the elements have it, the text file is designated as a delimited file.  Specification of `start` must always be accompanied by specification of `width`.  
* `width` - the number of characters in a fixed-width character column or the column of a fixed format file. If `width`is specified for a character column, it will be imported as a fixed-width character variable.  Any characters beyond the fixed width will be ignored. Specification of `width` is required for all columns of a fixed format file.   
* `decimalPlaces` - the number of decimal places.  
 
  
  
  
    
 ### `rowsPerRead`
 number of rows to read at a time. 
  
  
    
 ### `type`
 character string set specifying file type of `inData`. This is ignored if `inData` is a data source. Possible values are:   
   *   `"auto"`: file type is automatically detected by looking at file extensions and argument values. 
   *   `"textFast"`: delimited text import using faster, more limited import mode. By default variables containing the values `TRUE` and `FALSE` or `T` and `F` will be created as logical variables. 
   *   `"text"`: delimited text import using enhanced, slower import mode (not supported with HDFS). This allows for importing Date and POSIXct data types, handling the delimiter character inside a quoted string, and specifying decimal character and thousands separator. (See [RxTextData](rxtextdata.md).) 
   *   `"fixedFast"`: fixed format text import using faster, more limited import mode. You must specify a .sts format file or colInfo specifications with `start` and `width`for each variable.    
   *   `"fixed"`: fixed format text import using enhanced, slower import mode (not supported with HDFS). This allows for importing Date and POSIXct data types and specifying decimal character and thousands separator.  You must specify a .sts format file or colInfo specifications with `start` and `width`for each variable.      
   *   `"sas"`: SAS data files. (See [RxSasData](rxsasdata.md).)	   
   *   `"spss"`: SPSS data files. (See [RxSpssData](rxspssdata.md).) 
   *   `"odbcFast"`: ODBC import using faster, more limited import mode. 
   *   `"odbc"`: ODBC import using slower, enhanced import on Windows.  (See [RxOdbcData](rxodbcdata.md).)	   
 
  
  
    
 ### `maxRowsByCols`
 the maximum size of a data frame that will be read in if `outData` is set to `NULL`, measured by the number of rows times the number of columns. If the number of rows times the number of columns being imported exceeds this,  a warning will be reported and a smaller number of rows will be read in than requested. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory. 
  
  
    
 ### `reportProgress`
 integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, information on the import type is printed if `type` is set  to `auto`. 
   
  
     
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
  
   
     
 ### `createCompositeSet`
 logical value or `NULL`. If `TRUE`, a composite set of files will be created instead of a single .xdf file. A directory will be created whose name is the same as the .xdf file that would otherwise be created, but with no extension. Subdirectories data and metadata will be created. In the data subdirectory, the data will be split across a set of .xdfd files (see `blocksPerCompositeFile` below for determining how many blocks of data will be in each file). In the metadata subdirectory  there is a single .xdfm file, which contains the meta data for all of the  .xdfd files in the  data subdirectory. When the compute context is `RxHadoopMR` a composite set of files is always created. 
  
  
    
 ### `blocksPerCompositeFile`
 integer value. If `createCompositeSet=TRUE`, and if the compute context is not `RxHadoopMR`, this will be the number of blocks put into each .xdfd file in the composite set. When importing is being done on Hadoop using MapReduce, the number of rows per .xdfd file is determined by the rows assigned to each MapReduce task, and the number of blocks per .xdfd file is therefore determined by `rowsPerRead`. If the `outFile` is an [RxXdfData](rxxdfdata.md) object, set the value for `blocksPerCompositeFile` there instead. 
   
   
    
 ### ` ...`
 additional arguments to be passed directly to the underlying data source objects to be imported. These argument values will override the existing values in an existing data source, if it is passed in as the `inData`. See [RxTextData](rxtextdata.md), [RxSasData](rxsasdata.md), [RxSpssData](rxspssdata.md), and [RxOdbcData](rxodbcdata.md). 
  
 
 
 
 ##Details
 
If a data source is passed in as `inData`, argument values specified in the call
to `rxImport` will override any existing specifications in the data source. Setting
`type` to `"text"`, `"fixed"`, or `"odbc"` is equivalent to setting 
`useFastRead` to `FALSE` in an [RxTextData](rxtextdata.md) or [RxOdbcData](rxodbcdata.md)
input data source.  Similarly, setting `type` to `"textFast"`, `"fixedFast"`, 
or `"odbcFast"` is equivalent to setting `useFastRead` to `TRUE`.

The input and output data sources are automatically opened and closed within
the `rxImport` function.

For reasons of performance, the `textFast` 'type' does not properly handle text 
files that contain the delimiter character inside a quoted string 
(for example, the entry "Wade, John" inside a comma delimited file. Use the
`text` 'type' if your data set contains character data with this characteristic.

For information on using a .sts schema file for fixed format text import,
see the [RxTextData](rxtextdata.md) help file.

**Encoding Details**

Some files or data sources contain character data encoded in a particular format 
(for example, Excel files will always use UTF-16).  Others may contain encoding 
information inside the file itself.  When not determined by the file type, or specified
within the file, character data in the input file or data source must be encoded as 
ASCII or UTF-8 in order to be imported correctly.  If the data 
contains UTF-8 multibyte (i.e., non-ASCII) characters, make sure the `type` 
parameter is set to `"text"` or `"fixed"` for text import and `"odbc"` for ODBC import as 
the 'fast' versions of these values may not handle extended UTF-8 characters correctly.
 
 
 
 ##Value
 
If an `outFile` is not specified, an output
data frame is returned.  If an `outFile` is specified, an 
[RxXdfData](rxxdfdata.md) data source is returned that can be used in
subsequent RevoScaleR analysis.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxDataSource-class](rxdatasource-class.md),
[rxDataStep](rxdatastep.md),
[RxTextData](rxtextdata.md),
[RxSasData](rxsasdata.md),
[RxSpssData](rxspssdata.md),
[RxOdbcData](rxodbcdata.md),
[RxXdfData](rxxdfdata.md),
[rxSplit](rxsplitxdf.md),
[rxTransform](rxtransform.md).
   
 ##Examples

 ```
   
  
  ##############################################################################
  # Import a comma-delimited text file into an .xdf file
  ##############################################################################
  # Create names for input and output data files
  sampleDataDir <- rxGetOption("sampleDataDir")
  inputFile <- file.path(sampleDataDir, "claims.txt")
  outputFile <- file.path(tempdir(), "claims.xdf")
  
  # Import the data
  rxImport( inData = inputFile, outFile = outputFile)
  
  # Look at summary information 
  rxGetInfo( data = outputFile, getVarInfo = TRUE)
  
  # Clean-up: remove data file
  file.remove( outputFile )
  
  ##############################################################################
  # Import a small SAS file into a data frame in memory
  ##############################################################################
  # Specify a SAS file name
  claimsSasFileName <- file.path(rxGetOption("sampleDataDir"), "claims.sas7bdat")
  
  # Import the data into a data frame in memory
  claimsIn <- rxImport(inData = claimsSasFileName)
  head(claimsIn)
  
  ##############################################################################
  # Import a fixed format text file into an .xdf file
  ##############################################################################
  # Specify input and output file names
  
  claimsFF <- file.path(rxGetOption("sampleDataDir"), "claims.dat")
  claimsXdf <- file.path(tempdir(), "importedClaims.xdf")
  
  # Specify column information about the input file
  claimsFFColInfo <-
    list(rownames = list(start = 1, width = 3),
         age = list(type = "factor", start = 4, width = 5),
         car.age = list(type = "factor", start = 9, width = 3),
         type = list(type = "factor", start = 12, width = 1),
         cost = list(type = "numeric", start = 13, width = 6),
         number = list(type = "integer", start = 19, width = 3))
  
  # Import the data into the xdf file
  rxImport(inData = claimsFF, outFile = claimsXdf, 
  	colInfo = claimsFFColInfo, 
  	rowsPerRead = 65, # rowsPerRead will determine block size
  	overwrite = TRUE)
  	
  # Look at information about the new file
  rxGetInfo( data = claimsXdf, getVarInfo = TRUE)
  
  # Clean-up: delete the new file
  file.remove( claimsXdf)
  
  ##############################################################################
  # Import a fixed format text file using an RxTextData data source
  ##############################################################################
  
  claimsFF <- file.path(rxGetOption("sampleDataDir"), "claims.dat")
  claimsXdf <- file.path(tempdir(), "importedClaims.xdf")
  
  # Create an RxTextData data source
  
  claimsDS <- RxTextData( file = claimsFF,
  	colInfo =  list(
  	   rownames = list(start = 1, width = 3),
         age = list(type = "factor", start = 4, width = 5),
         car.age = list(type = "factor", start = 9, width = 3),
         type = list(type = "factor", start = 12, width = 1),
         cost = list(type = "numeric", start = 13, width = 6),
         number = list(type = "integer", start = 19, width = 3)),
         rowsPerRead = 50 # rowsPerRead will determine block size
         )
  
  # Import the data specified in the data source into an xdf file
  
  rxImport(inData = claimsDS, outFile = claimsXdf,
  	rowsPerRead = 65, # this will override 'rowsPerRead' in the data source
  	overwrite = TRUE)
  	
  # Clean-up: delete the new file
  file.remove( claimsXdf)
  	
  
  ##############################################################################
  # Import a an SPSS file into an Xdf file
  ##############################################################################
  
  # Specify SPSS file name
  spssFileName <- file.path(rxGetOption("unitTestDataDir"), "claimsInt.sav")
  
  # Create an XDF data file path
  xdfFileName <- file.path(tempdir(), "ClaimsInt.xdf")
  
  # Import data; notice that factor variables are created for any
  # SPSS variables with value labels
  rxImport(inData = spssFileName, outFile = xdfFileName, overwrite = TRUE, reportProgress = 0)
  varInfo <- rxGetVarInfo( data = xdfFileName )
  varInfo
  
  # Reimport SPSS data, storing some variables as integer instead of
  # numeric
  
  spssColClasses <- c(RowNum = "integer", cost = "integer", 
    number = "integer")
  rxImport(inData = spssFileName, outFile = xdfFileName, colClasses = spssColClasses,
     overwrite = TRUE, reportProgress = 0)
  varInfo <- rxGetVarInfo( data = xdfFileName )
  varInfo  
  
  # Reimport SPSS data, changing re-specifing the strings used
  # for factor levels
  
  spssColInfo <- list(
    RowNum = list(type="integer", newName = "RowNumber"),
    cost = list(type="integer", newName = "TotalCost", description = "Cost of Auto"),
    number = list(type="integer", newName = "Number", description = "Number of Claims"),
    type = list(
  	levels = as.character(1:4),
  	newLevels = c("Small", "Medium", "Large", "Huge"))) 
  rxImport(inData = spssFileName, outFile = xdfFileName, 
      colInfo = spssColInfo, overwrite = TRUE, reportProgress = 0)
  varInfo <- rxGetVarInfo( data = xdfFileName )
  varInfo  
  
  # Clean-up: delete the new file
  file.remove( xdfFileName )
  
 
```
 
 
 
 
 
 
 
 
 
