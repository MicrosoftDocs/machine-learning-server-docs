---

# required metadata
title: "Offline install for Microsoft R Server for Windows"
description: "How to install R Server without an internet connection"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/09/2017"
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

# Offline installation instructions for R Server for Windows

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can download files, transfer files to an offline server, manually install each prerequisite, and then install R Server.

<a name="download"><a/>
## Download prerequisites

| Component | Version | Download Link |
|-----------|---------|--------|
| .NET Framework | 4.5.2 | https://www.microsoft.com/net/download/framework |
| Microsoft R Open | 3.3.2 | [MRAN web site](https://mran.microsoft.com/download/) |
| Microsoft AS OLE DB Provider for SQL Server 2016 | 13.0.1601.5 | https://go.microsoft.com/fwlink/?linkid=834405 |
| Microsoft .NET Core | 1.0.1 | https://go.microsoft.com/fwlink/?linkid=834319 |
| Microsoft MPI | 7.1.12437.25 | https://go.microsoft.com/fwlink/?linkid=834316 |
| Microsoft Visual C++ 2013 Redistributable | 12.0.30501.0 | https://go.microsoft.com/fwlink/?linkid=799853 |
| Microsoft Visual C++ 2015 Redistributable Update 3 | 14.0.24123 | https://www.microsoft.com/en-us/download/details.aspx?id=52685 |
| SRO_3.3.2.0_1033.cab | none | http://go.microsoft.com/fwlink/?LinkID=834568 |

<a name="download"><a/>
## Download R Server installer

Get the zipped RServerSetup installer file from one of the following download sites.

| Site | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios. <br/><br/>1. Click **Join or Access Now** and enter your account information.<br/>2. Make sure you're in the right place: *my.visualstudio.com*.<br/>3. Click **Downloads**, and then search for *Microsoft R*. |
|[Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for "SQL Server 2016 Enterprise edition", and then choose a per-core or CAL licensing option. A selection for **R Server for Windows 9.0.1** is provided on this site. |
| [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) | Developer or Enterprise | Subscribers can download software at given subscription levels. Depending on your subscription, you can get either edition. |

## Check files

After downloading prerequisites and the R Server installer, you should have all of these files:

    vcredist_x64.exe ** redistributable for Visual Studio 2013 C++
    vc_redist.x64.exe ** redistributable for Visual Studio 2015 C++
    DotnetCore.1.0.1-Runtime-x64.exe`
    NDP452-KB2901954-Web.exe
    SQL_AS_OLEDB.msi
    microsoft-r-open-3.3.2.msi
    MSMpiSetup.exe
    SRO_3.3.2.0_1033.cab
    en_r_server_901_for_windows_X64_9649035.zip ** contains RServerSetup

## Transfer files

Use a flash drive or another mechanism to transfer files listed above to the offline server. Put all files in the same folder.

## Install prerequisites

Manually install the prerequisites, prior to unzipping and running RServerSetup. Installation order is important. Begin at the top of list, starting with vcredist_x64 and work your way down. Restarts may be required.

    vcredist_x64.exe ** redistributable for Visual Studio 2013 C++
    vc_redist.x64.exe ** redistributable for Visual Studio 2015 C++
    DotnetCore.1.0.1-Runtime-x64.exe`
    NDP452-KB2901954-Web.exe
    SQL_AS_OLEDB.msi
    microsoft-r-open-3.3.2.msi
    MSMpiSetup.exe

Installation of the .NET Framework requires a restart.

Ignore the .cab and .zip file. You will use them in the next step.

## Unzip setup and copy .cab

1. Right-click en_r_server_901_for_windows_X64_9649035.zip > **Extract All** to unpack the files. Create or choose the folder to store the files.
2. Copy SRO_3.3.2.0_1033.cab to the subfolder containing RServerSetup.exe. After unpacking the files, the folder containing  RServerSetup.exe is **MRS90Windows**.
3. Copy SRO_3.3.2.0_1033.cab to the temp folder: \Users\Admin\AppData\Local\Temp\. An easy way to find the Temp directory is to type `%temp%` in the Cortana "Ask me anything" search bar.

Copying the .cab file a second time to the Temp folder is a workaround measure that allows setup to continue. 

There are multiple Temp folders on a Windows computer, so if you get an installation error, it's possible the Temp folder is not the right one. To verify, check the setup logs (RServer_<timestamp>.log) for instances of Temp folders. The Temp folder used for the *default cache directory* is the correct folder for the .cab file.

## Run RServerSetup

Double-click `RServerSetup.exe` to start the wizard. 

## View log files

Post-installation, you can review log files (RServerSetup_<timestamp>.log) located in the system temp directory. An easy way to get there is typing %temp% as a Run command or search operation in Windows.

## Connect and validate

R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like RTVS or Rgui.exe connects to the server.

As a verification step, you can connect to the server and execute a few ScaleR functions to validate the installation.

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to show preloaded objects, including the `RevoScaleR` package.
4. Type `print(Revo.version)` to show the software version.
5. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`. 

Additionally, run the [Administrator Utility](operationalize/admin-utility.md) to configure your R Server for remote access and execution, web service deployment, or multi-server installation.

## Configure operationalization

The server can be used as-is if you install and use an R IDE on the same box, but to benefit from Microsoft R Server’s deployment and operationalization features, you must [configure R Server for operationalization](operationalize/configuration-initial.md) after installation to act as a deployment server and host analytic web services. It also enables remote execution, allowing you to connect to R Server from an R Client workstation and execute code on the server.

## See Also

[Run R Server for Windows](rserver-install-windows.md)

[Supported platforms](rserver-install-supported-platforms.md)

[What's new in R Server](notes/r-server-notes.md)

[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)

[Configure R Server for Operationalization](operationalize/configuration-initial.md)
