--- 
 
# required metadata 
title: "Generate Hive, Parquet or ORC Data Source Object" 
description: " These are constructors for Hive, Parquet and ORC data sources which extend `RxDataSource`. These three data sources can be used only in `RxSpark` compute context. " 
keywords: "RevoScaleR, RxHiveData, RxParquetData, RxOrcData {RevoScaleR}, RxHiveData, RxParquetData, RxOrcData" 
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
 
 
 
 
 #`RxHiveData, RxParquetData, RxOrcData {RevoScaleR}`: Generate Hive, Parquet or ORC Data Source Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
These are constructors for Hive, Parquet and ORC data sources
which extend `RxDataSource`. These three data sources
can be used only in `RxSpark` compute context.
 
 
 ##Usage

```   
  RxHiveData(query = NULL, table = NULL, colInfo = NULL, saveAsTempTable = FALSE, cache = FALSE, writeFactorsAsIndexes = FALSE)
  
  RxParquetData(file, colInfo = NULL, fileSystem = "hdfs", cache = FALSE, writeFactorsAsIndexes = FALSE)
  
  RxOrcData(file, colInfo = NULL, fileSystem = "hdfs", cache = FALSE, writeFactorsAsIndexes = FALSE)
 
```
 
 
 ##Arguments

   
    
 ### `query`
 character string specifying a Hive query, e.g. `"select * from sample_table"`. Cannot be used with 'table'. 
  
  
    
 ### `table`
 character string specifying the name of a Hive table, e.g. `"sample_table"`. Cannot be used with 'query'. 
  
  
    
 ### `colInfo`
 list of named variable information lists. Each variable information list contains one or more of the named elements given below (see [rxCreateColInfo](../../r-reference/revoscaler/rxcreatecolinfo.md) for more details):  
*   Currently available properties for a column information list are:  
   *  `type` character string specifying the data type for the column. Supported types are:  
      *   `"logical"` (stored as `uchar`) 
      *   `"integer"` (stored as `int32`) 
      *   `"int16"` (alternative to integer for smaller storage space) 
      *   `"float32"` (stored as `FloatType`) 
      *   `"numeric"` (stored as `float64`) 
      *   `"character"` (stored as `string`) 
      *   `"factor"` (stored as `uint32`) 
      *   `"Date"` (stored as `Date`, i.e. `float64`) 
      *   `"POSIXct"` (stored as `POSIXct`, i.e. `float64`) 
  
   *  `levels` character vector containing the levels when `type = "factor"`.  If the `"levels"` property is not provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value.
 
  
  
  
    
 ### `saveAsTempTable`
 logical. Only applicable when using as output with `table` parameter. If `TRUE` register a temporary Hive table in   Spark memory system otherwise generate a persistent Hive table. The temporary Hive table is always cached in Spark memory system. 
  
  
    
 ### `file`
 character string specifying a file path, e.g. `"/tmp/AirlineDemoSmall.parquet"` or `"/tmp/AirlineDemoSmall.orc"`. 
  
  
    
 ### `fileSystem`
 character string `"hdfs"` or `RxFileSystem` object indicating type of file system. It supports native HDFS and other HDFS compatible systems, e.g., Azure Blob and Azure Data Lake. Local file system is not supported. 
  
  
    
 ### `cache`
 [Deprecated] logical. If `TRUE` data will be cached in the Spark application's memory system after the first use. 
  
  
    
 ### `writeFactorsAsIndexes`
 logical. If `TRUE`, when writing to an output data source, underlying factor indexes will be written instead of the string representations. 
  
 
 
 
 ##Value
 
object of RxHiveData, RxParquetData or RxOrcData.
 
 
 ##Examples

 ```
   
  ## Not run:
 
myHadoopCluster <- rxSparkConnect()

colInfo = list(DayOfWeek = list(type = "factor"))

### import from a parquet file
ds1 <- RxParquetData(file = "/tmp/AirlineDemoSmall.parquet",
  colInfo = colInfo)
rxImport(ds1)  
  
### import from an orc file
ds2 <- RxOrcData(file = "/tmp/AirlineDemoSmall.orc",
  colInfo = colInfo)  
rxImport(ds2)    

### import from a Hive query
ds3 <- RxHiveData(query = "select * from hivesampletable")
rxImport(ds3)  

### import from a Hive persistent table
ds4 <- RxHiveData(table = "AirlineDemo")
rxImport(ds4)  

### output to a orc file
out1 <- RxOrcData(file = "/tmp/AirlineDemoSmall.orc",
  colInfo = colInfo)
rxDataStep(inData = ds1, outFile=out1, overwrite=TRUE)  

### output to a Hive temporary table
out2 <- RxHiveData(table = "AirlineDemoSmall", saveAsTempTable=TRUE)
rxDataStep(inData = ds1, outFile=out2)
 ## End(Not run) 
  
 
```
 
