---

# required metadata
title: "What's New in Microsoft R Server"
description: "Updates, improvements, and changes in this release of Microsoft R Server"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/21/2016"
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

# What's New in R Server 9.0

This release of R Server includes the following new features. To review known issues, bug fixes, and notifications about feature status, see the [release notes for R Server](../notes/r-server-notes.md). To learn about new features added to Microsoft R Client, see [What's new in R Client](../notes/r-client-notes.md).

##MicrosoftML Package (New)

**Microsoft Machine Learning (MicrosoftML package)** is new in this release. It contains a collection of functions in Microsoft R used for doing machine learning at scale, with fast performance, even when handling a large corpus of text data and high-dimensional categorical data. To learn more, see [Introduction to MicrosoftML](microsoftml-introduction.md).

##RevoScaleR Package

**Spark 2.0 support in ScaleR** is announced in this release.

**ScaleR (RevoScaleR package)** adds the following new functions.

|Function | Description |
|--|--|
|RxHiveData|--|
|RxParquetData |--|
|rxSparkConnect |--|
|rxSparkDisconnect |--|
|rxSparkListData |--|
|rxSparkRemoveData|--|

##mrsdeploy Package (New)

**mrsdeploy package** is new in this release. Functions in this package enable remote execution on the command line, in a session that's open on a remote R Server 9.0 instance. Additionally, this package includes functions for deploying Web services. You can publish any R code block as a Web service on a local or remote R Server 9.0 instance, with additional commands for service management. To learn more, see [mrsdeploy Function Reference](mrsdeploy/mrsdeploy.md).

##General updates

**DeployR** feature is now referred to as the Operationalization feature.

**Operationalization updates**

<TBD>

**R Server for Windows installation** includes a simplified setup program for a standalone install. You are no longer required to run SQL Server Setup to get a standalone R Server on Windows. For standalone server install on Windows, you can use either the simplified setup or the SQL Server installer. For SQL Server R Services, you must use SQL Server Setup. For information on SQL Server R Services, see [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx). For installation guidance, see [Install R Server for Windows](rserver-install-windows.md).

> Although the installation experience is changing, licensing is not. R Server for Windows remains a SQL Server enterprise feature, even when installed as a standalone server. You will need a SQL Server enterprise license to install R Server on Windows.

**Service and support for Microsoft R** now follows the  Modern Lifecycle policy, which covers products that are serviced and supported continuously. For more information, see [Support for Microsoft R Server Versions](rserver-servicing-support.md).

Microsoft R **licenses and Third Party Notices** files are included in the `MicrosoftR` package. The `Revo.home()` function now points to the location of this directory, and `Revo.home(“licenses”)` points to the “licenses” directory within. The `Revo.home(“doc”)` component is now defunct.
