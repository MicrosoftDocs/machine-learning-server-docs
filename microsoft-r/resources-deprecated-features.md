---

# required metadata
title: "Deprecated, discontinued, or changed features - Machine Learning Server "
description: "Notifications about deprecated and discontinued features, packages, and functions in Microsoft R packages."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
ms.suite: "machine-learning"
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---
# Deprecated, discontinued, or changed features

**Applies to: Machine Learning Server and Microsoft R Server**

This article notifies you about pending status changes to features and function libraries, such as deprecated and discontinued functions in the custom packages and libraries installed with the product.

## Machine Learning Server 9.3

There are no announcements for package or function deprecation, discontinuation, or changes to existing fucntionality.

To learn more about this release, see [What's New](whats-new-in-machine-learning-server.md) for feature announcements and [Known issues](resources-known-issues.md) for descriptions and possible workarounds to known problems found in this release.

## Machine Learning Server 9.2.1

### RevoScaleR Package

|Function| Status | Replacement |
|-----------|----|--------|
|`RxHadoopMR` | Deprecated | [`RxSpark`](r-reference/revoscaler/rxspark.md)| 
|`RxHadoopMR-class` | Deprecated | [`RxSpark-class`](r-reference/revoscaler/rxspark-class.md)| 
|`RxInTeradata` | Deprecated | None. The only in-database compute context is [SQL Server](r-reference/revoscaler/rxinsqlserver.md).| 
|`RxInTeradata-class` | Deprecated | None. The only in-database compute context is [SQL Server](r-reference/revoscaler/rxinsqlserver-class.md). |

For more information, see [discontinued RevoScaleR functions](r-reference/revoscaler/revoscaler-defunct.md) and [deprecated RevoScaleR functions](r-reference/revoscaler/revoscaler-deprecated.md).


## Microsoft R Server 9.1.0

### RevoScaleR Package

|Function| Status | Replacement |
|-----------|----|--------|
|`rxGetNodes` | Deprecated | [`rxGetAvailableNodes`](r-reference/revoscaler/rxgetavailablenodes.md)| 
|`RxHpcServer` | Deprecated | [`RxSpark`](r-reference/revoscaler/rxspark.md) or [`RxHadoopMR`](r-reference/revoscaler/rxhadoopmr.md)| 
|`rxImportToXdf` | Deprecated | [`rxImport`](r-reference/revoscaler/rximport.md) |
|`rxDataStepXdf` | Deprecated | [`rxDataStep`](r-reference/revoscaler/rxdatastep.md) |
|`rxDataFrameToXdf` | Deprecated | [`rxDataStep`](r-reference/revoscaler/rxdatastep.md) |
|`rxXdfToDataFrame` | Deprecated | [`rxDataStep`](r-reference/revoscaler/rxdatastep.md) |
|`rxSortXdf` | Deprecated | [`rxSort`](r-reference/revoscaler/rxsortxdf.md) |
|`rxGetVarInfoXdf` |Discontinued |[`rxGetVarInfo`](r-reference/revoscaler/rxgetvarinfo.md))|
|`rxGetInfoXdf` |Discontinued |[`rxGetInfo`](r-reference/revoscaler/rxgetinfoxdf.md))|

For more information, see [discontinued RevoScaleR functions](r-reference/revoscaler/revoscaler-defunct.md) and [deprecated RevoScaleR functions](r-reference/revoscaler/revoscaler-deprecated.md).

### RevoMods Package

In **RevoMods** functions are discontinued (all were intended for use solely by the R Productivity Environment discontinued in Microsoft R Server 8.0.3):

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
    + `rxReadXdf` (use `rxImport` or `rxDataStep`)

+ RevoScaleR discontinued functions:
    + (none)
     	

###Bug Fixes

+ Not available.

###Known Issues

See here: https://docs.microsoft.com/en-us/machine-learning-server/resources-known-issues

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
    	+ `rxgetvarinfo` (use `rxGetVarInfo`)
    	+ `rxGetInfoXdf` (us `rxGetInfo`)
    	+ the `covariance` argument (only) to the `rxLinMod` function

###Changed in this release

+  A new internal variable, `.rxPredict` is available inside transformation functions to indicate that the data is being processed from a prediction rather than a model estimation.

+ **Microsoft R documentation** has been moved from the product distribution to MSDN. The “doc” directories in the RevoScaleR and RevoPemaR packages have been removed, as has the top-level R Server “doc” directory.

+ Microsoft R **licenses and Third Party Notices** files are now included in the new `MicrosoftR` package. The Revo.home() function now points to the location of this directory, and `Revo.home(“licenses”)` points to the “licenses” directory within. The `Revo.home(“doc”)` component is now defunct.

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

See here: https://docs.microsoft.com/en-us/machine-learning-server/resources-known-issues

## Microsoft R Server 8.0.0

**Known Issues**

See here: http://go.microsoft.com/fwlink/?LinkID=717964&clcid=0x409
