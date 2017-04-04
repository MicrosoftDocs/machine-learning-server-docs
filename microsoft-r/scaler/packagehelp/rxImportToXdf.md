--- 
 
# required metadata 
title: "Import a Data Source to .xdf" 
description: " Write data source content to an .xdf file. " 
keywords: "RevoScaleR, ScaleR, KEYWORDS" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "02/17/2017" 
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
 
 
 #Import a Data Source to .xdf 
 ##Description
 
Write data source content to an .xdf file.
 
 
 ##Usage

```   
  rxImportToXdf(inSource, outSource, rowSelection = NULL, 
  		transforms = NULL, transformObjects = NULL,
  		transformFunc = NULL, transformVars = NULL, 
  		transformPackages = NULL, transformEnvir = NULL,
  		append = "none", overwrite = FALSE, numRows = -1,
  		maxRowsByCols = NULL,
  		reportProgress = rxGetOption("reportProgress"),
  		verbose = 0, 
  		xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
  		createCompositeSet = NULL,
  		blocksPerCompositeFile = 3
  		)
 
```
 
 ##Arguments

   
    
 >  `inSource` -  a non-RxXdfData RxDataSource object representing the input data source or a non-empty character string representing a file path. If a character string is supplied, the type of file is inferred from its extension, with the default being a text file.  
  
    
 >  `outSource` -  an RxXdfData object or non-empty character string representing the output .xdf file.  
  
    
 >  `rowSelection` -  name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function.  
  
    
 >  `transforms` -  an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function.  
  
    
 >  `transformObjects` -  a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`.  
    
 >  `transformFunc` -  variable transformation function. See [rxTransform](rxTransform.md) for details.  
  
    
 >  `transformVars` -  character vector of input data set variables needed for the transformation function. See [rxTransform](rxTransform.md) for details.  
  
    
 >  `transformPackages` -  character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded.  
  
    
 >  `transformEnvir` -  user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead.  
  
    
 >  `append` -  either `"none"` to create a new .xdf file or `"rows"` to append rows to an existing .xdf file. If `outSource` exists and `append` is `"none"`, the `overwrite` argument must be set to `TRUE`.  
  
    
 >  `overwrite` -  logical value. If `TRUE`, the existing `outSource` will be overwritten.  
  
    
 >  `numRows` -  integer value specifying the maximum number of rows to import. If set to -1, all rows will be imported.  
  
    
 >  `maxRowsByCols` -  the maximum size of a data frame that will be read in if `outData` is set to `NULL`, measured by the number of rows times the number of columns. If the number of rows times the number of columns being imported exceeds this,  a warning will be reported and a smaller number of rows will be read in than requested. If `maxRowsByCols` is set to be too large, you may experience problems  from loading a huge data frame into memory.  
  
    
 >  `reportProgress` -  integer value with options:  
*  `0`: no progress is reported. 
*  `1`: the number of processed rows is printed and updated. 
*  `2`: rows processed and timings are reported. 
*  `3`: rows processed and all timings are reported. 
   
  
    
 >  `verbose` -  integer value. If `0`, no additional output is printed.  If `1`, information on the import type is printed.   
  
     
 >  `xdfCompressionLevel` -  integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used.  
  
     
 >  `createCompositeSet` -  logical value or `NULL`. If `TRUE`, a composite set of files will be created instead of a single .xdf file. A directory will be created whose name is the same as the .xdf file that would otherwise be created, but with no extension. Subdirectories data and metadata will be created. In the data subdirectory, the data will be split across a set of .xdfd files (see `blocksPerCompositeFile` below for determining how many blocks of data will be in each file). In the metadata subdirectory  there is a single .xdfm file, which contains the meta data for all of the  .xdfd files in the  data subdirectory. When the compute context is `RxHadoopMR` a composite set of files is always created.  
  
    
 >  `blocksPerCompositeFile` -  integer value. If `createCompositeSet=TRUE`, and if the compute context is not `RxHadoopMR`, this will be the number of blocks put into each .xdfd file in the composite set. When importing is being done on Hadoop using MapReduce, the number of rows per .xdfd file is determined by the rows assigned to each MapReduce task, and the number of blocks per .xdfd file is therefore determined by `rowsPerRead`. If the `outSource` is an [RxXdfData](RxXdfData.md) object, set the value for `blocksPerCompositeFile` there instead.   
  
 
 
 
 ##Details
 
The input and output data sources are automatically opened and closed within
the `rxImportToXdf` function. 

**Encoding Details**

If the input data source or file contains non-ASCII character information, please use 
the function `rxImport` with the encoding settings recommended in the RxImport documentation 
under 'Encoding Details'
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](http://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxDataSource-class](RxDataSource-class.md),
[rxImport](rxImport.md),
[rxDataStep](rxDataStep.md),
[RxTextData](RxTextData.md),
[RxSasData](RxSasData.md),
[RxSpssData](RxSpssData.md),
[RxXdfData](RxXdfData.md),
[rxTransform](rxTransform.md).
   
 ##Examples

 ```
   
  # Create a SAS data source
  claimsSasFileName <- file.path(rxGetOption("sampleDataDir"), "claims.sas7bdat")
  claimsSasSource <- RxSasData(file = claimsSasFileName)
  
  # Create an xdf data source
  claimsXdfFileName <- file.path(tempdir(), "importedClaims.xdf")
  claimsXdfSource <- RxXdfData(file = claimsXdfFileName)
  
  # Import the data into the xdf file
  rxImportToXdf(inSource = claimsSasSource, outSource = claimsXdfSource, overwrite = TRUE)
  
  # Read xdf file into a data frame
  claimsIn <- rxDataStep(inData = claimsXdfFileName)
  head(claimsIn)
  
  # Clean-up: delete the new file
  file.remove( claimsXdfFileName)
  
  # Create SPSS data source
  spssFileName <- file.path(rxGetOption("unitTestDataDir"), "claimsInt.sav")
  spssDS <- RxSpssData(file = spssFileName)
  
  # Creates XDF data source
  xdfFileName <- file.path(tempdir(), "claimsInt.xdf")
  xdfDS <- RxXdfData(file = xdfFileName)
  
  # Import data; notice that factor variables are created for any
  # SPSS variables with value labels
  
  rxImportToXdf(inSource = spssDS, outSource = xdfDS, overwrite = TRUE, reportProgress = 0)
  varInfo <- rxGetVarInfo( data = xdfFileName )
  varInfo
  
  # Reimport SPSS data, storing some variables as integer instead of
  # numeric
  
  spssColClasses <- c(RowNum = "integer", cost = "integer", 
    number = "integer")
  spssDS <- RxSpssData(file = spssFileName, colClasses = spssColClasses)
  rxImportToXdf(inSource = spssDS, outSource = xdfDS, overwrite = TRUE, reportProgress = 0)
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
  spssDS <- RxSpssData(file = spssFileName, colInfo = spssColInfo)
  rxImportToXdf(inSource = spssDS, outSource = xdfDS, overwrite = TRUE, reportProgress = 0)
  varInfo <- rxGetVarInfo( data = xdfFileName )
  varInfo  
  
  # Clean-up: delete the new file
  file.remove( xdfFileName)
  
 
```
 
 
 
 
 
 
 
 
 
