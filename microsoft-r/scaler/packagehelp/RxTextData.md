--- 
 
# required metadata 
title: "Generate Text Data Source Object" 
description: " This is the main generator for S4 class RxTextData, which extends RxDataSource. " 
keywords: "RevoScaleR, RxTextData, file, connection" 
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
 
 
 #`RxTextData`: Generate Text Data Source Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
This is the main generator for S4 class RxTextData, which extends RxDataSource.
 
 
 ##Usage

```   
  RxTextData(file,  stringsAsFactors = FALSE,
             colClasses = NULL, colInfo = NULL, 
             varsToKeep = NULL, varsToDrop = NULL, missingValueString = "NA",
             rowsPerRead = 500000, delimiter = NULL, combineDelimiters = FALSE,
             quoteMark = "\"", decimalPoint = ".", thousandsSeparator = NULL,
             readDateFormat = "[%y[-][/]%m[-][/]%d]", 
             readPOSIXctFormat = "%y[-][/]%m[-][/]%d [%H:%M[:%S]][%p]",
             centuryCutoff = 20, firstRowIsColNames = NULL, rowsToSniff = 10000,
             rowsToSkip = 0, returnDataFrame = TRUE,
             defaultReadBufferSize = 10000, 
             defaultDecimalColType = rxGetOption("defaultDecimalColType"),
             defaultMissingColType = rxGetOption("defaultMissingColType"),
             writePrecision = 7, stripZeros = FALSE, quotedDelimiters = FALSE,
             isFixedFormat = NULL, useFastRead = NULL, 
             createFileSet = NULL, rowsPerOutFile = NULL,
             verbose = 0, checkVarsToKeep = FALSE,
             fileSystem = NULL, inputEncoding = "utf-8", writeFactorsAsIndexes = FALSE)
 
```
 
 
 
 ##Arguments

   
  
    
 ### `file`
 character string specifying a text file. If it has an .stsextension, it is interpreted as a fixed format schema file. If the `colInfo` argument contains `start` and `width` information, it is interpreted as a fixed format data file. Otherwise, it is treated as a delimited text data file. See the Details section for more information on using .sts files. 
  
  
    
 ### `returnDataFrame`
 logical indicating whether or not to convert the result from a list to a data frame (for use in `rxReadNext` only). If `FALSE`,  a list is returned. 
  
  
    
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
   *   `"Date"` (stored as Date, i.e. `float64`. Not supported if `useFastRead` = `TRUE`.) 
   *   `"POSIXct"` (stored as POSIXct, i.e. `float64`. Not supported if `useFastRead` = `TRUE`.) 
 
*   Note for `"factor"` and `"ordered"` types, the levels will  be coded in the order encountered. Since this factor level ordering is row dependent,  the preferred method for handling factor columns is to use `colInfo` with specified `"levels"`.  
  
  
  
    
 ### `colInfo`
 list of named variable information lists. Each variable information list contains one or more of the named elements given below. When importing fixed format data, either `colInfo` or an an .sts schema file should be supplied. For such fixed format data, only the variables specified will be imported. For all text types, the information supplied for `colInfo`overrides that supplied for `colClasses`.   
