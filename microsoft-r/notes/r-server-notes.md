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

+ The R Productivity Environment (RPE), a custom IDE for Revolution R Enterprise, is now defunct. We recommend use of R Tools for Visual Studio (RTVS), which provides a more modern and flexible IDE. 

  + These R packages that were previously included to support the RPE have also been removed: `revoIpe`, `pkgXMLBuilder`, `XML`, and `RevoRpeConnector`. 
  
  + These R functions, which had been modified to work with the RPE on Windows, have been returned to their original state: `q`, `quit`, `?`, `.tryHelp`, `print.help_files_with_topic`, and `timestamp`. 
  
  + These R functions, which were modified and renamed for use in the RPE on Windows, are now defunct: `revoFix`, `revoPlot`, `revoPlot.default`, `revoPlot.ts`, `revoPlot.matrix`, `revoPlot.data.frame`, and `revoSource`.
 
+ Microsoft R documentation has been moved from the product distribution to this site on MSDN. The “doc” directories in the RevoScaleR and RevoPemaR packages have been removed, as has the top-level R Server “doc” directory.

+ Microsoft R licenses and Third Party Notices files are now included in the new `MicrosoftR` package. The `Revo.home()` function now points to the location of this directory, and `Revo.home(“licenses”)` points to the “licenses” directory within. The `Revo.home(“doc”)` component is now defunct.

+ Linux installers are now composed of RPM packages that can be installed via a top-level install script or as individual RPM packages. This can be convenient for Enterprise IT departments managing extensive deployments.

+ Installation on Hadoop clusters has been simplified.

+ DeployR Enterprise includes the following changes and improvements:

  + Deployr Enterprise is more secure than ever with improved Web security features for better protection against malicious attacks, improved installation security, and improved Security Policy Management.

  + DeployR Enterprise now relies on an H2 database by default and allows you to easily use a SQL Server or PostgreSQL database instead to fit your production environment. 

  + DeployR Enterprise now has a simplified installer for a better customer experience.

  + The XML format for data exchange is not deprecated, but it will be removed from future versions of DeployR.

For information on SQL Server R Services, please refer to the corresponding [release notes](https://msdn.microsoft.com/en-us/library/mt604847.aspx). 

**Bug Fixes**


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
        + rxGetVarInfo now respects the ‘newName’ element of colInfo for non-xdf data sources.
        + If ‘inData’ for rxDataStep is a non-xdf data source that contains a colInfo specification using ‘newName’, the ‘newName’ should now be used for ‘varsToKeep’ and ‘varsToDrop’.
    + Deprecated and Defunct. 
        + ‘NIEDERR’ is no longer supported as a type of random number generator.
        + ‘scheduleOnce’ is now defunct for rxPredict.rxDForest and rxPredict.rxBTrees.
        + The compute context ‘RxLsfCluster’ is now defunct.
        + The compute context ‘RxHpcServer’ is now deprecated

+ DevelopR - The R Productivity Environment (the IDE provided with Revolution R Enterprise on Windows) is not deprecated, but it will be removed from future versions of Microsoft R Services.

+ RevoMPM, a Multinode Package Manager, is now defunct, as it was deemed redundant. Numerous distributed shells are available, including pdsh, fabric, and PyDSH. More sophisticated provisioning tools such as Puppet, Chef, and Ansible are also available. Any of these can be used in place of RevoMPM.

+ Known Issues: http://go.microsoft.com/fwlink/?LinkID=717964&clcid=0x409
