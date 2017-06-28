--- 
 
# required metadata 
title: "Search Paths for Packages in SQL compute context" 
description: " **NOTE: This new API is in pre-release mode and subject to change before final release.**  Gets the search path for the library trees for packages while executing inside the SQL server using [RxInSqlServer](RxInSqlServer.md) compute context or using T-SQL script with sp_execute_external_script stored procedure with embedded R script. " 
keywords: "RevoScaleR, rxSqlLibPaths, use, packages, sql, install, uninstall, remove" 
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
 
 
 #`rxSqlLibPaths`: Search Paths for Packages in SQL compute context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
**NOTE: This new API is in pre-release mode and subject to change before final release.**

Gets the search path for the library trees for packages while executing inside the SQL server using [RxInSqlServer](RxInSqlServer.md) compute context or using T-SQL script with sp_execute_external_script stored procedure with embedded R script.
 
 
 ##Usage

```   
  rxSqlLibPaths(connectionString)
 
```
 
 ##Arguments

   
  
    
 ### `connectionString`
 a `character` connection string for the SQL server. This should be local connection string as external connection strings are not supported while executing on a SQL server. You can also specify [RxInSqlServer](RxInSqlServer.md) compute context object for input from which the connection string will be extracted and used.  
   
 
 
 ##Details
 
For [RxInSqlServer](RxInSqlServer.md) compute context, the user specified as part of connection string needs to be a member of one of the following roles `'db_owner'` `'rpkgs-shared'`,  `'rpkgs-private'` or `'rpkgs-private'` in the database. When rxExec() function is called from client machine with [RxInSqlServer](RxInSqlServer.md) compute context to execute the rx function on SQL server the `.libPaths()` is automatically updated to include the library paths returned by this [rxSqlLibPaths](rxSqlLibPaths.md) function.

See the help file for additional details.
 
 
 
 ##Value
 
A character vector of the library paths containing both `"shared"` or `"private"` scope of the packages if the the user specified in the connection string is allowed access. On acccess denied, returns empty character vector.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxPackage](rxPackage.md),
.libPaths,
[rxFindPackage](../../r-reference/revoscaler/rxfindpackage.md),
[rxInstalledPackages](rxInstalledPackages.md),
[rxInstallPackages](rxInstallPackages.md),   
[rxRemovePackages](rxRemovePackages.md),
[rxSyncPackages](rxSyncPackages.md),
require
   
 ##Examples

 ```
   
  ## Not run:
 
#
# An example sp_execute_external_script T-SQL code using rxSqlLibPaths()
#
declare @instance_name nvarchar(100) = @@SERVERNAME, @database_name nvarchar(128) = db_name();
exec sp_execute_external_script 
  @language = N'R',
  @script = N'
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    .libPaths(rxSqlLibPaths(connStr));
    print(.libPaths());
  ', 
  @input_data_1 = N'', 
  @params = N'@instance_name nvarchar(100), @database_name nvarchar(128)',
  @instance_name = @instance_name, 
  @database_name = @database_name;

 ## End(Not run) 
  
 
```
     
 
 
 
 
 
 
 