*   Currently available properties for a column information list are: 
* `type` - character string specifying the data type for the column. See `colClasses` argument description for the available types. If the `type` is not specified for fixed format data, it will be read as character data.  
* `newName` - character string specifying a new name for the variable.   
* `description` - character string specifying a description  for the variable.   
* `levels` - character vector containing the levels when `type = "factor"`.  If the levels property is not provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value.  
* `newLevels` - new or replacement levels  specified for a column of type "factor". It must be used in conjunction with the `levels` argument.  After reading in the original data, the labels for each level will be replaced with the `newLevels`.  
* `low` - the minimum data value in the variable (used in computations using the `F()` function.  
* `high` - the maximum data value in the variable (used in computations using the `F()` function.  
* `start` - the left-most position, in bytes, for the column  of a fixed format file respectively. When all elements of `colInfo` have  `"start"`, the text file is designated as a fixed format file.  When none of the elements have it, the text file is designated as a delimited file.  Specification of `start` must always be accompanied by specification of `width`.  
* `width` - the number of characters in a fixed-width character column or the column of a fixed format file. If `width`is specified for a character column, it will be imported as a fixed-width character variable.  Any characters beyond the fixed width will be ignored. Specification of `width` is required for all columns of a fixed format file (if not provided in an .sts file).   
* `decimalPlaces` - the number of decimal places.  
* `index` - column index in the original delimited text data file.  It is used as an alternative to naming the variable information list if the  original delimited text file does not contain column names.  Ignored if a name for the list is specified. Should not be used with fixed format files. 
 
  
  
  
    
 ### `varsToKeep`
 character vector of variable names to include when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to exclude when reading from the input data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `missingValueString`
 character string containing the missing value code. It can be an empty string: `""`. 
  
  
    
 ### `rowsPerRead`
 number of rows to read at a time. 
  
  
    
 ### `delimiter`
 character string containing the character to use as the separator between variables. If `NULL` and the `colInfo` argument does not contain `"start"` and `"width"` information (which implies a fixed-formatted file), the delimiter is auto-sensed from the list `","`, `"\t"`, `";"`, and `" "`. 
  
  
    
 ### `combineDelimiters`
 logical indicating whether or not to treat consecutive non-space (`" "`) delimiters as a single delimiter. Space `" "` delimiters are always combined.   
  
  
    
 ### `quoteMark`
 character string containing the quotation mark. It can be an empty string: `""`.  
  
  
    
 ### `decimalPoint`
 character string containing the decimal point.  Not supported when `useFastRead` is set to `TRUE`. 
  
  
    
 ### `thousandsSeparator`
 character string containing the thousands separator. Not supported when `useFastRead` is set to `TRUE`. 
  
  
   
  
    
 ### `readDateFormat`
 character string containing the time date format to use during read operations. Not supported when `useFastRead` is set to `TRUE`. Valid formats are:  
   *   `%c` Skip a single character (see also `%w`). 
   *   `%Nc` Skip `N` characters. 
   *   `%$c` Skip the rest of the input string. 
   *   `%d` Day of the month as integer (01-31).  
   *   `%m` Month as integer (01-12) or as character string.      
   *   `%w` Skip a whitespace delimited word (see also `%c`). 
   *   `%y` Year. If less than 100, `centuryCutoff` is used to determine the actual year. 
   *   `%Y` Year as found in the input string. 
   *   `%%`, `%[`, `%]` input the `%`, `[`, and `]` characters from the input string. 
   *   `[...]` square brackets within format specifications indicate optional components; if present, they are used, but they need not be there. 
  
  
  
    
 ### `readPOSIXctFormat`
 character string containing the time date format to use during read operations. Not supported when `useFastRead` is set to `TRUE`. Valid formats are:  
   *   `%c` Skip a single character (see also `%w`). 
   *   `%Nc` Skip `N` characters. 
   *   `%$c` Skip the rest of the input string. 
   *   `%d` Day of the month as integer (01-31). 
   *   `%H` Hour as integer (00-23). 
   *   `%m` Month as integer (01-12) or as character string. 
   *   `%M` Minute as integer (00-59). 
   *   `%n` Milliseconds as integer (00-999). 
   *   `%N` Milliseconds or tenths or hundredths of second. 
   *   `%p` Character string defining 'am'/'pm'. 
   *   `%S` Second as integer (00-59). 
   *   `%w` Skip a whitespace delimited word (see also `%c`). 
   *   `%y` Year. If less than 100, `centuryCutoff` is used to determine the actual year. 
   *   `%Y` Year as found in the input string. 
   *   `%%`, `%[`, `%]` input the `%`, `[`, and `]` characters from the input string. 
   *   `[...]` square brackets within format specifications indicate optional components; if present, they are used, but they need not be there. 
  
  
  
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
  
    
 ### `centuryCutoff`
 integer specifying the changeover year between the twentieth  and twenty-first century if two-digit years are read. Values less than `centuryCutoff`are prefixed by 20 and those greater than or equal to `centuryCutoff` are prefixed by 19. If you specify 0, all two digit dates are interpreted as years in the twentieth century. Not supported when `useFastRead` is set to `TRUE`. 
  
  
    
 ### `firstRowIsColNames`
 logical indicating if the first row represents column names for reading text. If `firstRowIsColNames` is `NULL`, then column names are auto- detected. The logic for auto-detection is: if the first row contains all values that are interpreted as character and the second row contains at least one value that is interpreted as numeric, then the first row is considered to be column names; otherwise the first row is considered to be the first data row. Not relevant for fixed format data. As for writing, it specifies to write column names as the first row.  If `firstRowIsColNames` is `NULL`, the default is to  write the column names as the first row. 
  
  
    
 ### `rowsToSniff`
 number of rows to use in determining column type. 
  
  
    
 ### `rowsToSkip`
 integer indicating number of leading rows to ignore. Only supported for `useFastRead = TRUE`. 
  
  
  
    
 ### `defaultReadBufferSize`
 number of rows to read into a temporary buffer. This value could affect the speed of import. 
  
  
    
 ### `defaultDecimalColType`
 Used to specify a column's data type when  only decimal values (possibly mixed with missing (`NA`) values) are encountered upon first read of the data and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are "float32" and "numeric", for 32-bit floating point and 64-bit  floating point values, respectively. 
  
  
    
 ### `defaultMissingColType`
 Used to specify a given column's data type when  only missings (`NA`s) or blanks are encountered upon first read of the data  and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are "float32", "numeric", and "character" for 32-bit floating point, 64-bit floating point and string values, respectively. Only supported for `useFastRead = TRUE`. 
  
  
    
 ### `writePrecision`
 integer specifying the precision to use when  writing numeric data to a file. 
  
   
    
 ### `stripZeros`
 logical scalar.  If `TRUE`, if there are only  zeros after the decimal point for a numeric, it will be written as  an integer to a file. 
  
   
    
 ### `quotedDelimiters`
 logical scalar.  If `TRUE`, delimiters  within quoted strings will be ignored. This requires a slower parsing  process. Only applicable to `useFastRead` is set to `TRUE`;  delimiters within quotes are always supported when `useFastRead`  is set to `FALSE` 
  .
   
    
 ### `isFixedFormat`
 logical scalar.  If `TRUE`, the input data file is treated as a fixed format file. Fixed format files must have a .sts file or a `colInfo` argument specifying the `start` and `width` of each variable.  If `FALSE`, the input data file is treated as a delimited text file.   If `NULL`, the text file type (fixed or delimited text) is auto-determined. 
  
  
    
 ### `useFastRead`
 `NULL` or logical scalar.  If `TRUE`, the data file is accessed directly by the Microsoft R Services Compute Engine.  If `FALSE`,  StatTransfer is used to access the data file.  If `NULL`, the type of text import is auto-determined. `useFastRead` should be set to `FALSE` if importing `Date` or `POSIXct` data types, if the data set contains the delimiter character inside a quoted string, if the `decimalPoint` is not `"."`, if the `thousandsSeparator` is not `","`, if the `quoteMark` is not `"\""`, or if `combineDelimiters` is set to `TRUE`. `useFastRead` should be set to `TRUE` if `rowsToSkip` or `defaultMissingColType` are set. If `useFastRead` is `TRUE`, by default variables containing the values `TRUE` and `FALSE` or `T` and `F` will be created as logical variables.  `useFastRead` cannot be set to `FALSE` if an HDFS file system is being used. If an incompatible setting of `useFastRead` is detected, a warning will be issued and the value will be changed. 
  
  
  
     
 ### `createFileSet`
 logical value or `NULL`. Used only when writing.  If `TRUE`, a file folder of text files will be created instead of a single text file. A directory will be created whose name is the same as the text file that would otherwise be created, but with no extension. In the directory, the data will be split across a set of text files (see `rowsPerOutFile` below for determining how many rows of data will be in each file). If `FALSE`, a single text file will be created.  If `NULL`, a folder of files will be created if the input data set is a composite XDF file or a folder of text files.  This argument is ignored if the text file is fixed format. 
  
  
   
     
 ### `rowsPerOutFile`
 numeric value or `NULL`. If a directory of text files is being created, and if the compute context is not `RxHadoopMR`, this will be the number of rows of data put into each file in the directory. When importing is being done on Hadoop using MapReduce, the number of rows per file is determined by the rows assigned to each MapReduce task. If `NULL`, the rows of data will match the input data. 
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, information on the text type (`text` or `textFast`) is printed. 
   
  
    
 ### `checkVarsToKeep`
 logical value.  If `TRUE` variable names specified in `varsToKeep` will be checked against variables in the data set to make sure they exist.  An error will be reported if not found. If there are more than 500 variables in the data set, this flag is ignored and no checking is performed. 
  
  
     
 ### `fileSystem`
 character string or [RxFileSystem](../../r-reference/revoscaler/rxfilesystem.md) object indicating type of file system;  `"native"`or `RxNativeFileSystem` object can be used for the local operating system, or an `RxHdfsFileSystem` object for the Hadoop file system. 
  
  
     
 ### `inputEncoding`
 character string indicating the encoding used by input text.   May be set to either `"utf-8"` (the default), or `"gb18030"`, a standard Chinese encoding.   Not supported when `useFastRead` is set to  `TRUE`. 
  
   
    
 ### `writeFactorsAsIndexes`
 logical. If `TRUE`, when writing to an `RxTextData` data source, underlying factor indexes will be written instead of the string representations.  Not supported when  `useFastRead` is set to `FALSE`. 
  
  
 
 
 
 ##Details
 
**Delimited Data Type Details**

Imported `POSIXct` will have the `tzone` attribute set to `"GMT"`.
 
Decimal data in text files can be imported into .xdf files and 
stored as either 32-bit floats or 64-bit doubles. 
The default for this is 32-bit floats, which can be changed using
[rxOptions](rxOptions.md). If the data type cannot be determined
(i.e., only missing values are encountered), the column is imported
as 64-bit doubles unless otherwise specified.
 
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
set the `defaultDecimalColType` to `"numeric"`. Doing so will increase the size
of the .xdf file, since the size of a double is twice that of a float.

**Fixed Format Schema Details**

The .sts schema for a fixed format data file consists of two required
components (`FILE` and `VARIABLES`) and one optional component
(`FIRST LINE`), representing the first line of the data file to read.

```
 
 ```
```
 FILE  file name and path specification
 ```
```
 FIRST LINE  n       when required
 ```
```
 VARIABLES
 ```
```
        Variable name | variable list | variable range columns  (A)
 ```

where (A) is used to tag character columns, else use no tag for numeric columns.

If the data file names and .sts file name are different, the full
path must be specified in the `FILE` component.

See `file.show(file.path(rxGetOption("sampleDataDir"), "claims.sts"))` for
an example fixed format schema file.

If a .sts schema file is used in addition to a `colInfo` argument,
schema file is read first, then the `colInfo` is used to modify that
information (for example, to specify that a variable should be read as a factor).


**Encoding Details**


Character data in the input file must be encoded as 
ASCII or UTF-8 in order to be imported correctly.  If the data 
contains UTF-8 multibyte (i.e., non-ASCII) characters, make sure the `useFastRead` 
parameter is set to `FALSE` as the 'fast' version of this object may not 
handle extended UTF-8 characters correctly.
 
 
 ##Value
 
object of class RxTextData.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxTextData-class](RxTextData-class.md),
[rxImport](../../r-reference/revoscaler/rximport.md),
[rxNewDataSource](rxNew.md).
   
 ##Examples

 ```
   
  ### Read from a csv file ###
  # Create a csv data source
  claimsCSVFileName <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
  claimsCSVDataSource <- RxTextData(claimsCSVFileName)
  
  # Specify the location for the new .xdf file
  claimsXdfFileName <- file.path(tempdir(), "importedClaims.xdf")
  
  # Import the data into the xdf file
  claimsDS <- rxImport(inData = claimsCSVDataSource, 
      outFile = claimsXdfFileName, overwrite = TRUE)
  
  # Look at file information
  rxGetInfo( claimsDS, getVarInfo = TRUE )
  
  # Clean-up: delete the new file
  file.remove( claimsXdfFileName)
  
  
  ### Read from a fixed format file ###
  # Create a fixed format data source
  claimsFFFileName <- file.path(rxGetOption("sampleDataDir"), "claims.dat")
  claimsFFColInfo <-
    list(rownames = list(start = 1, width = 3),
         age = list(type = "factor", start = 4, width = 5),
         car.age = list(type = "factor", start = 9, width = 3),
         type = list(type = "factor", start = 12, width = 1),
         cost = list(type = "numeric", start = 13, width = 6),
         number = list(type = "integer", start = 19, width = 3))
  claimsFFSource <- RxTextData(claimsFFFileName, colInfo = claimsFFColInfo)
  
  # Import the data into a data frame
  claimsIn <- rxImport(inData = claimsFFSource)
  rxGetInfo( claimsIn, getVarInfo = TRUE)
  
  ##############################################################################
  # Import a space delimited text file without variable names
  # Specify names for the data, and use "." for missing values
  # Perform additional transformations when importing
  ##############################################################################
  
  unitTestDataDir <- rxGetOption("unitTestDataDir")
  gardenDAT <- file.path(unitTestDataDir, "Garden.dat")
  gardenDS <- RxTextData(file=gardenDAT, firstRowIsColNames = FALSE,
      missingValueString=".", 
      colInfo = list(
          list(index = 1, newName = "Name"),
          list(index = 2, newName = "Tomato"),
          list(index = 3, newName = "Zucchini"),
          list(index = 4, newName = "Peas"),
          list(index = 5, newName = "Grapes")) )
          
  rsrData <- rxImport(inData=gardenDS, 
      transforms = list( 
          Zone = rep(14, .rxNumRows),
          Type = rep("home", .rxNumRows),
          Zucchini = Zucchini * 10,
          Total = Tomato  + Zucchini + Peas + Grapes,
          PerTom = 100*(Tomato/Total))
      )
  rsrData
  
 
```
 
 
 
