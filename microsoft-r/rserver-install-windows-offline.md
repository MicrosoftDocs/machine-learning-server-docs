---

# required metadata
title: "Offline install for Microsoft R Server for Windows"
description: "How to install R Server without an internet connection"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "02/02/2017"
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

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can download individual components on a computer that has internet access, copy the files to another computer behind the firewall, manually install each component, and then run setup.

## Download the prerequisites

| Component | Version | Download Link |
|-----------|---------|--------|
| .NET Framework | 4.5.2 | https://www.microsoft.com/net/download/framework |
| Microsoft R Open | 3.3.2 | [MRAN web site](https://mran.microsoft.com/download/) |
| Microsoft AS OLE DB Provider for SQL Server 2016 | 13.0.1601.5 | https://go.microsoft.com/fwlink/?linkid=834405 |
| Microsoft .NET Core | 1.0.1 | https://go.microsoft.com/fwlink/?linkid=834319 |
| Microsoft MPI | 7.1.12437.25 | https://go.microsoft.com/fwlink/?linkid=834316 |
| Microsoft Visual C++ 2013 Redistributable | 12.0.30501.0 | https://go.microsoft.com/fwlink/?linkid=799853 |
| Microsoft Visual C++ 2015 Redistributable Update 3 | 14.0.24123 | https://www.microsoft.com/en-us/download/details.aspx?id=52685 |
| SRO_3.3.2.0_1033.cab| none | http://go.microsoft.com/fwlink/?LinkID=834568 |

## Download the installer

Get the rserversetup.zip or rserversetup.exe file from one of these locations: 

**Option 1: [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx)**

**Option 2: [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409)** This option provides the enterprise edition. Sign in, search for SQL Server 2016 Enterprise edition, and then choose a per-core or CAL licensing option. A selection for **R Server for Windows 9.0.1** is provided on this site.**

**Option 3: [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409)** This option provides a zipped file, free to developers who sign up for Visual Studio Dev Essentials. This is the Developer edition of Microsoft R Server; it has the same features as Enterprise except it is licensed for development scenarios.

    - Click **Join or Access Now** and enter your account information.
    - Click **Downloads**, and then search for *Microsoft R*.
    - Make sure you're in the right place before searching the **Downloads** list. The URL should start with *my.visualstudio.com*.

## Verify file downloads

After downloading all of the prerequisites and the rserversetup, your download folder should contain the following files:

`vcredist_x64.exe` ** redistributable for Visual Studio 2013 C++
`vc_redist.x64.exe` ** redistributable for Visual Studio 2015 C++
`DotnetCore.1.0.1-Runtime-x64.exe` 
`NDP452-KB2901954-Web.exe`
`SQL_AS_OLEDB.msi`
`microsoft-r-open-3.3.2.msi`
`MSMpiSetup.exe`

`SRO_3.3.2.0_1033.cab` ** download but don't install

`en_r_server_901_for_windows_X64_9649035.zip` ** contains RServerSetup

## Transfer all of the downloaded files to the target server

Use a flash drive or another mechanism to copy all of the files listed above to the offline server. Place all the files in the same folder.

## Install each individual component

+ Component downloads are self-executing. Double-click each file to begin installation. 
+ Install all of the prerequisites first, followed by R Server installer last. 
+ Installation order is important. Begin at the top of list, starting with vcredist_x64, and work your way down. Restarts are required.

Do not install the .cab file. RServerSetup.exe will take what it needs from the .cab when you run the installer.

A few of the installers use Windows SmartScreen and an internet connection to determine if an installer is legitimate. When prompted with a **Run** or **Don't Run** choice, you will need to click **Run** to continue.

## Extract the setup files and copy the .cab to the same folder as RServerSetup

In previous steps, you downloaded and then copied .zip file to the offline server. You should now extract the zipped files. In the resulting folder, copy the .cab file and place it in the same folder as the extracted setup file, RServerSetup.exe.

## Run R Server setup

Expand the folder containing `RServerSetup.exe` and double-click to start the wizard. 

Post-installation, you can review log files. Log files (RServerSetup_<timestamp>.log) can be found in your system temp directory. An easy way to navigate to the directory is to enter %temp% as a Run command or search operation.

## Connect to R Server and validate installation

R Server runs on demand as a background process, as **Microsoft R Engine** in Task Manager. Server startup occurs when a client application like RTVS or Rgui.exe connects to the server.

As a verification step, you can connect to the server and execute a few ScaleR functions to validate the installation.

1. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to view a list of objects already loaded. You should see the `RevoScaleR` package in the list. 
4. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`.


## See Also

[Run R Server for Windows](rserver-install-windows.md)

[Supported platforms](rserver-install-supported-platforms.md)

[What's new in R Server](notes/r-server-notes.md)

[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)

[Configure R Server for Operationalization](operationalize/configuration-initial.md)
