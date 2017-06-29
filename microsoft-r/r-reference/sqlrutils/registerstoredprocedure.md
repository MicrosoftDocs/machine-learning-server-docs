--- 
 
# required metadata 
title: "Register a SQL Stored Procedure with a Database" 
description: " `registerStoredProcedure`: Uses the the StoredProcedure object to register the stored procedure with the specified database " 
keywords: "sqlrutils, registerStoredProcedure" 
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
 
 
 
 
 #`registerStoredProcedure`: Register a SQL Stored Procedure with a Database

 Applies to version 1.0.0 of package sqlrutils.
 
 ##Description
 
`registerStoredProcedure`: Uses the the StoredProcedure object to register
the stored procedure with the specified database
 
 
 ##Usage

```   
  registerStoredProcedure(sqlSP, connectionString = NULL)
 
```
 
 ##Arguments

   
  
 ### `sqlSP`
 a valid StoredProcedure object 
  
  
  
 ### `connectionString`
 A character string (must be provided if the StoredProcedure object was created without a connection string) 
  
 
 
 ##Value
 
TRUE on success, FALSE on failure
 
 ##Examples

 ```
   
  ## Not run:
 
 ############# Example 1 #############
 # etl1 - reads from and write directly to the database
 etl1 <- function() {
   # The query to get the data
   qq <- "select top 10000 ArrDelay,CRSDepTime,DayOfWeek from AirlineDemoSmall"
   # The connection string
   conStr <- paste("Driver={ODBC Driver 13 for SQL Server};Server=.;Database=RevoTestDB;",
                  "Trusted_Connection=Yes;", sep = "")
   # The data source - retrieves the data from the database
   dsSqls <- RxSqlServerData(sqlQuery=qq, connectionString=conStr)
   # The destination data source
   dsSqls2 <- RxSqlServerData(table ="cleanData",  connectionString = conStr)
   # A transformation function
   transformFunc <- function(data) {
     data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
     return(data)
   }
   # The transformation variables
   transformVars <- c("CRSDepTime")
   rxDataStep(inData = dsSqls,
             outFile = dsSqls2,
             transformFunc=transformFunc,
             transformVars=transformVars,
             overwrite = TRUE)
   return(NULL)
 }
 # Create a StoredProcedure object
 sp_ds_ds <- StoredProcedure(etl1, "spTest",
                       filePath = ".", dbName ="RevoTestDB")
 # Define a connection string
 conStr <- paste0("Driver={ODBC Driver 13 for SQL Server};Server=.;",
                 "Database=RevoTestDB;Trusted_Connection=Yes;")
 # register the stored procedure with a database
 registerStoredProcedure(sp_ds_ds, conStr)
 # execute the stored procedure
 executeStoredProcedure(sp_ds_ds, connectionString = conStr)


 ############# Example 2 #############
 # train 1 takes a data frame with clean data and outputs a model
 train1 <- function(in_df) {
  in_df[,"DayOfWeek"] <- factor(in_df[,"DayOfWeek"], levels=c("Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"))
  # The model formula
  formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
  # Train the model
  rxSetComputeContext("local")
  mm <- rxLinMod(formula, data=in_df, transformFunc=NULL, transformVars=NULL)
  mm <- memCompress(serialize(mm, connection = NULL), type="gzip")
  return(list(mm = mm))
 }
 # create InpuData Object for an input parameter that is a data frame
 # note: if the input parameter is not a data frame use InputParameter object
 id <- InputData(name = "in_df",
               defaultQuery = paste0("select top 10000 ArrDelay,CRSDepTime,",
                                     "DayOfWeek,CRSDepHour from cleanData"))
 # create an OutputParameter object for the variable inside the return list
 # note: if that variable is a data frame use OutputData object
 out <- OutputParameter("mm", "raw")
 # connections string
 conStr <- paste0("Driver={ODBC Driver 13 for SQL Server};Server=.;Database=RevoTestDB;",
                 "Trusted_Connection=TRUE;")
 # create the stored procedure object
 sp_df_op <- StoredProcedure("train1", "spTest1", id, out,
                        filePath = ".")
 # register the stored procedure with the database
 registerStoredProcedure(sp_df_op, conStr)
 model <- executeStoredProcedure(sp_df_op, id, connectionString = conStr)
 rxReadObject(model$params[[1]])
 ## End(Not run) 
  
 
```
 
 
