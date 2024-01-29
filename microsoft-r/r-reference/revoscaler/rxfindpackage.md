--- 

# required metadata 
title: "rxFindPackage function (revoAnalytics) | Microsoft Docs" 
description: " Find the path for one or more packages for a compute context. " 
keywords: "(revoAnalytics), rxFindPackage, packages, sql, install, uninstall, remove, use" 
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


 # rxFindPackage: Find Packages for Compute Context 
 ## Description

Find the path for one or more packages for a compute context.


 ## Usage

```   

  rxFindPackage(package, computeContext = NULL, allNodes = FALSE, lib.loc = NULL, quiet = TRUE,
               verbose = getOption("verbose"))


```

 ## Arguments




 ### `package`
 character vector of name(s) of packag(es). 



 ### `computeContext`
 an [RxComputeContext](RxComputeContext.md) or equivalent character string or `NULL`.   If set to the default of `NULL`, the currently active compute context is used. Supported compute contexts are [RxInSqlServer](RxInSqlServer.md) and [RxLocalSeq](RxLocalSeq.md). 



 ### `allNodes`
 logical 



 ### `lib.loc`
 a character vector describing the location of R library trees to search through, or `NULL`.  The default value of `NULL` corresponds to checking the loaded namespace, then all libraries currently known in  `.libPaths()`. In [RxInSqlServer](RxInSqlServer.md) only `NULL` is supported. 



 ### `quiet`
 logical. If `FALSE`, warnings or an error is given if the package is not found. 



 ### `verbose`
 logical. If `TRUE`, additional diagnostics are printed if available. 



 ## Details

This is a wrapper for find.package. See the help file for additional details.



 ## Value

A character vector of paths of package directories. 
If using a distributed compute context with the `allNodes` set to `TRUE`,
a list of lists with a character vector of paths from each node will be returned.   



 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[rxPackage](rxPackage.md),
find.package,
[rxInstalledPackages](rxInstalledPackages.md),
[rxInstallPackages](rxInstallPackages.md),   
[rxRemovePackages](rxRemovePackages.md),
[rxSyncPackages](rxSyncPackages.md),
[rxSqlLibPaths](rxSqlLibPaths.md),   
require

 ## Examples

 ```

  #
  # Find the paths for the RevoScaleR and lattice packages
  #
  packagePaths <- rxFindPackage(package = c("RevoScaleR", "lattice"))
  packagePaths

  ## Not run:

#
# Find the path of the RevoScaleR package on a SQL Server compute context
#
sqlServerCompute <- RxInSqlServer(connectionString = 
    "Driver=SQL Server;Server=myServer;Database=TestDB;Uid=myID;Pwd=myPwd;")
sqlPackagePaths <- rxFindPackage(package = "RevoScaleR", computeContext = sqlServerCompute)
 ## End(Not run) 
```








