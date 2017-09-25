---

# required metadata
title: "Install R Server 9.1 for Windows"
description: "How to install, connect to, and use Microsoft R Server 9.1 on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "06/29/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# Install R Server 9.1 for Windows

**Looking for the 9.2.1 release? See [Machine Learning Server for Windows installation](machine-learning-server-windows-install.md)**

Microsoft R Server is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers and clusters. The server runs on a wide range of computing platforms, including Microsoft Windows.

This article explains how to install Microsoft R Server 9.1 on a standalone Windows server that has an internet connection. If your server has restrictions on internet access, see [offline installation](r-server-install-windows-offline.md). 

If you previously installed version 9.0.1, it will be replaced with the 9.1 version. An 8.x version can run side-by-side 9.x, unaffected by the new installation.

## System requirements

+ Operating system must be a [supported version of Windows](r-server-install-supported-platforms.md) on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium chips (also known as IA-64) are not supported. Multiple-core chips are recommended. 

+ Memory must be a minimum of 2 GB of RAM is required; 8 GB or more are recommended.

+ Disk space must be a minimum of 500 MB.

+ .NET Framework 4.5.2 or later. The installer checks for this version of the .NET Framework and provides a download link if it's missing. A computer restart is required after a .NET Framework installation.

The following additional components are included in Setup and required for an R Server on Windows.

* Microsoft .NET Core 1.1
* Microsoft MPI 7.1
* AS OLE DB (SQL Server 2016) provider
* Microsoft R Open 3.3.3
* Microsoft Visual C++ 2013 Redistributable
* Microsoft Visual C++ 2015 Redistributable

<a name="howtoinstall"></a>

## How to install

This section walks you through an R Server 9.1 deployment using the standalone Windows installer. Under these instructions, your installation will be serviced under the [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) and includes the ability to [operationalize your analytics](../what-is-operationalization.md).

<a name="download"><a/>

### Download R Server installer

You can get the zipped installation file from one of the following download sites.

