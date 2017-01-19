---

# required metadata
title: "Run Microsoft R Server for Windows"
description: "How to install, connect to, and use Microsoft R Server on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "01/18/2017"
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

Microsoft R Server is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers and clusters. The server runs on a wide range of computing platforms, including Windows. 

For a description of R Server components, benefits, and usage scenarios, see [Introduction to R Server](rserver.md). To learn more about features in the latest release, see [What's New in R Server](rserver-whats-new.md).

## Licensing, installation options, and support

How you install R Server determines the support policy, location of R binaries, and the availability of certain features.

Licensing is the same regardless of how you install the server. As an enterprise feature, R Server on a Windows computer is licensed under the SQL Server enterprise license agreement. Another option for developers and data scientists is to install the free developer edition, which delivers the same features as enterprise, but is licensed for smaller developer workloads. Each edition is available through different [download channels](#downloads).

If your objective is running R Server on a Windows computer, there are three installers (listed below) and two service models to choose from. **SQL Server support policy** offers updates and customer support over a longer period, but feature updates are tied to the SQL Server release schedule. **Modern Lifecycle** is condensed over a two year period. Releases are delivered more frequently, which means you get fixes and new features sooner.

The following table summarizes installers and service plan combinations. 

| Installer | Service plan | Benefits |
|-----------|--------------|----------|
|[Install R Server for Windows using a standalone Windows installer](#howtoinstall) | [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) | Faster turnaround of new feature releases. |
|[Install SQL Server R Services (In-database) as part of a SQL Server Database engine instance](https://msdn.microsoft.com/library/mt604845.aspx) | SQL Server support policy<sup>1</sup> or [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) <sup>2</sup> | Integration with the database engine.   |
|[Install R Server (Standalone) using the SQL Server installer](https://msdn.microsoft.com/ibrary/mt674874.aspx) | SQL Server support policy<sup>1</sup> | Not integrated with the database engine. Choose this option if you want a standalone server with the SQL Server support policy. |

<sup>1</sup> For details about SQL Server support, go to **[Modern Lifecycle Support](https://support.microsoft.com/en-us/lifecycle) > Search for products**, and then enter "SQL Server 2016" as the search term.

<sup>2</sup> You can [unbind an existing R Services instance from the SQL Server support plan](https://msdn.microsoft.com/library/mt791781.aspx) and rebind it to Modern Lifecycle. The duration of your license is the same. The only difference is that under Modern Lifecycle, you would adopt newer versions of R Server at a faster clip than what is typical for SQL Server deployments.

**R binaries** 

The Windows installer and SQL Server installer create different folder paths for the R libraries. This is something to be aware of when using tools like R Tools for Visual Studio (RTVS) or RStudio, both of which retain library file location.

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
| Microsoft Visual C++ 2015 Redistributable | 14.0.23026.0 |

<a name="Download"><a/>
### Download R Server installer

You can download the installation program from the following locations:

+ [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx).
+ [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) enterprise edition. Sign in, search for SQL Server 2016 Enterprise edition, and then choose a per-core or CAL licensing option. A selection for **R Server for Windows 9.0.1** is provided on this site.
+ [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) provides a zipped file, free to developers who sign up for Visual Studio Dev Essentials. This is the Developer edition of Microsoft R Server; it has the same features as Enterprise except it is licensed for development scenarios.

    - Click **Join or Access Now** and enter your account information.
    - Click **Downloads**, and then search for *Microsoft R*.
    - Be sure that you are connected to Visual Studio Dev Essentials before searching the **Downloads** list. You're in the right place if the URL starts with *my.visualstudio.com*.

<a name="Run-Setup"></a>
### Run Setup

RServerSetup.exe provides an installation wizard for standalone server deployment. The installer checks for prerequisites, prompts for acceptance of user agreements, and gives you the option of choosing a different program directory. When you complete the wizard, setup installs R Server, plus additional software (noted in the prerequisites) required for operations.

Post-installation, you can review log files. Log files (RServerSetup_<timestamp>.log) can be found in your system temp directory. An easy way to navigate to the directory is to enter %temp% as a Run command or search operation.

Optionally, consider adding a development tool on the server to build script or solutions using R Server features. We recommend either one of the following development environments:

+ [Visual Studio 2015](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://www.visualstudio.com/vs/rtvs/)
+ [Visual studio 2017 RC](https://www.visualstudio.com/vs/visual-studio-2017-rc/), which has built-in R tool support

### Connect to R Server and validate installation

R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like RTVS or Rgui.exe connects to the server.

As a verification step, you can connect to the server and execute a few ScaleR functions to validate the installation.

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to view a list of objects already loaded. You should see the `RevoScaleR` package in the list. 
4. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`.

### Configure R Server for operationalization

To benefit from Microsoft R Server’s deployment and operationalization features, you can [configure R Server for operationalization](operationalize/configuration-initial.md) after installation to act as a deployment server and host analytic web services. Doing so will enable you to operationalize your R code. It also enables remote execution, allowing you to connect to R Server from an R Client workstation and execute code on the server.

## Side-by-side installation

You can install R Server 9.0.1 and previous major versions side-by-side on the same computer, but you can only install one copy of each major version. As a standalone server, R Server for Windows is not multi-instance. If you require multiple copies of R Server at the same functional level on a single server, you can install SQL Server R Services as part of a multi-instance relational database engine service.

## Offline installation and firewall considerations

By default, installers reach out to Microsoft download sites to get required and updated components. If firewall restrictions or limits on internet access prevent the installer from reaching these sites, you can download components manually on a computer that has internet access, copy the files to another computer behind the firewall, and re-run setup.

**Step 1: Download the prerequisites**

| Component | Version | Download Link |
|-----------|---------|--------|
| .NET Framework | 4.5.2 | https://www.microsoft.com/net/download/framework |
| Microsoft R Open | 3.3.2 | [MRAN web site](https://mran.microsoft.com/download/) |
| Microsoft AS OLE DB Provider for SQL Server 2016 | 13.0.1601.5 | https://go.microsoft.com/fwlink/?linkid=834405 |
| Microsoft .NET Core | 1.0.1 | https://go.microsoft.com/fwlink/?linkid=834319 |
| Microsoft MPI | 7.1.12437.25 | https://go.microsoft.com/fwlink/?linkid=834316 |
| Microsoft Visual C++ 2013 Redistributable | 12.0.30501.0 | https://go.microsoft.com/fwlink/?linkid=799853 |
| Microsoft Visual C++ 2015 Redistributable | 14.0.23026.0 | https://go.microsoft.com/fwlink/?linkid=828641 |


**Step 2: Download the installer**

Get the rserversetup.zip or rserversetup.exe file from the [download sites](#Download).

**Step 3: Run setup**

Copy all of the downloaded files to the computer behind the firewall, and then follow the instructions on [how to install the server](#Run-Setup).


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
