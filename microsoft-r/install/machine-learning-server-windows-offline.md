---
# required metadata
title: "Windows offline installation for Machine Learning Server | Microsoft Docs"
description: "How to install Machine Learning Server for Windows with no internet connection."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/15/2017"
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

# Offline installation for Machine Learning Server for Windows

By default, installers connect to Microsoft download sites to get required and updated components for Machine Learning Server 9.2.1 for Windows. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

Before you start, review the following articles for requirements and restrictions:

+ [Install Machine Learning Server 9.2.1 on Windows](machine-learning-server-windows-install.md) for an internet-connected installation.
+ [Commandline installation](machine-learning-server-windows-offline.md) for a scripted installation.

## Downloads

On an internet-connected computer, download all of the following files.

<a name="file-list"></a>

| Component | Download | Used for | 
|-----------|----------|----------|
|Machine Learning Server setup | serversetup.exe from any one of the following sites:<br/><br/>[Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) <br/><br/> [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) <br/><br/> [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) | R Server |
|NameForMLM |[MLM_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852727) | Pre-trained models, R or Python |
|Microsoft R Open |[SRO_3.4.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852724) | R |
|Microsoft Python Open |[SPO_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852723) | Python |
|Microsoft Python Server |[SPS_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852726) | Python |

## Transfer and place files

Use a tool or device to transfer the files the offline server. Extract the zipped executable for setup. Place files in the following locations:

+ Put the serversetup.exe in a convenient folder. It is not important where this file resides.
+ Put the CAB files in the setup user's temp folder: `C:\Users\<user-name>\AppData\Local\Temp`. 

## Run setup

After files are placed, use the wizard or run setup from the commandline:

+ [Install Machine Learning Server > How to install > Run setup](machine-learning-server-windows-install.md#how-to-install)
+ [Commandline installation](machine-learning-server-windows-commandline.md)

## Check log files

Post-installation, you can check the log files located in the system temp directory. An easy way to get there is typing `%temp%` as a Run command or search operation in Windows. If you installed all components, your log file list looks similar to this screenshot:

  ![Machine Learning Server setup log files](./media/mlserver-setup-log-files.png)

## Connect and validate

Machine Learning Server executes on demand as R Server or as a Python application:
 
+ Python runs when you execute a .py script. 
+ R Server runs as a background process, as **Microsoft R Server Engine** in Task Manager. Server startup occurs when a client application like [R Tools for Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation) or Rgui.exe connects to the server.

As a verification step, connect to each application and run a script or function.

**For Python**

1. Go to C:\Program Files\Microsoft\ML Server\PYTHON_SERVER.
2. Double-click **Python**.
3. At the command line, type `help()` to open interactive help.
4. Type ` revoscalepy` at the help prompt, followed by `microsoftml` to print the function list for each module.

**For R**

1. Go to C:\Program Files\Microsoft\ML Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to show preloaded objects, including the `RevoScaleR` package. 
4. Type `print(Revo.version)` to show the software version.
5. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`. 

## Enable server to host analytic web services and accept remote connections

Machine Learning Server can be used as-is with an R IDE on the same box, but you can also [enable the server to host web services and to allow remote server connections](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization)

Configure the server to by running the [Administrator Utility](../operationalize/configure-use-admin-utility.md) to configure the server for remote access and execution, web service deployment, or cluster topologies. 

[Remote execution](../r/how-to-execute-code-remotely.md) makes the server accessible to client workstations running R Client or the Python client libraries on your network. Configuration steps are few and the benefit is big, so please take a few minutes to complete this task.

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

### See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)