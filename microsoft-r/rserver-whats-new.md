---

# required metadata
title: "What's New in Microsoft R Server"
description: "Updates, improvements, and changes in this release of Microsoft R Server"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/02/2016"
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

# What's New in R Server 9.0.1

This release of R Server, built on open source R 3.3.2, includes new and updated packages, plus new operationalization features in the core engine.

**Related Documents**

+ For known issues, bug fixes, and behavior changes in existing features, see the [Release notes for R Server](notes/r-server-notes.md).
+ For new feature announcements in Microsoft R Client, see [What's new in R Client](notes/r-client-notes.md).
+ For new feature announcements in SQL Server R Services (version 9.0), see [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx).

## New and updated packages

**mrsdeploy package** is new in this release and available on all [platforms supporting operationalization](rserver-install-supported-platforms.md). Functions support remote execution on a R Server 9.0.1 instance, and the ability to publish, and subsequently manage, an R code block as a web service. To learn more, see [mrsdeploy Function Reference](mrsdeploy/mrsdeploy.md).

**Microsoft Machine Learning algorithms (MicrosoftML package)** is a collection of functions for incorporating machine learning into R code or script that executes on R Server and R Client. It's available in the following Microsoft R products: R Server for Windows, R Client for Windows, and [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx). Availability on Linux and Hadoop is projected for the first quarter of 2017. To learn more, see [Introduction to MicrosoftML](microsoftml-introduction.md).

**RevoScaleR Package** is updated to include [support for **Spark 2.0**](#bkmk_Spark). For a list of all functions, see [RevoScaleR Function Reference](scaler/scaler.md).

|Function | Description |
|--|--|
|`RxHiveData`|Generate a Hive Data Source object.|
|`RxParquetData `|Generate a Parquet Data Source object.|
|`rxSparkConnect` | Create Spark compute context, connect and disconnect a Spark application. |
|`rxSparkDisconnect `| Create Spark compute context, connect and disconnect a Spark application. |
|`rxSparkListData` |Remove or list cached `RxParquetData` or `RxHiveData`.|
|`rxSparkRemoveData`|Remove or list cached `RxParquetData` or `RxHiveData`.|

> [!NOTE]
> Although ScaleR jobs only execute on Spark 2.0 if you have [R Server 9.0.1 for Hadoop](rserver-install-hadoop.md), you can create solutions containing Hive, Parquet, and Spark-related functions in R Client.

## General updates

<a name="operationalize"></a>
**Operationalization features**

You can configure R Server after installation to act as a deployment server and host analytic web services. When you enable operationalization, you can use [mrsdeploy](mrsdeploy/mrsdeploy.md) or write code against a swagger API to publish and consume web services composed of R script or code on the server. Remote execution can be interactive at the command line, or instrumented programmatically using the API.

An operationalized R Server supports advanced multi-server topologies composed of web and compute nodes on clustered servers, with the ability to accept pipelined data streams that are subsequently transformed, analyzed, and visualized using web services.

Operationalization includes tool support, a simplified deployment API, and new app integration experiences:

+ Configuration using an administrative tool, used to designate web and compute nodes and grant administrator rights.

+ Simplified deployment allows you to bundled R analytics into a Web Service with minor code changes. The `mrsdeploy` package enables Web service deployment from either R Client and R Server, but to create a Web service programmatically, R Server is required.

+ Simplified app integration is provided through Swagger-based APIs, easy to consume, supported on a wide range of programming languages.

+ In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. Connections are encrypted using HTTPS and only authenticated requests are accepted.

Configuration is required before you can operationalize R Server, but there is no separate installation component. Operationalization is bundled into the R Server installer on selected platforms including Windows, Red Hat Enterprise Linux, CentOS, and Ubuntu. For details, see [Supported platforms](rserver-install-supported-platforms.md). For feature information and next steps, see [Operationalization with R Server](operationalize/about.md) and [Configure operationalization on R Server](operationalize/configuration-initial.md).

> [!NOTE]
> In the context of operationalization, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure. Feature support is limited to a subset of the supported R Server platforms. [Supported platforms](rserver-install-supported-platforms.md) has the list.

**R Server for Linux**

This release now supports Ubuntu 14.04 and 16.04 on premises. For installation instructions, see [Install R Server for Linux](rserver-install-linux-server.md).

<a name="bkmk_Spark"></a>
**R Server for Hadoop (MapReduce and Spark)**

+ Support for Spark 1.6 and 2.0.
+ Support for Spark DataFrames through `RxHiveData` and `RxParquetData` in ScaleR:

  ~~~~
    hiveData <- RxHiveData("select * from hivesampletable", ...)
    pqData <- RxParquetData('/share/claimsParquet', ...)
  ~~~~

Additional new ScaleR functions for Spark 2.0:

+ Manage Spark persistent sessions: `rxSparkConnect`, `rxSparkDisconnect`
+ Manage data in Spark DataFrames : `rxSparkListData`, `rxSparkRemoveData`

For installation instructions, see [Install R Server for Hadoop](rserver-install-hadoop.md). For ScaleR help, see [ScaleR Function Reference](scaler/scaler.md).

**R Server for Windows**

As noted, installation of R Server or R Client on Windows delivers the new [MicrosoftML package](microsoftML-introduction.md) for machine learning.

Additionally, this release adds a simplified setup program for a standalone R Server installation on Windows. This setup is in addition to SQL Server Setup, which continues to be a viable option for installation.

Features in the 9.0.1 release are currently only available through simplified setup. In contrast, SQL Server Setup installs the 9.0 version of R Server for Windows. For a description of the features in 9.0, see [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx).

For installation and upgrade instructions, see [Install R Server for Windows](rserver-install-windows.md).

> [!NOTE]
> Although the installation experience is changing, licensing is not. R Server for Windows remains a SQL Server enterprise feature, even when installed outside of SQL Server Setup. A SQL Server enterprise license is required for the enterprise edition of R Server for Windows.

**Service and support for Microsoft R Server for Windows**

R Server for Windows can be serviced under the [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) or under [SQL Server's support policy (search for "SQL Server 2016" on this page)](https://support.microsoft.com/en-us/lifecycle). For support information for R Server in general, see [Support for Microsoft R Server Versions](rserver-servicing-support.md).

+ Modern lifecycle policy is designed for rapid release cycles. Individual versions age out sooner, but newer features roll out more frequently. The Modern lifecycle policy is in effect when you use simplified setup to install R Server for Windows.

+ SQL Server support policy supports released versions over a longer time frame, but updates are less frequent. This support policy is in effect when you use SQL Server Setup to install a standalone R Server for Windows.

+ Simplified setup can be used to replace instance-by-instance installs of SQL Server R Services. This is useful if you require 9.0.1 operationalization capabilities, or if you want to switch from the SQL Server support policy to the Modern Lifecycle policy. To upgrade, run simplified setup on a Windows computer that has an existing R Server instance that was previously installed using SQL Server Setup. You will be prompted to run a tool that handles the conversion.

##Previously released features

This section lists the feature announcements of recent previous releases.

### Announced in Microsoft R Server 9.0

R Server 9.0 is a Windows-only, SQL-Server-only release. It is roughly equivalent to the 9.0.1 release, minus the `mrsdeploy` package and new [operationalization features](operationalize/about.md). For a description of the features in 9.0, see [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx).

### Announced in Microsoft R Server 8.0.5

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

+ **DeployR Enterprise** includes the following changes and improvements:

    + Deployr Enterprise is more secure than ever with improved Web security features for better protection against malicious attacks, improved installation security, and improved Security Policy Management.

    + DeployR Enterprise now relies on an H2 database by default and allows you to easily use a SQL Server or PostgreSQL database instead to fit your production environment.

    + DeployR Enterprise now has a simplified installer for a better customer experience.

    + The XML format for data exchange is deprecated, and will be removed from future versions of DeployR.

    + The API has been updated. [See the change history.](deployr-api-reference.md#805)

    + This release is of DeployR Enterprise only.

### Announced in Microsoft R Server 8.0.0

+ Revolution R Open is now Microsoft R Open, and Revolution R Enterprise is now generically known as Microsoft R Services, and specifically Microsoft R Server for Linux platforms.

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
