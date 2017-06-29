--- 
 
# required metadata 
title: "Assign a Query to the Input Data Parameter of the SQL Stored Procedure" 
description: " `setInputDataQuery`: assigns a query to the InputData parameter of the                  stored procedure that is going to populate the input                  data frame of the embedded R function in the next                  run of the stored procedure. " 
keywords: "sqlrutils, setInputDataQuery" 
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
 
 
 
 
 #`setInputDataQuery`: Assign a Query to the Input Data Parameter of the SQL Stored Procedure

 Applies to version 1.0.0 of package sqlrutils.
 
 ##Description
 
`setInputDataQuery`: assigns a query to the InputData parameter of the
stored procedure that is going to populate the input
data frame of the embedded R function in the next
run of the stored procedure.
 
 
 ##Usage

```   
  setInputDataQuery(inputData, query)
 
```
 
 ##Arguments

   
  
 ### `inputData`
 A character string, the name of the data frame input parameter into the R function. 
  
  
  
 ### `query`
 A character string representing a query. 
  
 
 
 ##Value
 
InputData Object
 
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
  name <- InputParameter("predVarName", "character")
  sp_df_df <- StoredProcedure(score1, "sTest", id, model, name,
                          filePath = ".")
  # register the stored procedure with a database
  registerStoredProcedure(sp_df_df, conStr)
  # assign a different query to the InputData so that it only uses the firt 10 rows
  id <- setInputDataQuery(id, "SELECT top 10 * from cleanData")
  # assign a value to the name parameter
  name <- setInputParameterValue(name, "ArrDelayEstimate")
  # execute the stored procedure
  model <- executeStoredProcedure(sp_df_df, id, name, connectionString = conStr)
  model$data
  model$params[[1]]
 ## End(Not run) 
  
 
```
 
 
