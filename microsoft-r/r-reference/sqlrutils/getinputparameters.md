--- 
 
# required metadata 
title: "Get a List of Input Parameters of a SQL Stored Procedure" 
description: " `getInputParameters`: returns a list of SQL Server parameter objects                     that describe the input parameters associated                     with a stored procedure " 
keywords: "sqlrutils, getInputParameters" 
author: "richcalaway"
ms.author: "richcala" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 
 #`getInputParameters`: Get a List of Input Parameters of a SQL Stored Procedure

 Applies to version 1.0.0 of package sqlrutils.
 
 ##Description
 
`getInputParameters`: returns a list of SQL Server parameter objects
that describe the input parameters associated
with a stored procedure
 
 
 ##Usage

```   
  getInputParameters(sqlSP)
 
```
 
 ##Arguments

   
  
 ### `sqlSP`
 A valid StoredProcedure Object 
  
 
 
 ##Value
 
A named list of SQL Server parameter objects (InputData, InputParameter)
associated with the provided StoredProcedure object. The names are the names
of the variables from the R function provided into StoredProcedure associated
with the objects
 
 ##Examples

 ```
   
  ## Not run:
 
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
  conStr <- paste0("Driver={SQL Server};Server=.;Database=RevoTestDB;",
                   "Trusted_Connection=TRUE;")
  # create the stored procedure object
  sp_df_op <- StoredProcedure("train1", "spTest1", id, out,
                         filePath = ".")
  # register the stored procedure with the database
  registerStoredProcedure(sp_df_op, conStr)
 ## End(Not run) 
  
 
```
 
 
