---

# required metadata
title: "Run Microsoft R Server on Windows"
description: "How to install, connect to, and use Microsoft R Server on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/21/2016"
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

# Run Microsoft R Server on Windows

Microsoft R Server is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers and clusters. You can run R Server on a wide range of computing platforms, including Microsoft Windows.

R Server offers feature parity across platforms. Whether you use Windows or another platform, R Server is mostly the same in all contexts, with the exception of data-specific and platform helper functions created specifically for Hadoop MapReduce, Spark, SQL Server, or Teradata. To review specific OS and database platform versions that can be used for an R Server deployment, see [Supported platforms](rserver-install-supported-platforms.md).

Installation of R Server provides the following components:

* [**R Open**](r-open.md), Microsoft's distribution of R.
* [**ScaleR (RevoScaleR)**](scaler/scaler.md), a proprietary package with functions that enable data chunking and distributed, parallel workloads on single or clustered installations.
* [**Microsoft Machine Learning (MML package)**](microsoftml-introduction.md), a proprietary collection of functions used in R code or script for performing machine learning at scale.
* [**Operationalization features**](operationalize/about.md) that add deployment capabilities, enabled in part through the new [**mrsdeploy package**](mrsdeploy/mrsdeploy.md) in the Microsoft R 9.0 release.
* [**Other packages listed in Package Reference**](package-reference.md).

For information about the latest features, see [What's New in R Server](rserver-whats-new.md).

## Using R Server with other tools

R Server is commercial-grade software designed to run workloads that you have created and tested on a workstation that has **Microsoft R Client** and development tools such as **R Tools for Visual Studio (RTVS)**, RStudio, or other applications that can consume R packages.

R Server provides the infrastructure for distributing a workload across multiple nodes (referred to as *data chunking*), running jobs in parallel, and then reassembling the results for further analysis and visualization.

R Client is a scaled down, free execution engine for Microsoft R features that includes function libraries and operationalization features. It's suitable for development and smaller workloads that fit in memory, using a maximum of two processors. R Client is typically used in conjunction with development applications like RTVS to create and tune R solutions locally, prior to deployment on a commercial server platform like R Server for Windows. For more information about workstation setup and features, see [R Client](r-client.md).

Solutions developed locally on workstation can be published to remote R Server instances offering performance and scale. Existing solutions can be deployed as-is, modified to accommodate large data sets, or reconfigured to use a specific compute context. In this release, the [mrsdeploy package](mrsdeploy/mrsdeploy.md) gives you the ability to toggle between remote and local sessions in an R console application. As you change the compute context and make other adjustments to increase data size, you can set up a remote session and issue commands to validate your changes incrementally.

## How to install R Server 9.0

You can install R Server 9.0 and previous major versions side-by-side on the same computer, but you can only install one copy of each major version. As a standalone server, R Server for Windows is not multi-instance. If you require multiple copies of R Server at the same functional level on a single server, you can install SQL Server R Services as a multi-instance service, similar to how the relational database engine and other features are installed in SQL Server.

### Prerequisites

+ **.NET Framework 4.5.2** or later. The installer checks for this version of the .NET Framework and provides a download link if you need to install it first. A computer restart is required after the .NET Framework is installed.

+ You must accept the end user agreement. This agreement explains that R Server is licensed as a SQL Server enterprise feature, even though it can be installed independently of SQL Server on a Windows operating system.

+ You must agree to an installation of **R Open**. R Open is Microsoft's distribution of packages providing the R language and base functions. It is fully compatible with open source R and the R language, but includes performance optimizations that make R Open a better choice for R Server operations. Setup installs R Open for you, but it is also downloadable from the [MRAN web site](https://mran.microsoft.com/).

The following additional components are installed by Setup.

| Component | Version |
|-----------|---------|
| Microsoft AS OLE DB Provider for SQL Server 2016 | 13.0.1601.5 |
| Microsoft .NET Core | 1.0.1 |
| Microsoft MPI | 7.1.12437.25 |
| Microsoft Visual C++ 2013 Redistributable | 12.0.30501.0 |
| Microsoft Visual C++ 2015 Redistributable | 14.0.23026.0 |

### Download R Server installer

You can download the installation program from the following locations:

+ Through your existing MSDN subscription.
+ [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) provides a zipped file, free to developers who sign up for Visual Studio Dev Essentials. This is the Developer edition of Microsoft R Server; it has the same features as Enterprise except it is licensed for development scenarios.

    - Click **Join or Access Now** and enter your account information.
    - Click **Downloads**, and then search for *Microsoft R*.
    - Be sure that you are connected to Visual Studio Dev Essentials before searching the **Downloads** list. You're in the right place if the URL starts with *my.visualstudio.com*.

> A download option on [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) for R Server on Windows is pending. VLSC is expected to have an ISO available for the 9.0 release in January 2017.

### Run Setup

RServerSetup.exe provides an installation wizard for standalone server deployment. The installer checks for prerequisites, prompts for acceptance of user agreements, and gives you the option of choosing a different program directory. When you complete the wizard, setup installs R Server, plus additional software (noted in the prerequisites) required for operations.

Post-installation, you can review log files. Log files (RServerSetup_<timestamp>.log) can be found in your system temp directory. An easy way to navigate to the directory is to enter %temp% as a Run command or search operation.

### Connect to R Server and validate installation

R Server runs on demand as a background process. When connected to R Server, Task Manager shows **R GUI for Windows front-end** running as an app. **Microsoft R Engine** runs as a background process.

After R Server is installed, you can connect to the server and execute a few ScaleR functions to validate the installation.

**Local Connections**

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to view a list of objects already loaded. You should see the `RevoScaleR` package in the list. If you want to load a package that's not automatically available, such as `mrsdpeloy`, type `load.package("mrsdeploy")`.
4. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`.

**Remote Connections**

1. On a workstation computer, start a client application such as Rgui.exe or R Tools for Visual Studio (RTVS).
2. TBD

## Install earlier versions of R Server for Windows

Earlier versions are supported, but are no longer available on Microsoft download sites. If you already have one of the older supported versions, you can use the links in this section to complete the installation.

| Version | Details|
|---------|--------|
| Version 8.0.5  | This version of R Server for Windows, released as Microsoft R Server 2016, is integrated with the enterprise edition of SQL Server 2016. Licensing, installation, and support of this version of R Server for Windows is through SQL Server. Using SQL Server setup, you can install R Server as a standalone server, or as multi-instance service within SQL Server. For more information, see [R Server Standalone](https://msdn.microsoft.com/library/mt671127.aspx) and [SQL Server R Services - R Server install page](https://msdn.microsoft.com/library/mt671127.aspx) in SQL Server 2016 technical documentation.|
| Version 8.0 | Installation of this version is package by package, with a specific order of installation requirement. For more information and instructions, see [Install Revolution R Enterprise 2016 (version 8.0) for Windows](rserver-install-windows-800.md).|

## See Also

[Supported platforms](rserver-install-supported-platforms.md)

[What's new in R Server](notes/r-server-notes.md)

[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)
