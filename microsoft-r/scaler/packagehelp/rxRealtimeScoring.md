--- 
 
# required metadata 
title: "Real-time scoring in SQL Server R Services" 
description: " Real-time scoring brings the `rxPredict` functionality available in **RevoScaleR** and **MicrosoftML** packages to  Microsoft R Server and SQL Server platforms with near real-time performance.  You can take advantage of this functionality by upgrading your in-database R Services to Microsoft R Server 9.1 using the information in the following [`link`](https://docs.microsoft.com/en-us/sql/advanced-analytics/r-services/use-sqlbindr-exe-to-upgrade-an-instance-of-r-services) .  **NOTE:** This document contains information regarding **Real-time scoring in SQL Server R Services**. For information regarding **Real-time scoring in R Server**, please refer to the [`publishService`](https://msdn.microsoft.com/en-us/microsoft-r/operationalize/data-scientist-manage-services#publishservice)  documentation. " 
keywords: "RevoScaleR, rxRealTimeScoring, rxRTS, realtime, realtimescoring, rts, rxPredict" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 #`rxRealTimeScoring`: Real-time scoring in SQL Server R Services

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Real-time scoring brings the `rxPredict` functionality available in **RevoScaleR** and **MicrosoftML** packages to  Microsoft R Server and SQL Server platforms with near real-time performance.

