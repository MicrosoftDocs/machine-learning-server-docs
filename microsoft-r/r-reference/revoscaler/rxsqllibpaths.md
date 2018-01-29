--- 
 
# required metadata 
title: "rxSqlLibPaths function (revoAnalytics) | Microsoft Docs" 
description: " Gets the search path for the library trees for packages while executing inside the SQL Server, using [RxInSqlServer](RxInSqlServer.md) compute context or using T-SQL script with sp_execute_external_script stored procedure with embedded R script. " 
keywords: "(revoAnalytics), rxSqlLibPaths, use, packages, sql, install, uninstall, remove" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 #rxSqlLibPaths: Search Paths for Packages in SQL compute context 
 ##Description
 
Gets the search path for the library trees for packages while executing inside the SQL Server, using [RxInSqlServer](RxInSqlServer.md) compute context or using T-SQL script with sp_execute_external_script stored procedure with embedded R script.
 
 
 ##Usage

```   
  rxSqlLibPaths(connectionString)
 
```
 
 ##Arguments

   
  
    
 ### `connectionString`
 a `character` connection string for the SQL Server. This should be a local connection string (external connection strings are not supported while executing on a SQL Server). You can also specify [RxInSqlServer](RxInSqlServer.md) compute context object for input, from which the connection string will be extracted and used.  
   
 
 
 ##Details
 
For [RxInSqlServer](RxInSqlServer.md) compute context, a user specified on the connection string must be a member of one of the following roles `'db_owner'` `'rpkgs-shared'`,  `'rpkgs-private'` or `'rpkgs-private'` in the database. 
When rxExec() function is called from a client machine with [RxInSqlServer](RxInSqlServer.md) compute context to execute the rx function on SQL Server, the `.libPaths()` is automatically updated to include the library paths returned by this [rxSqlLibPaths](rxSqlLibPaths.md) function.
 
 
 
 ##Value
 
A character vector of the library paths containing both `"shared"` or `"private"` scope of the packages if the the user specified in the connection string is allowed access. On acccess denied, it returns an empty character vector.
 
 
 ##See Also
 
[rxPackage](rxPackage.md),
.libPaths,
[rxFindPackage](rxFindPackage.md),
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
     
 
 
 
 
 
 
 
