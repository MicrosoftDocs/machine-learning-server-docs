---

# required metadata
title: "Run Microsoft R Server for Windows"
description: "How to install, connect to, and use Microsoft R Server on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/09/2016"
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

# Run Microsoft R Server for Windows

Microsoft R Server is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers and clusters. You can run R Server on a wide range of computing platforms, including Microsoft Windows.

R Server offers feature parity across platforms. Whether you use Windows or another platform, R Server is mostly the same in all contexts, with the exception of data-specific and platform helper functions created specifically for Hadoop MapReduce, Spark, SQL Server, or Teradata. For description of components, benefits, and usage scenarios, see [Introduction to R Server](rserver.md).

To learn more about the latest release, see [What's New in R Server](rserver-whats-new.md).

## How to install R Server 9.0.1 on Windows

In this release, you can use a simplified setup program for R Server for Windows. This setup is in addition to SQL Server Setup, which continues to be a viable option for installation for R Server on Windows.

The setup program you use determines feature availability and the service and support policy.

+ Simplified setup installs the [9.0.1 feature set](rserver-whats-new.md) that includes operationalization.

  Using this setup, R Server for Windows is serviced under the [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912). Modern lifecycle policy is designed for rapid release cycles. Individual versions age out sooner, but newer features roll out more frequently.

+ SQL Server vNext setup installs the [9.0.0 feature set](https://msdn.microsoft.com/library/mt604847.aspx). Operationalization is not provided in this feature set.

  Using SQL Server Setup, SQL Server's support policy is in effect (search for "SQL Server 2016" on [this page](https://support.microsoft.com/en-us/lifecycle)). SQL Server support policy offers servicing updates and hot fixes over a longer time frame, but newer features roll out more slowly. For more information about installation options in SQL Server, see [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx).

### Prerequisites

+ A supported version of Windows. For an up-to-date list, see [Supported platforms](rserver-install-supported-platforms.md).

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

<a name="Download"><a/>
### Download R Server installer

You can download the installation program from the following locations:

+ [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx).
+ [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) provides a zipped file, free to developers who sign up for Visual Studio Dev Essentials. This is the Developer edition of Microsoft R Server; it has the same features as Enterprise except it is licensed for development scenarios.

    - Click **Join or Access Now** and enter your account information.
    - Click **Downloads**, and then search for *Microsoft R*.
    - Be sure that you are connected to Visual Studio Dev Essentials before searching the **Downloads** list. You're in the right place if the URL starts with *my.visualstudio.com*.

> A download option on [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) for R Server for Windows is pending. VLSC is expected to have an ISO available for the 9.0.1 release in January 2017.

### Run Setup

RServerSetup.exe provides an installation wizard for standalone server deployment. The installer checks for prerequisites, prompts for acceptance of user agreements, and gives you the option of choosing a different program directory. When you complete the wizard, setup installs R Server, plus additional software (noted in the prerequisites) required for operations.

Post-installation, you can review log files. Log files (RServerSetup_<timestamp>.log) can be found in your system temp directory. An easy way to navigate to the directory is to enter %temp% as a Run command or search operation.

Additionally, you should install a development tool on the server to code script or solutions that use R Server features. We recommend the following development environment:

+ [Visual Studio 2015](https://www.visualstudio.com/downloads/)
+ [R Tools for Visual Studio (RTVS) add-in](https://www.visualstudio.com/vs/rtvs/)

### Connect to R Server and validate installation

R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like RTVS or Rgui.exe connects to the server.

As a verification step, you can connect to the server and execute a few ScaleR functions to validate the installation.

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to view a list of objects already loaded. You should see the `RevoScaleR` package in the list. If you want to load a package that's not automatically available, such as `mrsdpeloy`, type `load.package("mrsdeploy")`.
4. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`.

##Side-by-side installation

You can install R Server 9.0.1 and previous major versions side-by-side on the same computer, but you can only install one copy of each major version. As a standalone server, R Server for Windows is not multi-instance. If you require multiple copies of R Server at the same functional level on a single server, you can install SQL Server R Services as part of a multi-instance relational database engine service.

## Install earlier versions of R Server for Windows

Earlier versions are supported, but are no longer available on Microsoft download sites. If you already have one of the older supported versions, you can use the links in this section to access installation instructions.

| Version | Details|
|---------|--------|
| Version 8.0.5  | This version of R Server for Windows, released as Microsoft R Server 2016, is integrated with the enterprise edition of SQL Server 2016. Licensing, installation, and support of this version of R Server for Windows is through SQL Server. Using SQL Server setup, you can install R Server as a standalone server, or as multi-instance service within SQL Server. For more information, see [R Server for Windows](https://msdn.microsoft.com/library/mt671127.aspx) and [SQL Server R Services - R Server install page](https://msdn.microsoft.com/library/mt671127.aspx) in SQL Server 2016 technical documentation.|
| Version 8.0 | Installation of this version is package by package, in a specific order. For more information and instructions, see [Install Revolution R Enterprise 2016 (version 8.0) for Windows](rserver-install-windows-800.md).|

## See Also

[Supported platforms](rserver-install-supported-platforms.md)

[What's new in R Server](notes/r-server-notes.md)

[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)
