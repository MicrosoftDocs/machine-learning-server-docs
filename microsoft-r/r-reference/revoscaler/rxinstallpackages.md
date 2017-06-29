--- 
 
# required metadata 
title: "Install Packages for Compute Context" 
description: "  **NOTE: This new API is in pre-release mode and subject to change before final release.**  Install Packages from Repositories or Local Files for a compute context.  " 
keywords: "RevoScaleR, rxInstallPackages, install, use, packages, sql" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #rxInstallPackages: Install Packages for Compute Context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 

**NOTE: This new API is in pre-release mode and subject to change before final release.**

Install Packages from Repositories or Local Files for a compute context.

 
 
 ##Usage

```   
  
  rxInstallPackages(pkgs, skipMissing = FALSE, repos = getOption("repos"), verbose = getOption("verbose"), 
        scope = "private", owner = '', computeContext = rxGetOption("computeContext"))
 
```
 
 ##Arguments

   
  
    
 ### pkgs
 `character` vector of the names of packages whose current versions should be downloaded from the repositories. If repos = NULL, a character vector of file paths of .zip files containing binary builds of packages. (http:// and file:// URLs are also accepted and the files will be downloaded and installed from local copies. If you specify .zip files with repos = `NULL` with [RxComputeContext](rxcomputecontext.md) compute context the .zip file paths should already be present on a folder. 
   
   
    
 ### skipMissing
 logical. Applicable only for [RxInSqlServer](rxinsqlserver.md) compute context. If `TRUE`, skips missing dependent packages for which otherwise an error is generated.  
  
  
    
 ### repos
 character vector, the base URL(s) of the repositories to use.Can be NULL to install from local files, directories. 
  
  
    
 ### verbose
 logical. If `TRUE`, "progress report" is given during installation of given packages. 
  
  
    
 ### scope
 character. Applicable only for [RxInSqlServer](rxinsqlserver.md) compute context. Should be either `"shared"` or `"private"`.  `"shared"` installs the packages on per database shared location on SQL server which in turn can be used (referred) by multiple different users. `"private"` installs the packages on per database, per user private location on SQL server which is only accessible to the single user. 
  
  
    
 ### owner
 character. Applicable only for [RxInSqlServer](rxinsqlserver.md) compute context. This is generally empty `''` value.  Should be either empty `''` or a valid SQL database user account name. Only users in `'db_owner'` role for a database can specify this value to install packages on  behalf of other users.  
  
  
    
 ### computeContext
 an [RxComputeContext](rxcomputecontext.md) or equivalent character string or `NULL`.   If set to the default of `NULL`, the currently active compute context is used. Supported compute contexts are [RxInSqlServer](rxinsqlserver.md), [RxLocalSeq](rxlocalseq.md). 
  
  
 
 
 ##Details
 
This is a simple wrapper for install.packages. 
For [RxInSqlServer](rxinsqlserver.md) compute context the user specified as part of connection string is used for installing the packages if `owner` argument is empty. The user calling this function needs to be granted permissions by database owner by making them member of either `'rpkgs-shared'` or `'rpkgs-private'` database role. Users in `'rpkgs-shared'` role can install packages to `"shared"` location and `"private"` location. Users in `'rpkgs-private'` role can only install packages `"private"` location for their own use. To use the packages installed on the SQL server a user needs to be member atleast `'rpkgs-users'` role.

See the help file for additional details.
 
 
 
 ##Value
 
Invisible `NULL`
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxPackage](rxpackage.md),
install.packages,
[rxFindPackage](rxfindpackage.md),
[rxInstalledPackages](rxinstalledpackages.md),
[rxRemovePackages](rxremovepackages.md),
[rxSyncPackages](rxsyncpackages.md),
[rxSqlLibPaths](rxsqllibpaths.md),   
require
   
 ##Examples

 ```
   
  ## Not run:
 
#
# create SQL compute context
#
sqlcc <- RxInSqlServer(connectionString = "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;")

#
# list of packages to install
#
pkgs <- c("dplyr")

#
# Install a package and its dependencies into private scope
#
rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)

#
# Install a package and its dependencies into shared scope
#
rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "shared", computeContext = sqlcc)

#
# Install a package and its dependencies into private scope for user1
#
rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", owner = "user1", computeContext = sqlcc)

#
# Install a package and its dependencies into shared scope for user1
#
rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "shared", owner = "user1", computeContext = sqlcc)
 ## End(Not run) 
  
 
```
     
 
 
 
 
 
