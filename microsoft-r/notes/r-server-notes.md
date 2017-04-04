---

# required metadata
title: "Microsoft R Server release notes"
description: "Readme file for Microsoft R Server release, documenting known issues, bug fixes, and deprecation notificiations."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "04/04/2017"
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
#R Server release notes (Microsoft R)

This article documents the known issues, bug fixes, and notifications about status changes to function libraries, such as deprecated and discontinued functions in RevoScaleR.

**Related Documents**

+ For feature announcements, see [What's new in R Server](../rserver-whats-new.md).
+ For Microsoft R Client, see [Release notes for R Client](r-client-notes.md).
+ For SQL Server R Services, see [What's new in SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604847.aspx).

## Microsoft R Server 9.1.0
 
###Changed in this release
+ R Server is built on Microsoft R Open 3.3.3 (R 3.3.3).
+ The `curl` package has been updated to version 2.3
+ The `jsonlite` package has been updated to version 1.2

###Deprecated or Discontinued
 
In **RevoScaleR**, deprecated functions and recommended replacements include the following:

|Deprecated | Replacement |
|-----------|------------|
|`rxGetNodes` | [`rxGetAvailableNodes`](rxGetAvailableNodes.md)| 
|`RxHpcServer` | [`RxSpark`](rxSpark.md) or [`RxHadoopMR`](rxHadoopMR.md)| 
|`rxImportToXdf` | [`rxImport`](rxImport.md) |
|`rxDataStepXdf` | [`rxDataStep`](rxDataStep.md) |
|`rxDataFrameToXdf` | [`rxDataStep`](rxDataStep.md) |
|`rxXdfToDataFrame` | [`rxDataStep`](rxDataStep.md) |
|`rxSortXdf` | [`rxSort`](rxSortXdf.md) |

The following functions are discontinued:

 + `rxGetVarInfoXdf` (use [`rxGetVarInfo`](rxGetVarInfoXdf.md))
 + `rxGetInfoXdf` (Use [`rxGetInfo`](rxGetInfoXdf.md))

For more information, see [discontinued RevoScaleR functions](scaler/packagehelp/RevoScaleR-defunct.md) and [deprecated RevoScaleR functions](scaler/packagehelp/RevoScaleR-deprecated.md).

In **RevoMods** functions have been discontinued (all were intended for use solely by the R Productivity Environment discontinued in Microsoft R Server 8.0.3):

+ `?` (use the standard R `?`, previously masked)
+ `q` (use the standard R `q` function, previously masked)
+ `quit` (use the standard R `quit` function, previously masked)
+ `revoPlot` (use the standard R `plot` function)
+ `revoSource` (use the standard R `source` function)


## Microsoft R Server 9.0.1

###Changed in this release

+ R Server is built on Microsoft R Open 3.3.2 (R 3.3.2).
+ R packages added to MRO include `curl`, `Jsonlite`, and `R6`.
+ R packages added to R Server include `mrupdate` (for automated update checks) and `CompatibilityAPI` (to check for feature compatibility between versions).
+ DeployR (now referred to as operationalization) no longer supports MongoDB. Use PostGreSQL, SQL Server, or Azure SQL Database instead.

###Deprecated or Discontinued

+ RevoScaleR deprecated functions:
    + `RxHpcServer` (no replacement)
    + `rxReadXdf` (use`rxDataStep`)

+ RevoScaleR discontinued functions:
    + (none)
     	

###Bug Fixes

+ Not available.

###Known Issues

See here: https://msdn.microsoft.com/microsoft-r/rserver-known-issues

## Microsoft R Server 8.0.5

###Deprecated or Discontinued

+ The **R Productivity Environment** (RPE) IDE for Revolution R Enterprise:
  + The RPE is now defunct. We recommend use of R Tools for Visual Studio (RTVS), which provides a more modern and flexible IDE.
  + These R packages that support the RPE have been removed: `revoIpe`, `pkgXMLBuilder`, `XML`, and `RevoRpeConnector`.

+ `RevoScaleR` changes:
    + These `RevoScaleR` functions are now deprecated:
	    + `rxImportToXdf` (use `rxImport`)
	    + `rxDataStepXdf` (use `rxDataStep`)
	    + `rxXdfToDataFrame` (use `rxDataStep`)
	    + `rxDataFrameToXdf` (use `rxDataStep`)

    + These `RevoScaleR` functions are now defunct:
    	+ `rxGetVarInfoXdf` (use `rxGetVarInfo`)
    	+ `rxGetInfoXdf` (us `rxGetInfo`)
    	+ the `covariance` argument (only) to the `rxLinMod` function

###Changed in this release

+  A new internal variable, `.rxPredict` is available inside transformation functions to indicate that the data is being processed from a prediction rather than a model estimation.

+ **Microsoft R documentation** has been moved from the product distribution to MSDN. The “doc” directories in the RevoScaleR and RevoPemaR packages have been removed, as has the top-level R Server “doc” directory.

+ Microsoft R **licenses and Third Party Notices** files are now included in the new `MicrosoftR` package. The `Revo.home()` function now points to the location of this directory, and `Revo.home(“licenses”)` points to the “licenses” directory within. The `Revo.home(“doc”)` component is now defunct.

+ An opt-in telemetry feature allows you to anonymously help improve Microsoft R Server by enabling us to gather data on the R Server functions you use, operating system, R version, and RevoScaleR version. Turn it on using the `rxPrivacyControl` function in `RevoScaleR`.

**Bug Fixes**

+ `rxKmeans` now works with an `RxHadoopMR` compute context and an `RxTextData` data source.

+ When writing to an `RxTextData` data source using
`rxDataStep`, specifying `firstRowIsColNames` to `FALSE` in
the output data source will now correctly result in no
column names being written.

+ When writing to an `RxTextData` data source using
`rxDataStep`, setting `quoteMark` to `""` in the output data
source will now result in no quote marks written around
character data.

+ When writing to an `RxTextData` data source using
`rxDataStep`, setting `missingValueString` to `""` in the
output data source will now result in empty strings for
missing values.

+ Using `rxImport`, if `quotedDelimiters` was set to `TRUE`,
transforms could not be processed.

+ `rxImport` of `RxTextData` now reports a warning instead of
an error if mismatched quotes are encountered.

+ When using `rxImport` and appending to an `.xdf` file, a
combination of the use of `newName` and `newLevels` for
different variables in `colInfo` could result in `newLevels`
being ignored, resulting in a different set of factor levels
for the appended data.

+ When using `rxPredict`, with an in-formula transformation of
the dependent variable, an error was given if the variable
used in the transformation was not available in the
prediction data set.

+ Hadoop bug fix for incompatibility when using both HA and Kerberos on HDP.

+ Deployr default grid node and any additional nodes should have 20 slots by default

+ DeployR was generating very large (~52GB) catalina.out log files.

+ When running scripts in the DeployR Repository Manager's Test tab, any numeric values set to `0` were ignored and not included as part of the request.

**Known Issues**

See here: https://msdn.microsoft.com/en-us/microsoft-r/rserver-known-issues

## Microsoft R Server 8.0.0

**Known Issues**

See here: http://go.microsoft.com/fwlink/?LinkID=717964&clcid=0x409