| Site | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios. <br/><br/>1. Click **Join or Access Now** and enter your account information.<br/>2. Make sure you're in the right place: *my.visualstudio.com*.<br/>3. Click **Downloads**, and then search for *Microsoft R*. |
| [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for "SQL Server 2016 Enterprise edition", and then choose a per-core or CAL licensing option. A selection for **R Server for Windows 9.0.1** is provided on this site. |
| [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) | Developer or Enterprise | Subscribers can download software at given subscription levels. Depending on your subscription, you can get either edition. |

<a name="Run-Setup"></a>

### Run Setup

Unzip the installation file en_microsoft_r_server_910_for_windows_x64_10324119.zip, navigate to the folder containing RServerSetup.exe, and then run setup.

1. Double-click **RServerSetup.exe** to start the wizard.
2. In Configure installation, choose optional components. Required components are listed, but not configurable. Optional components include:
    + R Server (Standalone)
    + [Pre-trained Models](microsoftml-install-pretrained-models.md) used for machine learning.
3. Accept the SQL Server license agreement for R Server <sup>1</sup>, as well as the license agreement for Microsoft R Open.
4. Optionally, change the home directory for R Server.
5. At the end of the wizard, click **Install** to run setup.

<sup>1</sup> R Server for Windows is licensed as a SQL Server enterprise feature, even though it's installed independently of SQL Server on a Windows operating system.

### Log files

Post-installation, you can check the log files (RServerSetup_<timestamp>.log) located in the system temp directory. An easy way to get there is typing `%temp%` as a Run command or search operation in Windows.

<a name="connect-validate"></a>

### Connect and validate

R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like [R Tools for Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation) or Rgui.exe connects to the server.

As a verification step, connect to the server and execute a few ScaleR functions to validate the installation.

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to show preloaded objects, including the `RevoScaleR` package. 
4. Type `print(Revo.version)` to show the software version.
5. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`. 

Additionally, run the [Administrator Utility](../operationalize/configure-use-admin-utility.md) to configure your R Server for remote access and execution, web service deployment, or multi-server installation.

## Enable Remote Connections and Analytic Deployment

The server can be used as-is if you install and use an R IDE on the same box, but to benefit from the deployment and consumption of web services with Microsoft R Server, then you must configure R Server after installation to act as a deployment server and host analytic web services. Possible configurations are a [one-box setup](operationalize-r-server-one-box-config.md) or an [enterprise setup](operationalize-r-server-enterprise-config.md). Doing so also enables remote execution, allowing you to connect to R Server from an R Client workstation and execute code on the server.

## What's Installed with R Server

An installation of Microsoft R Server includes the following components.

| Component | Description |
|-----------|-------------|
| Microsoft R Open (MRO) | An open source distribution of the base R language, plus the Intel Math Kernel library (int-mkl). The distribution includes standard libraries, documentation, and tools like R.exe and RGui.exe. <br/><br/>Tools for the standard base R (RTerm, Rgui.exe, and RScript) are under `<install-directory>\bin`. Documentation is under `<install-directory>\doc` and in `<install-directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options. |
| Microsoft R Server proprietary libraries and script engine | MRS packages provide libraries of functions. MRS libraries are co-located with R libraries in the `<install-directory>\library` folder. Libraries include RevoScaleR, MicrosoftML, mrsdeploy, olapR, RevoPemaR, and others listed in [Package Reference](../r-reference/introducing-r-server-r-package-reference.md). <br/><br/>On Windows, the default R Server installation directory is `C:\Program Files\Microsoft\R Server\R_SERVER`. <br/><br/>R Server is engineered for distributed and parallel processing for all multi-threaded functions, utilizing available cores and disk storage of the local machine. R Server also supports the ability to transfer computations to other R Server instances on other platforms through compute context instructions. |
| [Admin tool](../operationalize/configure-use-admin-utility.md) | Used for enabling remote execution and web service deployment, operationalizing analytics, and configuring web and compute nodes.| 
| [pre-trained models](microsoftml-install-pretrained-models.md) | Used for sentiment analysis and image featurization. |

Consider adding a development tool on the server to build script or solutions using R Server features:

+ [Visual Studio 2015](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://docs.microsoft.com/visualstudio/rtvs/installation)

> [!NOTE]
> By default, telemetry data is collected during your usage of R Server. To turn this feature off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.

## Licensing and support

How you install R Server determines the support policy, location of R binaries, and the availability of certain features. 

**Licensing**

R Server for Windows is licensed as a SQL Server enterprise feature. Licensing is the same regardless of how you install the server (standalone, standalone as part of SQL Server, or embedded in a relational database instance as SQL Server Machine Learning Services). 

In a development context, developers and data scientists can install the free developer edition, which delivers the same features as enterprise, but is licensed for smaller developer workloads. Each edition is available through different [download channels](#download).

**Support Policy** 

Two support plans <sup>1</sup>  are available for R Server for Windows. The installation method determines which one is in effect.

| Service Plan | Details | How to get this plan |
|--------------|---------|----------------------|
|[Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912)| Requires running the latest version of R Server. | [Install R Server for Windows using a standalone Windows installer](#howtoinstall) |
| SQL Server support policy | Service updates are on the SQL Server release schedule. | [Install SQL Server Machine Learning Services (In-database) as part of a SQL Server Database engine instance](https://msdn.microsoft.com/library/mt604845.aspx) <br/> - or - <br/>[Install R Server (Standalone) using the SQL Server installer](https://msdn.microsoft.com/library/mt674874.aspx) <sup>2, </sup> <sup>3</sup>| 

<sup>1</sup> For details, go to [Microsoft Lifecycle Policy](https://support.microsoft.com/lifecycle/selectindex). Use the index to navigate to **R Server** or **SQl Server 2016**.

<sup>2</sup> You can [unbind an existing R Services instance from the SQL Server support plan](https://msdn.microsoft.com/library/mt791781.aspx) and rebind it to Modern Lifecycle. The terms and duration of your license are the same. The only difference is that under Modern Lifecycle, you would adopt newer versions of R Server at a faster cadence than what might be typical for SQL Server deployments.

<sup>3</sup> You can provision an Azure virtual machine running Windows that has SQL Server R Server (Standalone) already installed. This VM is provisioned under the SQL Server service plan, but you could rebind to the Modern Lifecycle support policy. For more information, see [Provision an R Server Virtual Machine](https://msdnstage.redmond.corp.microsoft.com/library/mt759780.aspx).

**Location of R binaries** 

The Windows installer and SQL Server installer create different library folder paths for the R packages. This is something to be aware of when using tools like R Tools for Visual Studio (RTVS) or RStudio, both of which retain library folder location references.

| File location | Installer |
|---------------|-----------|
|C:\Program Files\Microsoft\R Server\R_SERVER | Windows installer |
|C:\Program Files\Microsoft SQL Server\130\R_SERVER | SQL Server Setup, R Server (Standalone) |
|C:\Program Files\Microsoft SQL Server\<instance_name>\R_SERVICES | SQL Server Setup, R Services (In-Database) |

## Deploy at scale

As a standalone server, R Server for Windows is not multi-instance. If you require multiple copies of R Server at the same functional level on a single server, you can install [SQL Server R Services](../r/concept-what-is-sql-server-r-services.md) as part of a multi-instance relational database engine service and then use each one independently.

Another scalable topology is to install multiple R Servers, each configured as either a dedicated web node or compute node. Nodes can be clustered using NLB or Windows failover clustering. For more information, see [Operationalize your analytics](../what-is-operationalization.md).

## Install earlier versions

Earlier versions are supported, but with limited availability on Microsoft download sites. If you already have one of the older supported versions, you can use the links in this section to access installation instructions.

| Version | Details|
|---------|--------|
| Version 8.0.5  | This version of R Server for Windows, released as Microsoft R Server 2016, is integrated with the enterprise edition of SQL Server 2016. Licensing, installation, and support of this version of R Server for Windows is through SQL Server. Using SQL Server setup, you can install R Server as a standalone server, or as multi-instance service within SQL Server. For more information, see [R Server for Windows](https://msdn.microsoft.com/library/mt671127.aspx) and [SQL Server R Services - R Server install page](https://msdn.microsoft.com/library/mt671127.aspx) in SQL Server 2016 technical documentation.|
| Version 8.0 | Installation of this version is package by package, in a specific order. For more information and instructions, see [Install Revolution R Enterprise 2016 (version 8.0) for Windows](r-server-install-windows-800.md).|

## See Also

 [Introduction to R Server](../what-is-microsoft-r-server.md) 
 [What's New in R Server](../whats-new-in-r-server.md)
 [Supported platforms](r-server-install-supported-platforms.md)  
 [Known Issues](../resources-known-issues.md)  
 [Command line installation](r-server-install-windows-command-line.md)  
 [Microsoft R Getting Started Guide](../microsoft-r-getting-started.md)    
 [Configure R Server to operationalize your analytics](operationalize-r-server-one-box-config.md)
