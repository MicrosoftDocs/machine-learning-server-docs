---

# required metadata
title: "Run Microsoft R Server for Windows"
description: "How to install, connect to, and use Microsoft R Server on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/01/2016"
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

In this release, you can use a simplified setup program for a standalone R Server installation on Windows. This setup is in addition to SQL Server Setup, which continues to be a viable option for installation of a standalone R Server instance on Windows.

The setup program you use determines the service and support policy.

+ Using simplified setup, R Server for Windows is serviced under the [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912). Modern lifecycle policy is designed for rapid release cycles. Individual versions age out sooner, but newer features roll out more frequently.

+ Using SQL Server Setup, SQL Server's support policy is in effect (search for "SQL Server 2016" on this page)](https://support.microsoft.com/en-us/lifecycle). SQL Server support policy offers servicing updates and hot fixes over a longer time frame, but at longer intervals. For more information about installation options in SQL Server, see [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx).

>[!NOTE]
> Simplified setup can be used to replace instance-by-instance installs of SQL Server R Services. This is useful if you want to switch from the SQL Server support policy to the Modern Lifecycle policy. To switch support policies, run simplified setup on a Windows computer that has an existing R Server instance that was previously installed using SQL Server Setup. You will be prompted to run a tool that handles the conversion.

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

+ Through your existing MSDN subscription.
+ [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) provides a zipped file, free to developers who sign up for Visual Studio Dev Essentials. This is the Developer edition of Microsoft R Server; it has the same features as Enterprise except it is licensed for development scenarios.

    - Click **Join or Access Now** and enter your account information.
    - Click **Downloads**, and then search for *Microsoft R*.
    - Be sure that you are connected to Visual Studio Dev Essentials before searching the **Downloads** list. You're in the right place if the URL starts with *my.visualstudio.com*.

> A download option on [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) for R Server for Windows is pending. VLSC is expected to have an ISO available for the 9.0 release in January 2017.

### Run Setup

RServerSetup.exe provides an installation wizard for standalone server deployment. The installer checks for prerequisites, prompts for acceptance of user agreements, and gives you the option of choosing a different program directory. When you complete the wizard, setup installs R Server, plus additional software (noted in the prerequisites) required for operations.

Post-installation, you can review log files. Log files (RServerSetup_<timestamp>.log) can be found in your system temp directory. An easy way to navigate to the directory is to enter %temp% as a Run command or search operation.

Additionally, you should install a development tool on the server to code script or solutions that use R Server features. We recommend the following development environment:

+ [Visual Studio 2015](https://www.visualstudio.com/downloads/)
+ [R Tools for Visual Studio (RTVS) add-in](https://www.visualstudio.com/vs/rtvs/)

> Unless you are using the remote execution feature of `mrsdeploy`, the only additional configuration is to provide credentials for login or give individual Windows user or groups login rights to the machine. R Server interaction is either local to the machine, where individual data scientists use Remote Desktop to start a local session, or accessed remotely via `mrsdeploy` on the command line. For more information, see [mrsdeploy](mrsdeploy/mrsdeploy.md).

### Connect to R Server and validate installation

R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like RTVS or Rgui.exe connects to the server.

As a verification step, you can connect to the server and execute a few ScaleR functions to validate the installation.

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to view a list of objects already loaded. You should see the `RevoScaleR` package in the list. If you want to load a package that's not automatically available, such as `mrsdpeloy`, type `load.package("mrsdeploy")`.
4. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`.

##Side-by-side installation

You can install R Server 9.0.x and previous major versions side-by-side on the same computer, but you can only install one copy of each major version. As a standalone server, R Server for Windows is not multi-instance. If you require multiple copies of R Server at the same functional level on a single server, you can install SQL Server R Services as part of a multi-instance relational database engine service.

##Upgrade 9.0 to 9.0.1

Currently, version 9.0.1 is only available through simplified setup. SQL Server Setup installs 9.0 only. If you want to use the new 9.0.1 features with an existing 9.0 instance, you can run simplified setup program and accept the prompts to upgrade to the newer version.

Doing so voids the SQL Server Support policy, replacing it with the Modern Lifecycle Support policy described above. Moving forward, R-related updates to SQL Server are not applied to your installation. Instead, updates are applied through the Microsoft *check for updates* program.

+  On a computer that has standalone R Server or SQL Server R Services (in database), as installed by SQL Server Setup, [download and run the new setup program](#download) to upgrade to 9.0.1.

Later, if you want to revert to the SQL Server Support policy, follow these steps:

1. Uninstall the SQL Server relational database instance that previously hosted R Services.
2. Reinstall the SQL Server relational database instance with R Services.
3. Reapply any service updates or hot fixes to that instance.

## Install earlier versions of R Server for Windows

Earlier versions are supported, but are no longer available on Microsoft download sites. If you already have one of the older supported versions, you can use the links in this section to access installation instructions.

| Version | Details|
|---------|--------|
| Version 8.0.5  | This version of R Server for Windows, released as Microsoft R Server 2016, is integrated with the enterprise edition of SQL Server 2016. Licensing, installation, and support of this version of R Server for Windows is through SQL Server. Using SQL Server setup, you can install R Server as a standalone server, or as multi-instance service within SQL Server. For more information, see [R Server Standalone](https://msdn.microsoft.com/library/mt671127.aspx) and [SQL Server R Services - R Server install page](https://msdn.microsoft.com/library/mt671127.aspx) in SQL Server 2016 technical documentation.|
| Version 8.0 | Installation of this version is package by package, in a specific order. For more information and instructions, see [Install Revolution R Enterprise 2016 (version 8.0) for Windows](rserver-install-windows-800.md).|

## See Also

[Supported platforms](rserver-install-supported-platforms.md)

[What's new in R Server](notes/r-server-notes.md)

[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)
