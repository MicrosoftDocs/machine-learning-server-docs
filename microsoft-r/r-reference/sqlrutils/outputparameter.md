--- 
 
# required metadata 
title: "Output Parameter for SQL Stored Procedure: Class Generator" 
description: " `OutputParameter`: generates an OutputParameter Object that captures the information about the output parameters of the function that is to be embedded into a SQL Server Stored Procesure. Those will become the output parameters of the stored procedure. Supported R types of the output parameters are POSIXct, numeric, character, integer, logical, and raw. This object must be created if the R function is returning a named list for non-data frame memebers of the list " 
keywords: "sqlrutils, OutputParameter" 
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
 
 
 
 
 #`OutputParameter`: Output Parameter for SQL Stored Procedure: Class Generator

 Applies to version 1.0.0 of package sqlrutils.
 
 ##Description
 
`OutputParameter`: generates an OutputParameter Object that captures the
information about the output parameters of the function that is
to be embedded into a SQL Server Stored Procesure. Those will become
the output parameters of the stored procedure. Supported R types of the output
parameters are POSIXct, numeric, character, integer, logical, and raw.
This object must be created if the R function is returning a named
list for non-data frame memebers of the list
 
 
 ##Usage

```   
  OutputParameter(name, type)
 
```
 
 ##Arguments

   
  
 ### `name`
 A character string, the name of the output parameter object. 
  
  
  
 ### `type`
 R type of the output parameter object. 
  
 
 
 ##Value
 
OutputParameter Object
 
 ##Examples

 ```
   
  ## Not run:
 
  # train 2 takes a data frame with clean data and outputs a model
  # as well as the data on the basis of which the model was built
  train2 <- function(in_df) {
  in_df[,"DayOfWeek"] <- factor(in_df[,"DayOfWeek"], levels=c("Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"))
    # The model formula
    formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
    # Train the model
    rxSetComputeContext("local")
    mm <- rxLinMod(formula, data=in_df, transformFunc=NULL, transformVars=NULL)
    mm <- memCompress(serialize(mm, connection = NULL), type="gzip")
    return(list(mm = mm, in_df = in_df))
  }
  # create InpuData Object for an input parameter that is a data frame
  # note: if the input parameter is not a data frame use InputParameter object
  id <- InputData(name = "in_df",
                  defaultQuery = paste0("select top 10000 ArrDelay,CRSDepTime,",
                                        "DayOfWeek,CRSDepHour from cleanData"))
  out1 <- OutputData("in_df")
  # create an OutputParameter object for the variable "mm" inside the return list
  out2 <- OutputParameter("mm", "raw")
  # connections string
  conStr <- paste0("Driver={SQL Server};Server=.;Database=RevoTestDB;",
                   "Trusted_Connection=TRUE;")
  # create the stored procedure object
  sp_df_op <- StoredProcedure(train2, "spTest2", id, out1, out2,
                          filePath = ".")
 ## End(Not run) 
  
 
```
 
 
