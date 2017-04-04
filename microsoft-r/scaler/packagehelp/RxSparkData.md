--- 
 
# required metadata 
title: "Generate Hive or Parquet Data Source object" 
description: " These are constructor for HIVE and Parquet data sources which extend `RxDataSource`. These two data sources can be used only in a `RxSpark` compute context. " 
keywords: "RevoScaleR, RxSparkData, RxHiveData, RxParquetData" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 
 #`RxSparkData`: Generate Hive or Parquet Data Source object

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
These are constructor for HIVE and Parquet data sources
which extend `RxDataSource`. These two data sources
can be used only in a `RxSpark` compute context.
 
 
 ##Usage

```   
  RxHiveData( query = NULL, colInfo = NULL, cache = TRUE)
  
  RxParquetData(file, colInfo = NULL, fileSystem = "hdfs", cache = TRUE)
 
```
 
 
 
 ##Arguments

   
    
 ### `query`
 character string specifying a HIVE query, e.g. `"select * from sample_table"` 
  
  
    
 ### `colInfo`
 list of named variable information lists. Each variable information list contains one or more of the named elements given below (see [rxCreateColInfo](rxCreateColInfo.md) for more details):  
*   Currently available properties for a column information list are: `type`character string specifying the data type for the column. If the `type` is not specified, it will be read as `character` data. Supported types are:  
   *   `"logical"` (stored as `uchar`), 
   *   `"integer"` (stored as `int32`), 
   *   `"int16"` (alternative to integer for smaller storage space), 
   *   `"float32"` (stored as `FloatType` in spark data frame), 
   *   `"numeric"` (stored as ‘float64’ as in R), 
   *   `"character"` (stored as `string`), 
   *   `"factor"` (stored as `uint32`) 
  `levels`character vector containing the levels when `type = "factor"`.  If the `"levels"` property is no provided, factor levels will be determined by the values in the source column. If levels are provided, any value that does not match a provided level will be converted to a missing value.
  
  
  
    
 ### `file`
 character string specifying a Parquet file path, e.g. `"/tmp/AirlineDemoSmall.parquet"` 
  
  
    
 ### `fileSystem`
 character string or `RxFileSystem` object indicating type of file system. It supports native HDFS and other  HDFS compatible systems, e.g. Azure Blob and Azure Data Lake. Local  file system is not supported. 
  
  
    
 ### `cache`
 if `TRUE` HIVE/Parquet data will be cached in the Spark application's memory after the first run. 
  
 
 
 
 ##Value
 
object of class RxHiveData or RxParquetData.
 
 
 ##Examples

 ```
   
  ## Not run:
 
myHadoopCluster <- rxSparkConnect(consoleOutput = TRUE, executorCores = 2,
  executorMem = "1g", driverMem = "1g", executorOverheadMem = "1g",
  numExecutors = 2)

### import from a parquet cource
colInfo = list( DayOfWeek = list(type = "factor"))
df <- RxParquetData(parquetPath = "/tmp/AirlineDemoSmall.parquet",
  colInfo = colInfo, cache = TRUE)

### import from a hive query
df2 <- RxHiveData(query = "select * from hivesampletable")

 ## End(Not run) 
  
 
```
 
