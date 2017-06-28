--- 
 
# required metadata 
title: "Generate Xdf Data Source Object" 
description: " This is the main generator for S4 class RxXdfData, which extends RxDataSource. " 
keywords: "RevoScaleR, RxXdfData, XdfData, head.RxXdfData, summary.RxXdfData, tail.RxXdfData, file, connection" 
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
 
 
 
 
 
 
 #`RxXdfData`: Generate Xdf Data Source Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
This is the main generator for S4 class RxXdfData, which extends RxDataSource.
 
 
 ##Usage

```   
  RxXdfData(file, varsToKeep = NULL, varsToDrop = NULL, returnDataFrame = TRUE,
          stringsAsFactors = FALSE, blocksPerRead = rxGetOption("blocksPerRead"),
          fileSystem = NULL, createCompositeSet = NULL, createPartitionSet = NULL,
          blocksPerCompositeFile = 3) 
  
 ## S3 method for class `RxXdfData':
head  (x, n = 6L, reportProgress = 0L, ...)
  
 ## S3 method for class `RxXdfData':
summary  (object, ...)
  
 ## S3 method for class `RxXdfData':
tail  (x, n = 6L, addrownums = TRUE, reportProgress = 0L, ...)
  	      
 
```
 
 
 ##Arguments

   
    
 ### `file`
 character string specifying the location of the data. For single Xdf, it is a .xdf file.  For composite Xdf, it is a directory like /tmp/airline. When using distributed compute contexts like `RxSpark`, a directory should be used since those compute contexts always use composite Xdf. 
  
  
    
 ### `varsToKeep`
 character vector of variable names to keep around during operations. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to drop from operations. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `returnDataFrame`
 logical indicating whether or not to convert the result to a data frame when reading with `rxReadNext`. If `FALSE`, a list is returned when reading with `rxReadNext`. 
  
  
    
 ### `stringsAsFactors`
 logical indicating whether or not to convert strings into factors in R (for reader mode only). 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
     
 ### `fileSystem`
 character string or [RxFileSystem](../../r-reference/revoscaler/rxfilesystem.md) object indicating type of file system;  `"native"`or `RxNativeFileSystem` object can be used for the local operating system, or an `RxHdfsFileSystem` object for the Hadoop file system. If `NULL`, the file system will be set to that in the current compute context, if available, otherwise the `fileSystem` option. 
  
  
     
 ### `createCompositeSet`
 logical value or `NULL`. Used only when writing.  If `TRUE`, a composite set of files will be created instead of a single .xdf file.  Subdirectories data and metadata will be created. In the data subdirectory, the data will be split across a set of .xdfd files (see `blocksPerCompositeFile` below for determining how many blocks of data will be in each file). In the metadata subdirectory  there is a single .xdfm file, which contains the meta data for all of the  .xdfd files in the  data subdirectory. When the compute context is `RxHadoopMR` or `RxSpark`, a composite  set of files are always created. 
  
  
     
 ### `createPartitionSet`
 logical value or `NULL`. Used only when writing.  If `TRUE`, a set of files for partitioned Xdf will be created when assigning this RxXdfData object for `outData` of rxPartition. Subdirectories data and metadata will be created. In the data subdirectory, the data will be split across a set of .xdf files (each file stores data of a single data partition, see rxPartition for details). In the metadata subdirectory there is a single .xdfp file, which contains the meta data for all of the  .xdf files in the  data subdirectory. The partitioned Xdf object is currently supported only in rxPartition and [rxGetPartitions](../../r-reference/revoscaler/rxgetpartitions.md) 
  
   
     
 ### `blocksPerCompositeFile`
 integer value. If `createCompositeSet=TRUE`, and if the compute context is not `RxHadoopMR`, this will be the number of blocks put into each .xdfd file in the composite set. When importing is being done on Hadoop using MapReduce, the number of rows per .xdfd file is determined by the rows assigned to each MapReduce task, and the number of blocks per .xdfd file is therefore determined by `rowsPerRead`. 
   
   
     
 ### `x`
 an `RxXdfData` object 
  
   
     
 ### `object`
 an `RxXdfData` object 
  
   
     
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
  
   
 
 
 ##Value
 
object of class RxXdfData.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxXdfData-class](RxXdfData-class.md),
[rxNewDataSource](../../r-reference/revoscaler/rxnew.md),
[rxOpen](rxOpen-methods.md),
[rxReadNext](rxOpen-methods.md).
   
 ##Examples

 ```
   
  myDataSource <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "claims"))
  # both of these should return TRUE
  is(myDataSource, "RxXdfData")
  is(myDataSource, "RxDataSource")
  
  names(myDataSource)
  
  modelFormula <- formula(myDataSource, depVars = "cost", varsToDrop = "RowNum")
  
 
```
 
 
 
