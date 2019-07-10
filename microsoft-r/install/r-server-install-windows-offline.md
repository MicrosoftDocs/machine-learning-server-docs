---

# required metadata
title: "Offline install for Microsoft R Server 9.1 for Windows"
description: "How to install R Server 9.1 without an internet connection"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "04/19/2017"
ms.topic: "conceptual"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Offline installation for R Server 9.1 for Windows

**Looking for the latest release? See [Machine Learning Server for Windows installation](machine-learning-server-windows-offline.md)**

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

In this release, most components required for R Server installation are embedded, which means fewer prerequisites have to be downloaded in advance. The following components are now included in the Windows installer:

* Microsoft .NET Core 1.1
* Microsoft MPI 7.1
* AS OLE DB (SQL Server 2016) provider
* Microsoft R Open 3.3.3
* Microsoft Visual C++ 2013 Redistributable
* Microsoft Visual C++ 2015 Redistributable

## System requirements

+ Operating system must be a [supported version of Windows](r-server-install-supported-platforms.md) on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

+ Memory must be a minimum of 2 GB of RAM is required; 8 GB or more are recommended.

+ Disk space must be a minimum of 500 MB.

+ .NET Framework 4.5.2 or later. 

<a name="download"><a/>

## Download required components

Without an internet connection, the following components must be downloaded to a separate device and transferred to the target machine.

| Component | Description | Download Link |
|-----------|-------------|---------------|
| SRO_3.3.3.0_1033.cab | Microsoft R Open | https://go.microsoft.com/fwlink/?LinkID=842800 |
| MLM_9.1.0.0_1033.cab | Machine learning models | https://go.microsoft.com/fwlink/?LinkID=845098 |

<a name="download"><a/>

## Download R Server installer

You can get the zipped installation file from one of the following download sites.

| Site | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](https://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios.|
|[Volume Licensing Service Center (VLSC)](https://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for "SQL Server 2016 Enterprise edition", and then choose a per-core or CAL licensing option. A selection for **R Server for Windows 9.0.1** is provided on this site. |

From [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/):

1. Click **Join or access now** to sign up for download benefits.
2. Check the URL to verify it changed to *https://my.visualstudio.com/*.
3. Click **Downloads** to search for R Server.
4. Click **Downloads** for a specific version to select the platform.

![Download page on Visual Studio benefits page](./media/mlserver-install-older-versions.png)

## Transfer files

Use a flash drive or another mechanism to transfer the following to the offline server. 

+ SRO_3.3.3.0_1033.cab
+ MLM_9.1.0.0_1033.cab 
+ en_microsoft_r_server_910_for_windows_x64_10324119.zip

Put the CAB files in the setup user's temp folder: `C:\Users\<user-name>\AppData\Local\Temp`. 

## Run RServerSetup

If you previously installed version 9.0.1, it will be replaced with the 9.1 version. An 8.x version can run side-by-side 9.x, unaffected by the new installation.

Unzip the installation file en_microsoft_r_server_910_for_windows_x64_10324119.zip, navigate to the folder containing RServerSetup.exe, and then run setup.

1. Double-click **RServerSetup.exe** to start the wizard.
2. In Configure installation, choose optional components. Required components are listed, but not configurable. Optional components include:
    + R Server (Standalone)
    + [Pre-trained Models](microsoftml-install-pretrained-models.md) used for machine learning.
3. In an offline installation scenario, you are notified about missing requirements, given a URL for obtaining the CAB files using an internet-connected device, and a folder path for placing the files. 
4. Accept the SQL Server license agreement for R Server <sup>1</sup>, as well as the license agreement for Microsoft R Open.
5. Optionally, change the home directory for R Server.
5. At the end of the wizard, click **Install** to run setup.

<sup>1</sup> R Server is licensed as a SQL Server enterprise feature, even though it can be installed independently of SQL Server on a Windows operating system.

## View log files

Post-installation, you can review log files (RServerSetup_<timestamp>.log) located in the system temp directory. An easy way to get there is typing `%temp%` as a Run command or search operation in Windows.

## Connect and validate

R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like RTVS or Rgui.exe connects to the server.

As a verification step, you can connect to the server and execute a few ScaleR functions to validate the installation.

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
| Microsoft R Open (MRO) | An open-source distribution of the base R language, plus the Intel Math Kernel library (int-mkl). The distribution includes standard libraries, documentation, and tools like R.exe and RGui.exe. <br/><br/>Tools for the standard base R (RTerm, Rgui.exe, and RScript) are under `<install-directory>\bin`. Documentation is under `<install-directory>\doc` and in `<install-directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options. |
| Microsoft R Server proprietary libraries and script engine | MRS packages provide libraries of functions. MRS libraries are co-located with R libraries in the `<install-directory>\library` folder. Libraries include RevoScaleR, MicrosoftML, mrsdeploy, olapR, RevoPemaR, and others listed in [Package Reference](../r-reference/introducing-r-server-r-package-reference.md). <br/><br/>On Windows, the default R Server installation directory is `C:\Program Files\Microsoft\R Server\R_SERVER`. <br/><br/>R Server is engineered for distributed and parallel processing for all multi-threaded functions, utilizing available cores and disk storage of the local machine. R Server also supports the ability to transfer computations to other R Server instances on other platforms through compute context instructions. |
| [Admin tool](../operationalize/configure-use-admin-utility.md) | Used for enabling remote execution and web service deployment, operationalizing analytics, and configuring web and compute nodes.| 
| [Pretrained models](microsoftml-install-pretrained-models.md) | Used for sentiment analysis and image featurization. | 

> [!NOTE]
> By default, telemetry data is collected during your usage of R Server. To turn this feature off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.

## See Also

 [Introduction to R Server](../what-is-microsoft-r-server.md) 
 [What's New in R Server](../whats-new-in-r-server.md)
 [Supported platforms](r-server-install-supported-platforms.md)  
 [Known Issues](../resources-known-issues.md)  
 [Command line installation](r-server-install-windows-command-line.md)  
 [Microsoft R Getting Started Guide](../microsoft-r-getting-started.md)    
 [Configure R Server to operationalize analytics](operationalize-r-server-one-box-config.md)
