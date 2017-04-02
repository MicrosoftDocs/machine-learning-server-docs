--- 
 
# required metadata 
title: " Execute SQL Command for Data Manipulation, Definition, or Control " 
description: " Execute a command to define, manipulate, or control SQL data (but not  return data). " 
keywords: "RevoScaleR, rxExecuteSQLDDL, rxExecuteSQLDDL,RxDataSource-method,  ~kwd1 ,  ~kwd2 " 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 #`rxExecuteSQLDDL`:  Execute SQL Command for Data Manipulation, Definition, or Control 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Execute a command to define, manipulate, or control SQL data (but not 
return data).
 
 
 ##Usage

```   
  rxExecuteSQLDDL(src, ...)
 
```
 
 
 ##Arguments

   
    
 ### `src`
  An RxOdbcData data source.  
  
    
 ### ` ...`
  Additional arguments, typically `sSQLString=` supplied with a character string specifying the SQL command to be executed. The typical SQL commands are `CREATE TABLE` and `DROP TABLE`.  
  
 
 
 
 ##Value
 
Returns `NULL`; executed for its side-effect (the manipulation of the
SQL data source).
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 
 ##See Also
 
[RxOdbcData](RxOdbcData.md)
   
 ##Examples

 ```
   
  ## Not run:
 

# Note: for improved security, read connection string from a file, such as
# conString <- readLines("conString.txt")

conString <- "Driver=Teradata;DBCNAME=myDb;Database=test;Uid=tester;pwd=pwd;"
outOdbcDS <- RxOdbcData(table = "MyClaims",           
                        connectionString = conString,
                        useFastRead=TRUE)                           
rxOpen(outOdbcDS, "w")                       
rxExecuteSQLDDL(outOdbcDS, sSQLString = paste("CREATE TABLE [MyClaims]([RowNum] [int] NULL, [age] [char](5) NULL, ",
        "[car_age] [char](3) NULL, [type] [char](1) NULL, ", " [cost] [float] NULL, [number] [float] NULL);", sep=""))
inTextData <- RxTextData(file = file.path(rxGetOption("sampleDataDir"), "claims.txt"),
                        stringsAsFactors = TRUE, useFastRead = TRUE)
outOdbcDS <- RxOdbcData(table = "MyClaims",           
                        connectionString = conString,
                        useFastRead=TRUE)                           
rxDataStep(inData = inTextData, outFile = outOdbcDS)   
   ## End(Not run) 
            
 
```
 
 
 
 
 
