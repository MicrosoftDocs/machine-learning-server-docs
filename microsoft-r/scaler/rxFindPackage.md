---

# required metadata
title: "ScaleR Functions RxInSqlServer"
description: "ScaleR Functions: RxInSqlServer"
keywords: "RevoScaleR, ScaleR, RxInSqlServer"
author: "j-martens"
manager: "jhubbard"
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

# rxFindPackage

Finds the path for one or more packages for a compute context.

## Usage

     rxFindPackage(package, computeContext = NULL, allNodes = FALSE, lib.loc = NULL, quiet = TRUE,
                  verbose = getOption("verbose"))

## Arguments

_package_: character vector of name(s) of packag(es).

_computeContext_: an ‘RxComputeContext’ or equivalent character string or ‘NULL’.  If set to the default of ‘NULL’, the currently active compute context is used.

_allNodes_: logical. If ‘TRUE’ and an ‘RxInTeradata’ compute context is used, a list of results from each node is returned.

_lib.loc_: a character vector describing the location of R library trees to search through, or ‘NULL’.  The default value of ‘NULL’ corresponds to checking the loaded namespace, then all libraries currently known in ‘.libPaths()’.

_quiet_: logical. If ‘FALSE’, warnings or an error is given if the package is not found.

_verbose_: logical. If ‘TRUE’, additional diagnostics are printed if available.

## Remarks

This is a wrapper for ‘find.package’. See the help file for additional details.
Related R functions are `find.package` and `require`.

## Return Value

A character vector of paths of package directories.  If using a distributed compute context with the ‘allNodes’ set to ‘TRUE’, a list of lists with a character vector of paths from each node will be returned.



## Examples

This example demonstrates how to find the paths for the **RevoScaleR** and **lattice** packages.

~~~~
packagePaths <- rxFindPackage(package = c("RevoScaleR", "lattice"))
packagePaths
~~~~

This example gets the path of the RevoScaleR package in a SQL Server compute context.

~~~~
sqlServerCompute <- RxInSqlServer(connectionString =
"Driver=SQL Server;Server=myServer;Database=TestDB;Uid=myID;Pwd=myPwd;")
sqlPackagePaths <- rxFindPackage(package = "RevoScaleR", computeContext = sqlServerCompute)
~~~~

## See Also:
rxInstalledPackages (link)
