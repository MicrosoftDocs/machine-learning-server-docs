--- 
 
# required metadata 
title: "Input Parameter for SQL Stored Procedure: Class Generator" 
description: " `InputParameter`: generates an InputParameter Object, that captures the information about the input parameters of the R function that is to be embedded into a SQL Server Stored Procesure. Those will become the input parameters of the stored procedure. Supported R types of the input parameters are POSIXct, numeric, character, integer, logical, and raw. " 
keywords: "sqlrutils, InputParameter" 
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
 
 
 
 
 #InputParameter: Input Parameter for SQL Stored Procedure: Class Generator

 Applies to version 1.0.0 of package sqlrutils.
 
 ##Description
 
`InputParameter`: generates an InputParameter Object, that captures the
information about the input parameters of the R function that is
to be embedded into a SQL Server Stored Procesure. Those will become
the input parameters of the stored procedure. Supported R types of the input
parameters are POSIXct, numeric, character, integer, logical, and raw.
 
 
 ##Usage

```   
  InputParameter(name, type, defaultValue = NULL, defaultQuery = NULL,
  value = NULL, enableOutput = FALSE)
 
```
 
 ##Arguments

   
  
 ### name
 A character string, the name of the input parameter object. 
  
  
  
 ### type
 A character string representing the R type of the input parameter object. 
  
  
  
 ### defaultValue
 Default value of the parameter. Not supported for "raw". 
  
  
  
 ### defaultQuery
 A character string specifying the default query that will retrieve the data if a different query is not provided at the time of the execution of the stored procedure. 
  
  
  
 ### value
 A value that will be used for the parameter. in the next run of the stored procedure. 
  
  
  
 ### enableOutput
 Make this an Input/Output Parameter 
  
 
 
 ##Value
 
InputParameter Object
 
 ##Examples

 ```
   
  ## Not run:
 
  # score1 makes a batch prediction given clean data(indata),
  # model object(model_param), and the new name of the variable
  # that is being predicted
  score1 <- function(indata, model_param, predVarName) {
  indata[,"DayOfWeek"] <- factor(indata[,"DayOfWeek"], levels=c("Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"))
  # The connection string
  conStr <- paste("Driver={SQL Server};Server=.;Database=RevoTestDB;",
                  "Trusted_Connection=TRUE;", sep = "")
  # The compute context
  computeContext <- RxInSqlServer(numTasks=4, connectionString=conStr)
  mm <- rxReadObject(as.raw(model_param))
  # Predict
  result <- rxPredict(modelObject = mm,
                      data = indata,
                      outData = NULL,
                      predVarNames = predVarName,
                      extraVarsToWrite = c("ArrDelay"),
                      writeModelVars = TRUE,
                      overwrite = TRUE)
}
# connections string
conStr <- paste0("Driver={SQL Server};Server=.;Database=RevoTestDB;",
                 "Trusted_Connection=TRUE;")
# create InpuData Object for an input parameter that is a data frame
id <- InputData(name = "indata", defaultQuery = "SELECT * from cleanData")
# InputParameter for the model_param input variable
model <- InputParameter("model_param", "raw",
                        defaultQuery =
                          "select top 1 value from rdata where [key] = 'linmod.v1'")
# InputParameter for the predVarName variable
name <- InputParameter("predVarName", "character", value = "ArrDelayEstimate")
sp_df_df <- StoredProcedure(score1, "sTest", id, model, name,
                        filePath = ".")
 ## End(Not run) 
  
  
 
```
 
 
