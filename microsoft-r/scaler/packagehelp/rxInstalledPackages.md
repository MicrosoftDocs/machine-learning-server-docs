--- 
 
# required metadata 
title: "Installed Packages for Compute Context" 
description: " Find (or retrieve) details of installed packages for a compute context. " 
keywords: "RevoScaleR, rxInstalledPackages, packages, sql, install, uninstall, remove, use" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 #`rxInstalledPackages`: Installed Packages for Compute Context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Find (or retrieve) details of installed packages for a compute context.
 
 
 ##Usage

```   
  
  rxInstalledPackages(computeContext = NULL, allNodes = FALSE, lib.loc = NULL,
                      priority = NULL, noCache = FALSE, fields = "Package",
                      subarch = NULL)
 
```
 
 ##Arguments

   
  
    
 ### `computeContext`
 an [RxComputeContext](RxComputeContext.md) or equivalent character string or `NULL`.   If set to the default of `NULL`, the currently active compute context is used. Supported compute contexts are [RxInTeradata](RxInTeradata.md), [RxInSqlServer](RxInSqlServer.md), [RxLocalSeq](RxLocalSeq.md). 
  
  
    
 ### `allNodes`
 logical. If `TRUE` and an [RxInTeradata](RxInTeradata.md) compute context is used, a list of results from each node is returned. 
   
   
    
 ### `lib.loc`
 a character vector describing the location of R library  trees to search through, or `NULL`.  The default value of `NULL` corresponds to checking the loaded namespace,  then all libraries currently known in  `.libPaths()`. In [RxInSqlServer](RxInSqlServer.md) only `NULL` is supported. 
  
   
    
 ### `priority`
 character vector or `NULL` (default). If non-null, used to select packages;  `"high"` is equivalent to `c("base", "recommended")`.  To select all packages without an assigned priority use priority = `"NA"`. 
  
   
    
 ### `noCache`
 logical.  If `TRUE`, do not use cached information, nor cache it. 
  
   
    
 ### `fields`
 a character vector giving the fields to extract from each package's DESCRIPTION file,  or `NULL`. If `NULL`, the following fields are used: `"Package"`, `"LibPath"`, `"Version"`, `"Priority"`, `"Depends"`,  `"Imports"`, `"LinkingTo"`, `"Suggests"`, `"Enhances"`,  `"License"`, `"License_is_FOSS"`, `"License_restricts_use"`,  `"OS_type"`, `"MD5sum"`, `"NeedsCompilation"`, and `"Built"`. Unavailable fields result in `NA` values. 
  
   
    
 ### `subarch`
 character string or `NULL`. If non-null and non-empty, used to select packages  which are installed for that sub-architecture.  
  
  
 
 
 ##Details
 
This is a wrapper for installed.packages. See the help file for additional details.
Note that `rxInstalledPackages` treats the `field` argument differently, only
returning the `fields` specified in the argument.
 
 
 
 ##Value
 
By default, a character vector of installed packages is returned.  If `fields` is not
set to `"Package"`, a matrix with one row per package is returned. 
The row names are the package names and the possible
column names are `"Package"`, `"LibPath"`, `"Version"`, `"Priority"`, `"Depends"`, 
`"Imports"`, `"LinkingTo"`, `"Suggests"`, `"Enhances"`, 
`"License"`, `"License_is_FOSS"`, `"License_restricts_use"`, 
`"OS_type"`, `"MD5sum"`, `"NeedsCompilation"`,
and `"Built"` (the R version the package was built under). 
Additional columns can be specified using the fields argument. 
If using a distributed compute context with the `allNodes` set to `TRUE`,
a list of matrices from each node will be returned.
In [RxInSqlServer](RxInSqlServer.md) compute context multiple rows for a package will be returned if different versions of the
same package is installed in different `"system"`, `"shared"` and `"private"` scopes.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxPackage](rxPackage.md),
installed.packages,
[rxFindPackage](rxFindPackage.md),
[rxInstallPackages](rxInstallPackages.md),   
[rxRemovePackages](rxRemovePackages.md),
[rxSqlLibPaths](rxSqlLibPaths.md),   
require
   
 ##Examples

 ```
   
  #
  # Find the packages installed for the current compute context
  #
  myPackages <- rxInstalledPackages()
  myPackages
  
  #
  # Get full information about all the packages installed for the current compute context
  #
  myPackageInfo <- rxInstalledPackages(fields = NULL)
  myPackageInfo
  
  #
  # Get specific information about the installed packages
  #
  myPackageInfo <- rxInstalledPackages(fields = c("Package", "Version", "Built"))
  myPackageInfo
  
  ## Not run:
 
#
# Find the packages installed on a SQL Server compute context
#
sqlServerCompute <- RxInSqlServer(connectionString = 
    "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;")
sqlPackages <- rxInstalledPackages(computeContext = sqlServerCompute)
sqlPackages
 ## End(Not run) 
  
 
```
     
 
 
 
 
 
 
 
