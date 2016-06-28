---

# required metadata
title: "R Client Release Notes"
description: "R Client Readme"
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
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

#Microsoft R Server 2016 Release notes

The following release notes apply to Microsoft R Server.

##Microsoft R Server 2016

**New in this Release**

+ On **Linux**:
  + Support for RedHat RHEL 7.x has been added.

  + Installers are now composed of RPM packages, which can be installed via a top-level install script or as individual RPM packages. This can be convenient for Enterprise IT departments managing extensive deployments.

+ On **Hadoop**:

  + Installation on Hadoop clusters has been simplified to eliminate manual steps.

  + New support Hadoop on SUSE 11 and Hadoop distributions (Cloudera CDH 5.5-5.7, Hortonworks HDP 2.4, MapR 5.0-5.1)

  + A new distributed compute context `RxSpark` is available, in which computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark. This provides up to a 7x performance boost compared to `RxHadoopMR`.

  + New Hadoop diagnostic tool to collect status of MRS and dependencies on all nodes (available through a CSS Support request)

  + Hadoop user directories in HDFS and Linux are now created automatically as needed.

  + New Hadoop administrator script to clean-up orphaned HDFS and Linux user directories.

+ On **Teradata**:

  + New option has been added to `rxPredict` to insert into an existing table.

  + New option has been added for use of LDAP authentication with a TPT connection.

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

    +  An new internal variable, `.rxPredict` is available inside transformation functions to indicate that the data is being processed from a prediction rather than a model estimation.

+ **DeployR Enterprise** includes the following changes and improvements:

  + Deployr Enterprise is more secure than ever with improved Web security features for better protection against malicious attacks, improved installation security, and improved Security Policy Management.

  + DeployR Enterprise now relies on an H2 database by default and allows you to easily use a SQL Server or PostgreSQL database instead to fit your production environment. 

  + DeployR Enterprise now has a simplified installer for a better customer experience.

  + The XML format for data exchange is deprecated, and will be removed from future versions of DeployR.

  + The API has been updated. [See the change history.](../deployr-api-reference.md#805)

  + This release is of DeployR Enterprise only.
  
+ **Microsoft R documentation** has been moved from the product distribution to this site on MSDN. The “doc” directories in the RevoScaleR and RevoPemaR packages have been removed, as has the top-level R Server “doc” directory.

+ Microsoft R **licenses and Third Party Notices** files are now included in the new `MicrosoftR` package. The `Revo.home()` function now points to the location of this directory, and `Revo.home(“licenses”)` points to the “licenses” directory within. The `Revo.home(“doc”)` component is now defunct.

For information on SQL Server R Services, please refer to the corresponding [release notes](https://msdn.microsoft.com/en-us/library/mt604847.aspx). 

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

<br />
##Microsoft R Server 8.0.0

**New Features in this Release**

This guide is an introduction to using the new features of Microsoft R Services. The current release is Microsoft R Services 2016, which includes the following new features:

+ Revolution R Open is now Microsoft R Open, and Revolution R Enterprise is now generically known as Microsoft R Services, and specifically Microsoft R Server on Linux platforms.

+ Microsoft R Services installs on top of an enhanced version of R 3.2.2, Microsoft R Open for Revolution R Enterprise 8.0.0 (on Windows) or Microsoft R Open for Microsoft R Server (on Linux)

+ Installation of Microsoft R Services has been simplified from three installers to two: the new Microsoft R Open for Revolution R Enterprise/Microsoft R Server installer combines Microsoft R Open with the GPL/LGPL components needed to support Microsoft R Services, so there is no need for the previous “Revolution R Connector” install.

+ RevoScaleR includes:
    + New Fuzzy Matching Algorithms: The new rxGetFuzzyKeys and rxGetFuzzyDist functions provide access to fuzzy matching 
algorithms for cleaning and analyzing text data.
    + Support for Writing in ODBC Data Sources. The RxOdbcData data source now supports writing
    + Bug Fixes: 
        + When using rxDataStep, new variables created in a transformation no longer inherit the rxLowHigh attribute of the variable used to create them.
        + rxGetInfo was failing when an extended class of RxXdfData was used.
        + rxGetVarInfo now respects the `newName` element of colInfo for non-xdf data sources.
        + If `inData` for rxDataStep is a non-xdf data source that contains a colInfo specification using `newName`, the `newName` should now be used for `varsToKeep` and `varsToDrop`.
    + Deprecated and Defunct. 
        + `NIEDERR` is no longer supported as a type of random number generator.
        + `scheduleOnce` is now defunct for rxPredict.rxDForest and rxPredict.rxBTrees.
        + The compute context `RxLsfCluster` is now defunct.
        + The compute context `RxHpcServer` is now deprecated

+ DevelopR - The R Productivity Environment (the IDE provided with Revolution R Enterprise on Windows) is not deprecated, but it will be removed from future versions of Microsoft R Services.

+ RevoMPM, a Multinode Package Manager, is now defunct, as it was deemed redundant. Numerous distributed shells are available, including pdsh, fabric, and PyDSH. More sophisticated provisioning tools such as Puppet, Chef, and Ansible are also available. Any of these can be used in place of RevoMPM.

+ Known Issues: http://go.microsoft.com/fwlink/?LinkID=717964&clcid=0x409
