---

# required metadata
title: "Install Microsoft R Server for Windows"
description: "How to install Microsoft R Server on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/17/2016"
ms.topic: "get-started-article"
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

# Install Microsoft R Server for Windows

R Server can be installed on a variety of operating systems and integrates with several database platforms. For more information about specific OS and database platform versions, see [Supported platforms](rserver-install-supported-platforms.md).

R Server for Windows is designed to run workloads that you have created and tested on a workstation that has **Microsoft R Client** and development tools such as **R Tools for Visual Studio (RTVS)**, RStudio, or other applications that can consume R packages. R Client is a scaled down, free execution engine for Microsoft R features that includes function libraries and operationalization features. R Client is used in conjunction with tools like RTVS to create and tune R solutions locally, prior to deployment on a commercial server platform like R Server for Windows. For more information about workstation setup, see [R Client](r-client.md).

R Server includes the following components:

* R Open
* ScaleR (RevoScaleR package)
* Operational support for solution deployment and interaction (mrsdeploy package)
* Microsoft Machine Learning (MML package)
* Other packages as described in [Package Reference](package-reference.md)

## Install R Server 9.0

You can install R Server 9.0 and previous major versions side-by-side on the same computer, but you can only install one copy of each major version. As a standalone server, R Server for Windows is not multi-instance. If you require multiple copies of R Server at the same functional level on the same server, you can install SQL Server R Services, which is a multi-instance service similar to the relational database engine.

### Prerequisites

**.NET Framework 4.5.2** or later. The installer checks for this version of the .NET Framework and provides a download link in case you need to install it prior to R Server. A computer restart is required after the .NET Framework is installed.

You must accept the end user agreement. This agreement explains that R Server is licensed as a SQL Server enterprise feature, even though it can be installed independently of SQL Server on a Windows operating system.

You must agree to an installation of **R Open**. R Open is Microsoft's distribution of packages providing the R language. It is fully compatible with open source R and the R language, but includes the following additional components that make R Open a better choice for R Server operations: intel-mkl package, XXX, YYY. R Open is free under the GNU open source licensing framework and can be downloaded separately from the MRAN web site.

### How to obtain R Server for Windows

You can download the installation program from the following locations:

* MSDN Dev Essentials
* (pending) Microsoft volume licensing

## How to connect to R Server

Once the software is installed, you can connect to R Server and run ScaleR functions.

<TO DO>

Sample data is include in R Server installations:

*
*

## Install R Server 8.0.5

This version of R Server for Windows, released as Microsoft R Server 2016, is integrated with the enterprise edition of SQL Server 2016. Licensing and installation of this version of R Server for Windows is through SQL Server. Using SQL Server setup, you can install R Server as a standalone server, or as multi-instance service within SQL Server. For more information, see [R Server Standalone](https://msdn.microsoft.com/en-us/library/mt671127.aspx) and [SQL Server R Services - R Server install page](https://msdn.microsoft.com/en-us/library/mt671127.aspx) in SQL Server 2016.

## See Also

[Supported platforms](rserver-install-supported-platforms.md)
[What's new in R Server](r-server-notes.md)
[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)
