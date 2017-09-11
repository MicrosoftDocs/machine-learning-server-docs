--- 
 
# required metadata 
title: "Execute a SQL Stored Procedure" 
description: " `executeStoredProcedure`: Executes a stored procedure registered with the database " 
keywords: "sqlrutils, executeStoredProcedure" 
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
 
 
 
 
 #executeStoredProcedure: Execute a SQL Stored Procedure

 Applies to version 1.0.0 of package sqlrutils.
 
 ##Description
 
`executeStoredProcedure`: Executes a stored procedure registered with the database
 
 
 ##Usage

```   
  executeStoredProcedure(sqlSP, ..., connectionString = NULL)
 
```
 
 ##Arguments

   
  
 ### sqlSP
 a valid StoredProcedure Object 
  
  
  
 ###  ...
 Optional input and output parameters for the stored procedure. All of the parameters that do not have default queries or values assigned to them must be provided 
  
  
  
 ### connectionString
 A character string (must be provided if the StoredProcedure object was created without a connection string). This function requires using an ODBC driver which supports ODBC 3.8 functionality. 
  
  
  
 ### verbose
 Boolean. Whether to print out the command used to execute the stored procedure 
  
 
 
 ##Value
 
TRUE on success, FALSE on failure
 
 ##Note
 
This function relies that the ODBC driver used supports ODBC 3.8 features.
Otherwise it will fail.
 
 
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
  # score1 makes a batch prediction given clean data(indata),
  # model object(model_param), and the new name of the variable
  # that is being predicted
score1 <- function(in_df, model_param, predVarNameInParam) {
    factorLevels <- c("Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday")
    in_df[,"DayOfWeek"] <- factor(in_df[,"DayOfWeek"], levels=factorLevels)
    mm <- rxReadObject(as.raw(model_param))
    # Predict
    result <- rxPredict(modelObject = mm,
                        data = in_df,
                        outData = NULL,
                        predVarNames = predVarNameInParam,
                        extraVarsToWrite = c("ArrDelay"),
                        writeModelVars = TRUE,
                        overwrite = TRUE)
    return(list(result = result, pvOutParam = mm$f.pvalue))
}

# create  an InputData object for the input data frame in_df
indata <- InputData(name = "in_df", defaultQuery = "SELECT top 10 * from cleanData")
# create InputParameter objects for model_param and predVarNameInParam
model <- InputParameter("model_param", "raw",
                        defaultQuery = paste("select top 1 value from rdata",
                                             "where [key] = 'linmod.v1'"))
predVarNameInParam <- InputParameter("predVarNameInParam", "character")
# create OutputData object for the data frame inside the return list
outData <- OutputData("result")
# create OutputParameter object for non data frame variable inside the return list
pvOutParam <- OutputParameter("pvOutParam", "numeric")
scoreSP1 <- StoredProcedure(score1, "spScore_df_param_df", indata, model, predVarNameInParam, outData, pvOutParam,
                            filePath = path)
conStr <- "Driver={ODBC Driver 13 for SQL Server};Server=.;Database=RevoTestDB;Trusted_Connection=Yes;"
# connection string necessary for registrations and execution
# since we did not pass it to StoredProcedure
registerStoredProcedure(scoreSP1, conStr)
model <- executeStoredProcedure(scoreSP1, predVarNameInParam = "ArrDelayEstimate", connectionString = conStr, verbose = TRUE)
model$data
model$params[[1]]
 ## End(Not run) 
  
  
 
```
 
 
