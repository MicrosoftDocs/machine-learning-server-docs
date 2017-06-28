---

# required metadata
title: "sqlrutils Functions"
description: "A package used for executing stored procedures on SQL Server from R script."
keywords: "sqlrutils package reference"
author: "richcalaway"
manager: "jhubbard"
ms.date: "04/02/2017"
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

# sqlrutils functions

The **sqlrutils** package provides functions for executing stored procedures on SQL Server from R script.

## Class library

|Class | Description |
|------|-------------|
|[`executeStoredProcedure`](../r-reference/sqlrutils/executestoredprocedure.md)| Execute a SQL stored procedure.|
|[`getInputParameters`](../r-reference/sqlrutils/getinputparameters.md)| Get a list of input parameters to the stored procedure.| 
|[`InputData`](../r-reference/sqlrutils/inputdata.md)| Input data for the stored procedure. | 
|[`InputParameter`](../r-reference/sqlrutils/inputparameter.md)| Input parameters for the stored procedure.| 
|[`OutputData`](../r-reference/sqlrutils/outputdata.md)| Output from the stored procedure.| 
|[`OutputParameter`](../r-reference/sqlrutils/outputparameter.md) | Output parameters from the stored procedure.|
|[`registerStoredProcedure`](../r-reference/sqlrutils/registerstoredprocedure.md) | Register the stored procedure with a database.|
|[`setInputDataQuery`](../r-reference/olapr/query.md)| Assign a query to an input data parameter of the stored procedure.| 
|[`setInputParameterValue`](../r-reference/sqlrutils/setinputparametervalue.md)| Assign a value to the an input parameter of the stored procedure.| 
|[`StoredProcedure`](packagehelp/StoredProcedure.md)| A stored procedure object.|

## Get help on sqlrutils functions from the R console

To see the **sqlrutils** functions that can be called from the R console:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
2. Load `sqlrutils` from the command line by typing `library(sqlrutils)`.
1. In the console, open the package help by typing the following at the R prompt: `help(package="sqlrutils")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.
 
> [!NOTE]
> To list all public functions, type library(help="sqlrutils") at the R prompt.
>



## See also

[Package Reference](~/package-reference.md)

[Install R Server](~/rserver.md)

[Install R Client](~/r-client.md)