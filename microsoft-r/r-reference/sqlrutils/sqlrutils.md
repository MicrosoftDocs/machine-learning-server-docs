---

# required metadata
title: "sqlrutils Functions R Package for Microsoft R | Microsoft Docs"
description: "A package used for executing stored procedures on SQL Server from R script."
keywords: "sqlrutils package reference"
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "08/22/2017"
ms.topic: "reference"
ms.prod: "microsoft-r"

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

# sqlrutils package for R

The **sqlrutils** package provides a mechanism for R users to put their R scripts into a T-SQL stored procedure, register that stored procedure with a database, and run the stored procedure from an R development environment.

| Package details | |
|--------|-|
| Version: |  1.0.0 |
| Runs on: | [SQL Server 2016 R Services or SQL Server 2017 Machine Learning Services (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services) |
| Built on: | R 3.3.x (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|

## How to use sqlrutils

The **sqlrutils** library is installed as part of SQL Server Machine Learning when you add R to your installation. You get the full collection of proprietary packages plus an R distribution with its base packages and interpreters. You can use any R IDE to write R script calling functions in **sqlrutils**, but the script must run on a computer having SQL Server Machine Learning with R.

The workflow for using this package includes the following steps:

+ Define stored procedure parameters (inputs, outputs, or both) 
+ Generate and register the stored procedure    
+ Execute the stored procedure  

In an R session, load **sqlrutils** from the command line by typing `library(sqlrutils)`.

> [!Note]
> You can load this library on computer that does not have SQL Server (for example, on an R Client instance) if you change the compute context to SQL Server and execute the code in that compute context.

## Class library

|Class | Description |
|------|-------------|
|[executeStoredProcedure](executestoredprocedure.md)| Execute a SQL stored procedure.|
|[getInputParameters](getinputparameters.md)| Get a list of input parameters to the stored procedure.| 
|[InputData](inputdata.md)| Input data for the stored procedure. | 
|[InputParameter](inputparameter.md)| Input parameters for the stored procedure.| 
|[OutputData](outputdata.md)| Output from the stored procedure.| 
|[OutputParameter](outputparameter.md) | Output parameters from the stored procedure.|
|[registerStoredProcedure](registerstoredprocedure.md) | Register the stored procedure with a database.|
|[setInputDataQuery](../olapr/query.md)| Assign a query to an input data parameter of the stored procedure.| 
|[setInputParameterValue](setinputparametervalue.md)| Assign a value to the an input parameter of the stored procedure.| 
|[StoredProcedure](storedprocedure.md)| A stored procedure object.|

## Next steps

Add R packages to your computer by running setup for R Server or R Client: 

+ [R Client](../r-client/what-is-microsoft-r-client.md) 
+ [R Server](../what-is-microsoft-r-server.md)

Next, review the steps in a typical sqlrutils workflow:

+ [Generating an R Stored Procedure for R Code using the sqlrutils Package](https://docs.microsoft.com/sql/advanced-analytics/r/generating-an-r-stored-procedure-for-r-code-using-the-sqlrutils-package)  

## See also

 [Package Reference](../introducing-r-server-r-package-reference.md)    
 [R tutorials for SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials) 