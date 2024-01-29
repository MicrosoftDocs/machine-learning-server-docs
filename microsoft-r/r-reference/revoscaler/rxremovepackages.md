--- 

# required metadata 
title: "rxRemovePackages function (revoAnalytics) | Microsoft Docs" 
description: " Removes installed packages from a compute context. " 
keywords: "(revoAnalytics), rxRemovePackages, remove, uninstall, packages, sql" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "sql-non-specified"
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
ms.technology: machine-learning-server

--- 


 # rxRemovePackages: Remove Packages from Compute Context 
 ## Description

Removes installed packages from a compute context.


 ## Usage

```   

  rxRemovePackages(pkgs, lib, dependencies = TRUE, checkReferences = TRUE,
      verbose = getOption("verbose"), scope = "private", owner = '', computeContext = rxGetOption("computeContext"))

```

 ## Arguments




 ### `pkgs`
 a `character` vector of names of the packages to be removed. 



 ### `lib`
 a `character` vector  identifying library path from where the package needs to be removed. This argument is not supported in [RxInSqlServer](RxInSqlServer.md) compute context. Only valid for a local compute context. 



 ### `dependencies`
 logical. Applicable only for [RxInSqlServer](RxInSqlServer.md) compute context. If `TRUE`, does dependency resolution of the packages being removed and removes the dependent packages also if the dependent packages aren't referenced by other packages outside the dependency closure.  



 ### `checkReferences`
 logical. Applicable only for [RxInSqlServer](RxInSqlServer.md) compute context. If `TRUE`, verifies there are no references to the dependent packages by other packages outside the dependency closure.  



 ### `verbose`
 logical. If `TRUE`, "progress report" is given during removal of given packages. 



 ### `scope`
 character. Applicable only for [RxInSqlServer](RxInSqlServer.md) compute context. Should be either `"shared"` or `"private"`.  `"shared"` removes the packages from a per-database shared location on SQL Server which in turn could have been used (referred) by multiple different users. `"private"` removes the packages from a per-database, per-user private location on SQL Server which is only accessible to the single user. 



 ### `owner`
 character. Applicable only for [RxInSqlServer](RxInSqlServer.md) compute context. This is generally empty `''` value.  Should be either empty `''` or a valid SQL database user account name. Only users in `'db_owner'` role for a database can specify this value to remove packages on  behalf of other users.  



 ### `computeContext`
 an [RxComputeContext](RxComputeContext.md) or equivalent character string or `NULL`.   If set to the default of `NULL`, the currently active compute context is used. Supported compute contexts are [RxInSqlServer](RxInSqlServer.md), [RxLocalSeq](RxLocalSeq.md). 




 ## Details

This is a simple wrapper for remove.packages. 
For [RxInSqlServer](RxInSqlServer.md) compute context, the user specified as part of connection string is used for removing the packages if `owner` argument is empty. The user calling this function needs to be granted permissions by database owner by making them member of either `'rpkgs-shared'` or `'rpkgs-private'` database role. Users in `'rpkgs-shared'` role can remove packages from `"shared"` location and their own `"private"` location. Users in `'rpkgs-private'` role can only remove packages from their own `"private"` location.



 ## Value

Invisible `NULL`


 ## See Also

[rxPackage](rxPackage.md),
remove.packages,
[rxFindPackage](rxFindPackage.md),
[rxInstalledPackages](rxInstalledPackages.md),
[rxInstallPackages](rxInstallPackages.md),  
[rxSyncPackages](rxSyncPackages.md),
[rxSqlLibPaths](rxSqlLibPaths.md),   
require

 ## Examples

 ```

  ## Not run:

#
# create SQL compute context
#
sqlcc <- RxInSqlServer(connectionString = 
    "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;")

pkgs <- c("dplyr")

#
# Remove a package and its dependencies after checking references from private scope
#
rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)

#
# Remove a package and its dependencies after checking references from shared scope
#
rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "shared", computeContext = sqlcc)

#
# Forcefully remove a package and its dependencies without checking references from 
# private scope
#
rxRemovePackages(pkgs = pkgs, checkReferences = FALSE, verbose = TRUE, 
  scope = "private", computeContext = sqlcc)

#
# Force fully remove a package without its dependencies and without 
# any references checking from shared scope private scope
#
rxRemovePackages(pkgs = pkgs, dependencies = FALSE, checkReferences = FALSE, 
  verbose = TRUE, scope = "shared", computeContext = sqlcc)

#
# Remove a package and its dependencies from private scope for user1
#
rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", owner = "user1", computeContext = sqlcc)

#
# Remove a package and its dependencies from shared scope for user1
#
rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "shared", owner = "user1", computeContext = sqlcc)
 ## End(Not run) 
```






