--- 
 
# required metadata 
title: "Output Data for SQL Stored Procedure: Class Generator" 
description: " `OutputData`: generates an OutputData Object, that captures the information about the data frame that needs to be returned after the execution of the R function embedded into the stored procedure. This object must be created if the R function is returning a named list, where one of the items in the list is a data frame. The return list can contain at most one data frame. " 
keywords: "sqlrutils, OutputData" 
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
 
 
 
 
 #OutputData: Output Data for SQL Stored Procedure: Class Generator

 Applies to version 1.0.0 of package sqlrutils.
 
 ##Description
 
`OutputData`: generates an OutputData Object, that captures the
information about the data frame that needs to be returned after
the execution of the R function embedded into the stored procedure.
This object must be created if the R function is returning a named
list, where one of the items in the list is a data frame. The return
list can contain at most one data frame.
 
 
 ##Usage

```   
  OutputData(name)
 
```
 
 ##Arguments

   
  
 ### name
 A character string, the name of the data frame variable. 
  
 
 
 ##Value
 
OutputData Object
 
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
 
 
