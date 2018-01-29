--- 
 
# required metadata 
title: "rxSyncPackages function (revoAnalytics) | Microsoft Docs" 
description: "  Synchronizes all R packages installed in a database with the packages on the files system. The packages contained in the database are also installed on the file system so they can be loaded by R.  You might need to use rxSyncPackages if you rebuilt a server instance or restored SQL Server databases on a new machine, or if you think an R package on the file system is corrupted.  " 
keywords: "(revoAnalytics), rxSyncPackages, sync, synchronize, use, packages, sql" 
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
 
 
 #rxSyncPackages: Synchronize Packages for Compute Context 
 ##Description
 

Synchronizes all R packages installed in a database with the packages on the files system. The packages contained in the database are also installed on the file system so they can be loaded by R. 
You might need to use rxSyncPackages if you rebuilt a server instance or restored SQL Server databases on a new machine, or if you think an R package on the file system is corrupted.

 
 
 ##Usage

```   
  rxSyncPackages(computeContext = rxGetOption("computeContext"), 
                scope = c("shared", "private"),
                owner = c(),
                verbose = getOption("verbose"))
 
```
 
 ##Arguments

   
    
 ### `computeContext`
 an [RxComputeContext](RxComputeContext.md) or equivalent character string or `NULL`.   If set to the default of `NULL`, the currently active compute context is used. Supported compute contexts are [RxInSqlServer](RxInSqlServer.md). 
  
  
    
 ### `scope`
 character vector containing either `"shared"` or `"private"` or both. `"shared"` synchronizes the packages on a per-database shared location on SQL Server, which in turn can be used (referred) by multiple different users. `"private"` synchronizes the packages on per-database, per-user private location on SQL Server which is only accessible to the single user. By default both `"shared"` and `"private"` are set, which will synchronize  the entire table of packages for all scopes and users. 
  
  
    
 ### `owner`
 character vector. Should be either empty `''` or a vector of valid SQL database user account names. Only users in `'db_owner'` role for a database can specify this value to install packages on  behalf of other users.  
  
  
    
 ### `verbose`
 logical. If `TRUE`, "progress report" is given during installation of given packages. 
  
 
 
 ##Details
 
For a [RxInSqlServer](RxInSqlServer.md) compute context, the user specified as part of connection string is considered the package owner if the `owner` argument is empty. 
To call this function, a user must be granted permissions by a database owner, making the user a member of either `'rpkgs-shared'` or `'rpkgs-private'` database role. 
Members of the `'rpkgs-shared'` role can install packages to `"shared"` location and `"private"` location. 
Members of the `'rpkgs-private'` role can only install packages in a `"private"` location for their own use. 
To use the packages installed on the SQL Server, a user must be at least a member of `'rpkgs-users'` role.

See the help file for additional details.
 
 
 
 ##Value
 
Invisible `NULL`
 
 
 ##See Also
 
[rxInstallPackages](rxInstallPackages.md),
[rxPackage](rxPackage.md),
install.packages,
[rxFindPackage](rxFindPackage.md),
[rxInstalledPackages](rxInstalledPackages.md),
[rxRemovePackages](rxRemovePackages.md),
[rxSqlLibPaths](rxSqlLibPaths.md),   
require
   
 ##Examples

 ```
   
  ## Not run:
 
#
# create SQL compute context
#
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )

#
# Synchronizes all packages listed in the database for all scopes and users
#
rxSyncPackages(computeContext=computeContext, verbose=TRUE)

#
# Synchronizes all packages for shared scope
#
rxSyncPackages(computeContext=computeContext, scope="shared", verbose=TRUE)

#
# Synchronizes all packages for private scope
#
rxSyncPackages(computeContext=computeContext, scope="private", verbose=TRUE)


#
# Synchronizes all packages for a private scope for user1
#
rxSyncPackages(pcomputeContext=computeContext, scope="private", owner = "user1", verbose=TRUE))

 ## End(Not run) 
  
 
```
     
 
 
 
 
 
 
