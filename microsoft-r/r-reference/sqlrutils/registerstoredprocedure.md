--- 

# required metadata 
title: "registerStoredProcedure function (sqlrutils) | Microsoft Docs" 
description: " registerStoredProcedure: Uses the StoredProcedure object to register the stored procedure with the specified database " 
keywords: "(sqlrutils), registerStoredProcedure" 
author: "dphansen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 




 # registerStoredProcedure: Register a SQL Stored Procedure with a Database 
 ## Description

`registerStoredProcedure`: Uses the StoredProcedure object to register
the stored procedure with the specified database


 ## Usage

```   
  registerStoredProcedure(sqlSP, connectionString = NULL)

```

 ## Arguments



 ### `sqlSP`
 a valid StoredProcedure object 



 ### `connectionString`
 A character string (must be provided if the StoredProcedure object was created without a connection string) 



 ## Value

TRUE on success, FALSE on failure

 ## Examples

 ```

  ## Not run:

 # See ?StoredProcedure for creating the "cleandata" table.

 # train 1 takes a data frame with clean data and outputs a model
 train1 <- function(in_df) {
  in_df[,"DayOfWeek"] <- factor(in_df[,"DayOfWeek"], levels=c("Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"))
  # The model formula
  formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
  # Train the model
  rxSetComputeContext("local")
  mm <- rxLinMod(formula, data=in_df)
  mm <- rxSerializeModel(mm)
  return(list("mm" = mm))
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
                 "Trusted_Connection=Yes;")
 # create the stored procedure object
 sp_df_op <- StoredProcedure("train1", "spTest1", id, out,
                        filePath = ".")
 # register the stored procedure with the database
 registerStoredProcedure(sp_df_op, conStr)
 model <- executeStoredProcedure(sp_df_op, connectionString = conStr)

 # Getting back the model by unserializing it.
 mm <- rxUnserializeModel(model$params$op1)
 ## End(Not run) 
```

