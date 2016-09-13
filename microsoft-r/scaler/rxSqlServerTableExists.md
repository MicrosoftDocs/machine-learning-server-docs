---

# required metadata
title: "ScaleR Functions rxSqlServerTableExists"
description: "ScaleR Functions: rxSqlServerTableExists"
keywords: "RevoScaleR, ScaleR, rxSqlServerTableExists"
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

#rxSqlServerTableExists

Returns a Boolean that indicates whether the specified database table exists.

## Usage

`rxSqlServerTableExists(table, connectionString)`

## Arguments

_table_: A string containing a table name or the name of an `RxSqlServerData` data source that specifies a table.

_connectionString_: A string containing connection information.  If NULL, the connection string from the currently active compute context will be used.

## Remarks
A SQL query is prepared and passed to the ODBC driver.

## Return Value
TRUE if the table exists, FALSE if the table does not exists or was not found.

## Example

The following example checks for the existence of a table and drops it if it exists.
~~~~
if (rxSqlServerTableExists(tempTable)) rxSqlServerDropTable(tempTable)
~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](https://msdn.microsoft.com/en-us/library/mt652103.aspx)