You can take advantage of this functionality by upgrading your in-database R Services to Microsoft R Server 9.1 using the information in the following [`link`](https://docs.microsoft.com/en-us/sql/advanced-analytics/r-services/use-sqlbindr-exe-to-upgrade-an-instance-of-r-services)
.

**NOTE:** This document contains information regarding **Real-time scoring in SQL Server R Services**. For information regarding **Real-time scoring in R Server**, please refer to the [`publishService`](https://msdn.microsoft.com/en-us/microsoft-r/operationalize/data-scientist-manage-services#publishservice)
 documentation.
 
 
 ##Details
 
With Microsoft R Server 9.1 version, we are introducing this feature which allows models trained using **RevoScaleR** and **MicrosoftML** packages to be used for high performance scoring in SQL Server. These models can be published and scored without using R interpreter, which reduces the overhead of multiple process interactions. Hence it allows for faster prediction performance in enterprise scenarios.

The following is the list of models that are currently supported in real-time scoring:


* 
 **RevoScaleR**


   * 
 rxLogit

   * 
 rxLinMod

   * 
 rxBTrees

   * 
 rxDTree

   * 
 rxDForest




* 
**MicrosoftML**


   * 
 rxFastTrees

   * 
 rxFastForest

   * 
 rxLogisticRegression

   * 
 rxOneClassSvm

   * 
 rxNeuralNet

   * 
 rxFastLinear






**Workflow for Real-time scoring in SQL Server**

The high-level workflow for real-time scoring in SQL Server is as follows:



1 
 Serialize model for real-time scoring

1 
 Publish model to SQL Server

1 
 Enable real-time scoring functionality in SQL Server R Services using RegisterRExt tool

1 
 Call real-time scoring stored procedure using T-SQL



The following sections describe each of these steps in details. These steps are illustrated with a sample in the Example section below.

**1. Serialize model for Real-time scoring**

To enable this functionality in SQL Server, we are adding support for serializing the model trained in R in a specific `raw` format that allows the model to be scored in an efficient manner. The serialized model can then be stored in a SQL Server database table for doing real-time scoring. The following new APIs are introduced to allow this serialization functionality



* 
 [rxSerializeModel](rxSerializeModel.md)() - Serialize a **RevoScaleR**/**MicrosoftML** model in `raw` format to enable saving the model to a database. This enables the model to be loaded into SQL Server for real-time scoring.

* 
 [rxUnserializeModel](rxSerializeModel.md)() - Retrieve the original R model object from the serialized raw model.



**2. Publish model to SQL Server**

The serialized models can be published to the target SQL Server Database table in `VARBINARY` format for real-time scoring. You can use the existing `rxWriteObject` API to publish the model to SQL Server. Alternatively, you can also save the `raw` type to a file and load into SQL Server.



* 
 [rxWriteObject](rxWriteObject.md)() - Store/retrieve R objects to/from ODBC data sources like SQL Server. The API is modeled after a simple key value store.



**NOTE:** Since the model is already serialized via `rxSerializeModel` there is no need to additionally serialize in `rxWriteObject`, so the `serialize` flag need to set to `FALSE`.



**3. Enable Real-time scoring functionality in SQL Server R Services**

By default, real-time scoring functionality is disabled on SQL Server R Services and it needs to be enabled on a particular SQL database. To use this functionality, the server administrator needs to do the following:

**`RegisterRExt.exe`** is the command line utility which ships with RevoScaleR package and allows administrators to enable real-time scoring feature in a SQL server database. You can find RegisterRExt.exe at `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\x64\RegisterRExe.exe`.


To enable SQL Server R Services real-time scoring, open an elevated command prompt and use the following command:


* 
 `RegisterRExt.exe /installRts [/instance:name]  /database:databasename`



The /instance flag is optional if the database is part of the default SQL Server instance. This command will create assemblies and stored procedure on the SQL Server database to allow real-time scoring. Additionally, it creates a database role `rxpredict_users` to help database admins to grant permission to use the real-time scoring functionality.

**NOTE:** On enabling real-time scoring, RegisterRExt will enable  SQL CLR functionality in the instance and the specific database will be marked as `TRUSTWORTHY`. Please carefully consider additional security implications of doing this.

To disable real-time scoring functionality in any database, open an elevated command prompt and use the following command:


* 
 `RegisterRExt.exe /uninstallrts /database:databasename  [/instance:name]`


**4. Call real-time scoring stored procedure using T-SQL**

The functionality once installed, will be available in the particular SQL Server database via a stored procedure called `sp_rxPredict`. Here is the syntax of the `sp_rxPredict` stored procedure

**Syntax**

`sp_rxPredict`

`@model [VARBINARY](max)`,

`@inputData [NVARCHAR](max)`

where


* 
 `@model` - Real-time model to be used for scoring in `VARBINARY` format

* 
 `@inputData` - SQL query to be used to get the input data for scoring



**Limitations**:



1 
Real-time scoring does not use an R interpreter for scoring hence any functionality requiring R interpreter is not supported.  Here are a few unsupported scenarios:


   * 
 Models of `rxGlm` and `rxNaiveBayes` algorithms are not currently supported

   * 
 **RevoScaleR** Models with R transform function or transform based formula (e.g `"A ~ log(B)"`) are not supported in real-time scoring. Such transforms to input data may need to be handled before calling real-time scoring.




1 
Arguments other than `modelObject`/`data` available in `rxPredict` are not supported in real-time scoring



**Known Issues**:


1 
 `sp_rxPredict` with `RevoScaleR` model only supports only following .NET column types are double, float, short, ushort, long, ulong and string. You may need to filter out unsupported types in your input data before using it for real-time scoring. For corresponding SQL type information refer [`SQL-CLR Type Mapping`](https://msdn.microsoft.com/en-us/library/bb386947(v=vs.110).aspx)



1 
  `sp_rxPredict` is optimized for fast predictions on smaller data (from a few rows to a few 1000 rows), the performance may start degrading on larger data sets.

1 
 `sp_rxPredict` returns inaccurate error message when NULL value is passed as model

`System.Data.SqlTypes.SqlNullValueException:`**Data in Null**


 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxSerializeModel](rxSerializeModel.md),
[rxWriteObject](rxWriteObject.md),
publishService,
[rxPredict](../../r-reference/microsoftml/rxpredict.md)
   
 ##Examples

 ```
   
  ## Not run:
 
form <- Sepal.Length ~ Sepal.Width + Species
model <- rxLinMod(form, data = iris)
library(RevoScaleR)
# 1. Serialize model for real-time scoring
serializedModel <- rxSerializeModel(model)

# 2: Publish model to database
server <- ".\SQL2016"
database <- "revotestdb"
modelTable <- "modelTable"

# Create a table for storing the real-time scoring model
connectionString <- paste0("Driver=SQL Server;Server=", server, ";Database=", database, ";Trusted_connection=True")
odbcDS <- RxOdbcData(table = modelTable, connectionString = connectionString)
if (!rxSqlServerTableExists(table = modelTable, connectionString = connectionString))
{
  ddlCreateTable <- paste(" create table [", modelTable, "] (",
                          "     [id] varchar(200) not null, ",
                          "     [value] varbinary(max), ",
                          "     constraint unique_id unique (id))",
                          sep = "")
  rxExecuteSQLDDL(odbcDS, ddlCreateTable)
}

modelId <- "linModModel" # Any key to identify the model
rxWriteObject(dest = odbcDS, key = modelId, value = serializedModel, serialize = FALSE, overwrite=TRUE) # serialize is FALSE since the model is already serialized into raw in Step 1

# 3: Enable real-time scoring functionality using RegiterRExt (see above)

# 4: Call real-time scoring stored procedure (sp_rxPredict) using T-SQL
query <- paste0("
                DECLARE @model VARBINARY(max);
                SELECT @model = value FROM ", modelTable, " WHERE id = '", modelId, "';
                EXEC sp_rxPredict @model = @model, @inputData = '
                SELECT
                CAST(3.1 AS FLOAT) AS [Sepal.Width],
                ''virginica'' AS Species", "'")
  # Use CAST to map numeric types to SQL Float

  # T-SQL Example above uses inline data in the query. Alternatively, a data could come from a table in a SQL database

cat(query) #  TSQL to be executed in SQL Server
#  The above command displays the T-SQL that can be executed in a SQL Server for real-time scoring

# To test this from R you can execute the following test method
rtsResult <- RevoScaleR:::rxExecuteSQL(connectionString = connectionString, query = query)

 ## End(Not run) 
  
 
```
 
 
 
 
 
