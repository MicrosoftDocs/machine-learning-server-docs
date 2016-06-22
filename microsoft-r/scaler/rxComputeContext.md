---

# required metadata
title: "RxComputeContext - ScaleR Functions"
description: "ScaleR Functions: RxComputeContext"
keywords: "RevoScaleR, ScaleR, RxComputeContext"
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "06/13/2016"
ms.topic: "article"
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


#RxComputeContext

Defines a new compute context or changes the definition of an existing compute context.

## Usage
`RxComputeContext( computeContext, <other>)`
     
## Arguments
_computeContext_ : A keyword that specifies the type of object to instantiate. For example, possible values are **RxLocalSeq** or **local**, or **RxLocalParallel**. You can also specify a string that references an existing RxComputeContext object.
  
_other_   : Depending on the type of compute context that you are creating, additional parameters might be required.


## Return Value
No change; any existing active compute context remains in effect until the `rxSetComputeContext` function is called to set a new compute context. 


## Remarks
A SQL DDL statement is prepared and passed to the ODBC driver.


## Example

The following example creates a compute context using previously defined variables, sets it as the active compute context to run a function, and then switches back to the local compute context.
~~~~
# switch between local and remote compute contexts
     
     sqlCompute <- RxInSqlServer(connectionstring = sqlConnString, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
     rxSetComputeContext("sqlCompute")
     x <- 1:10
     rxExec(print, x, elemType = "cores", timesToRun = 10)
     rxSetComputeContext("local")

~~~~

For additional examples of how to use local and remote compute contexts, see this tutorial: [Data Science Deep Dive - Using the ScaleR Functions](https://msdn.microsoft.com/en-us/library/mt637368.aspx)

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)