---

# required metadata
title: "Command line install of R Server 9.1 for Windows"
description: "Run unattended or scripted setup of R Server 9.1 on a Windows operating system"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/20/2017"
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

# Command line install of R Server 9.1 for Windows

This article provides syntax and examples for running RServerSetup.exe from the command line. You can use command line parameters for an internet-connected or offline installation. A command line installation requires administrator permissions.

Before you start, review the following articles for system requirements, prerequisites, download links, and steps:

+ [Install R Server 9.1. on Windows](r-server-install-windows.md) for an internet-connected installation.
+ [Offline installation](r-server-install-windows-offline.md) for a machine with no internet access.

## Command line options

You can run RServerSetup.exe from the command line with options to expose or hide the wizard, set an install mode, or specify options for adding features or using custom folder locations.

**User Interaction Options**

| Parameter | Description |
|-----------|-------------|
| `/full` | Invokes the wizard. |
| `/quiet` | Hides the wizard, running a silent installation with no interaction or prompts. EULA acceptance is automatic for both MRS and MRO. |
| `/passive` | Equivalent to `/quiet` in this release. |

 
**Install Modes**

| Parameter | Description |
|-----------|-------------|
| `/install` | Runs RServerSetup.exe in install mode, used to add R Server or the [pretrained machine learning models](microsoftml-install-pretrained-models.md)|
| `/uninstall` | Removes an existing installation of R Server. |
| `/modify` | Runs RServerSetup.exe in modify mode. Setup looks for an existing installation of R Server 9.1 and gives you options for changing an installation (for example, you could add the pretrained models, or uninstall the server). Use this option if you want to rerun (or repair) an installation. |

 
**Install Options**

| Parameter | Description |
|-----------|-------------|
| `/offline` | Instructs setup to find .cab files on the local system in the `mediadir` location. In this release, two .cab files are required: SRO_3.3.3.0_1033.cab for MRO, and MLM_9.1.0.0_1033.cab for the machine learning models.|
| `/installdir=""` | Specifies the installation directory. By default, this is C:\Program Files\Microsoft\R Server\R_SERVER. |
| `/cachedir=""` | A download location for the .cab files. By default, setup uses `%temp%` for the local admin user. Assuming an online installation scenario, you can set this parameter to have setup download the .cabs to the folder you specify. |
| `/mediadir=""` | The .cab file location setup uses to find .cab files in an offline installation. By default, setup uses `%temp%` for local admin. |
| `/models` | Adds the [pretrained machine learning models](microsoftml-install-pretrained-models.md). Use with `/install`.|


## Default installation

The default installation adds Microsoft R Server (MRS) and its required components: Microsoft R Open (MRO) and .NET Core used for operationalizing analytics and machine learning. The command line equivalent of a double-click invocation of RServerSetup.exe is `rserversetup.exe /install /full`.

A default installation includes the MicrosoftML package, but not the pretrained models. You must explicitly add `/models` to an installation to add this feature.

## Examples

1. Run setup in unattended mode with no prompts or user interaction to install all features, including the pretrained models.

   `rserversetup.exe /quiet /models`

2. Add the pretrained machine learning models to an existing installation. The pretrained models are inserted into the MicrosoftML package. Once installed, you cannot incrementally remove them. Removal will require uninstall and reinstall of R Server. 

   `rserversetup.exe /install /models`

3. Uninstall the software in unattended mode.

  `rserversetup.exe /quiet /uninstall`  

4. Offline install requires two .cab files that provide MRO and other dependencies. The `/offline` parameter instructs setup to look for the .cab files on the local system. By default, setup looks for the .cab files in the `%temp%` directory of local admin, but you could also set the media directory if the .cab files are in a different folder. For more information and .cab download links, see [Offline installation](r-server-install-windows-offline.md).

  `rserversetup.exe /offline /mediadir="D:/Public/CABS` 

## See Also

 [Introduction to R Server](../what-is-microsoft-r-server.md) 
 [What's New in R Server](../rserver-whats-new.md)
 [Supported platforms](r-server-install-supported-platforms.md)  
 [Known Issues](../rserver-known-issues.md)  
 [Microsoft R Getting Started Guide](../microsoft-r-getting-started.md)    
 [Configure R Server to operationalize your analytics](operationalize-r-server-one-box-config.md)
