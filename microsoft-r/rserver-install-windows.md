---

# required metadata
title: "Run Microsoft R Server for Windows"
description: "How to install, connect to, and use Microsoft R Server on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "02/01/2017"
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

# Run Microsoft R Server for Windows

Microsoft R Server is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers and clusters. The server runs on a wide range of computing platforms, including Microsoft Windows. 

For a description of R Server components, benefits, and usage scenarios, see [Introduction to R Server](rserver.md). To learn more about features in the latest release, see [What's New in R Server](rserver-whats-new.md).

## Licensing, installation options, and support

How you install R Server determines the support policy, location of R binaries, and the availability of certain features. 

**Support Policy** 

Two service plans are available for R Server for Windows:

+ **SQL Server support policy** offers updates and customer support on a longer servicing plan, but with less frequent feature updates, per the SQL Server release schedule. 
+ **Modern Lifecycle** is concentrated over a two year period. It delivers fixes and features sooner.

To get a specific policy, use an installer that configures your server to use it. The following table summarizes installers and service plan combinations. 

| Installer | Service plan | Benefits |
|-----------|--------------|----------|
|[Install R Server for Windows using a standalone Windows installer](#howtoinstall) | [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) | Faster turnaround of new feature releases. |
|[Install SQL Server R Services (In-database) as part of a SQL Server Database engine instance](https://msdn.microsoft.com/library/mt604845.aspx) | SQL Server support policy<sup>1</sup> or [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) <sup>2</sup> | Integration with the database engine.   |
|[Install R Server (Standalone) using the SQL Server installer](https://msdn.microsoft.com/library/mt674874.aspx) | SQL Server support policy<sup>1</sup> | Not integrated with the database engine. Choose this option if you want a standalone server with the SQL Server support policy<sup>3</sup>. |

<sup>1</sup> For details about SQL Server support, go to **[Modern Lifecycle Support](https://support.microsoft.com/en-us/lifecycle) > Search for products**, and then enter "SQL Server 2016" as the search term.

<sup>2</sup> You can [unbind an existing R Services instance from the SQL Server support plan](https://msdn.microsoft.com/library/mt791781.aspx) and rebind it to Modern Lifecycle. The terms and duration of your license is the same. The only difference is that under Modern Lifecycle, you would adopt newer versions of R Server at a faster cadence than what is typical for SQL Server deployments.

<sup>3</sup> You can provision an Azure virtual machine running Windows that has SQL Server R Server (Standalone) already installed. This VM is provisioned under the SQL Server service plan, but you could rebind to the Modern Lifecycle support policy. For more information, see [Provision an R Server Virtual Machine](https://msdnstage.redmond.corp.microsoft.com/library/mt759780.aspx).

> [!NOTE]
> Licensing is the same regardless of how you install the server. As an enterprise feature, R Server on a Windows computer is licensed under the SQL Server enterprise license agreement. In a development context, developers and data scientists can install the free developer edition, which delivers the same features as enterprise, but is licensed for smaller developer workloads. Each edition is available through different [download channels](#downloads).

**R binaries** 

The Windows installer and SQL Server installer create different library folder paths for the R packages. This is something to be aware of when using tools like R Tools for Visual Studio (RTVS) or RStudio, both of which retain library folder location references.

| File location | Installer |
|---------------|-----------|
|C:\Program Files\Microsoft\R Server\R_SERVER | Windows installer |
|C:\Program Files\Microsoft SQL Server\130\R_SERVER | SQL Server Setup, R Server (Standalone) |
|C:\Program Files\Microsoft SQL Server\<instance_name>\R_SERVICES | SQL Server Setup, R Services (In-Database) |

**Feature availability**

On Windows, R Server [operationalization](operationalize/about.md) is available right now if you use the standalone Windows installer. It is not yet available if you use the SQL Server installer. Projected availability through a SQL Server installer is the first half of 2017.

<a name="howtoinstall"></a>
## How to install R Server 9.0.1 on Windows using the standalone Windows installer

As noted, using the standalone windows installer, R Server for Windows is serviced under the [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) and includes [operationalization](operationalize/about.md).

### Prerequisites

+ A supported version of Windows. For an up-to-date list, see [Supported platforms](rserver-install-supported-platforms.md).

+ **.NET Framework 4.5.2** or later. The installer checks for this version of the .NET Framework and provides a download link if you need to install it first. A computer restart is required after the .NET Framework is installed.

+ You must accept the end user agreement. This agreement explains that R Server is licensed as a SQL Server enterprise feature, even though it can be installed independently of SQL Server on a Windows operating system.

+ You must agree to an installation of **R Open**. R Open is Microsoft's distribution of packages providing the R language and base functions. It is fully compatible with open source R and the R language, but includes performance optimizations that make R Open a better choice for R Server operations. Setup installs R Open for you, but it is also downloadable from the [MRAN web site](https://mran.microsoft.com/download/).

The following additional components are installed by Setup and required for an R Server installation on Windows.

| Component | Version |
|-----------|---------|
| Microsoft AS OLE DB Provider for SQL Server 2016 | 13.0.1601.5 |
| Microsoft .NET Core | 1.0.1 |
| Microsoft MPI | 7.1.12437.25 |
| Microsoft Visual C++ 2013 Redistributable | 12.0.30501.0 |
| Microsoft Visual C++ 2015 Redistributable Update 3 | 14.0.23026.0 |

<a name="Download"><a/>
Get the rserversetup.zip or rserversetup.exe file from one of these locations: 

**Option 1: [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx)**

Subscribers can download software at given subscription levels. Depending on your subscription, you can get the developer or enterprise edition.

**Option 2: [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409)** 

This option provides the enterprise edition. Sign in, search for "SQL Server 2016 Enterprise edition", and then choose a per-core or CAL licensing option. A selection for **R Server for Windows 9.0.1** is provided on this site.**

**Option 3: [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409)** 

This option provides a zipped file, free to developers who sign up for Visual Studio Dev Essentials. This is the Developer edition of Microsoft R Server; it has the same features as Enterprise except it is licensed for development scenarios.

1. Click **Join or Access Now** and enter your account information.
2. Make sure you're in the right place. The URL should start with *my.visualstudio.com*.
3. Click **Downloads**, and then search for *Microsoft R*.

<a name="Run-Setup"></a>
### Run Setup

RServerSetup.exe checks for prerequisites, prompts for acceptance of user agreements, and gives you the option of choosing a different program directory. At the end of the wizard, you click **Run** to install R Server and required components.

Post-installation, you can review log files (RServerSetup_<timestamp>.log) located in your system temp directory. An easy way to get there is typing %temp% as a Run command or search operation in Windows.

Optionally, consider adding a development tool on the server to build script or solutions using R Server features. We recommend either one of the following development environments:

+ [Visual Studio 2015](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://www.visualstudio.com/vs/rtvs/)
+ [Visual studio 2017 RC](https://www.visualstudio.com/vs/visual-studio-2017-rc/), which has built-in R tool support

<a name="connect-validate"></a>
### Connect and validate installation
R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like RTVS or Rgui.exe connects to the server.

As a verification step, you can connect to the server and execute a few ScaleR functions to validate the installation.

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to show preloaded objects, including the `RevoScaleR` package. 
4. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`. 

Additionally, run the [Administrator Utility](operationalize/admin-utility.md) to configure your R Server for remote access and execution, web service deployment, or multi-server installation.

## Configure R Server for operations

The server can be used as-is if you install and use an R IDE on the same box, but to benefit from Microsoft R Server’s deployment and operationalization features, you must [configure R Server for operationalization](operationalize/configuration-initial.md) after installation to act as a deployment server and host analytic web services. Doing so will enable you to operationalize your R code. It also enables remote execution, allowing you to connect to R Server from an R Client workstation and execute code on the server.

## Side-by-side installation

You can install R Server 9.0.1 and previous major versions side-by-side on the same computer, but you can only install one copy of each major version. As a standalone server, R Server for Windows is not multi-instance. If you require multiple copies of R Server at the same functional level on a single server, you can install SQL Server R Services as part of a multi-instance relational database engine service.

## Offline installation and firewall considerations

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can download individual components on a computer that has internet access, copy the files to another computer behind the firewall, manually install each component, and then run setup. For instructions, see [Offline installation](rserver-install-windows-offline.md).

## Install earlier versions of R Server for Windows

Earlier versions are supported, but with limited availability on Microsoft download sites. If you already have one of the older supported versions, you can use the links in this section to access installation instructions.

| Version | Details|
|---------|--------|
| Version 8.0.5  | This version of R Server for Windows, released as Microsoft R Server 2016, is integrated with the enterprise edition of SQL Server 2016. Licensing, installation, and support of this version of R Server for Windows is through SQL Server. Using SQL Server setup, you can install R Server as a standalone server, or as multi-instance service within SQL Server. For more information, see [R Server for Windows](https://msdn.microsoft.com/library/mt671127.aspx) and [SQL Server R Services - R Server install page](https://msdn.microsoft.com/library/mt671127.aspx) in SQL Server 2016 technical documentation.|
| Version 8.0 | Installation of this version is package by package, in a specific order. For more information and instructions, see [Install Revolution R Enterprise 2016 (version 8.0) for Windows](rserver-install-windows-800.md).|

## See Also

[Supported platforms](rserver-install-supported-platforms.md)

[What's new in R Server](notes/r-server-notes.md)

[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)

[Configure R Server for Operationalization](operationalize/configuration-initial.md)
