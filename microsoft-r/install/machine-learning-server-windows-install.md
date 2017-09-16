---
# required metadata
title: "Install Machine Learning Server for Windows"
description: "How to install, connect to, and use Machine Learning Server on computers running the Windows operating system."
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

# Install Machine Learning Server for Windows

Machine Learning Server for Windows runs machine learning and data mining solutions written in R in standalone and clustered topologies. 

This article explains how to install Machine Learning Server 9.2.1 on a standalone Windows server that has an internet connection. If your server has restrictions on internet access, see [offline installation](machine-learning-server-windows-offline.md). 

> [!Note]
> Python support is new in this release. Although you can add Python, local script that calls [proprietary Python functions](../python-reference/introducing-python-package-reference.md) must execute on [SQL Server 2017 Machine Learning Server with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-service), or [Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md) in a Spark [compute context](../r/concept-what-is-compute-context.md). <br/><br/>On Windows, Python operations are limited to generic Python script, pushing any proprietary function calls to remote instances. You can also run Machine Learning Server [web services](../operationalize/concept-what-are-web-services) that contain  compiled Python script. More capability, including interactive Python sessions and direct execution, is projected for future releases.

## System requirements

+ Operating system must be a [supported version of 64-bit Windows](r-server-install-supported-platforms.md). 

+ Memory must be a minimum of 2 GB of RAM is required; 8 GB or more are recommended. Disk space must be a minimum of 500 MB.

+ .NET Framework 4.5.2 or later. The installer checks for this version of the .NET Framework and provides a download link if it's missing. A computer restart is required after a .NET Framework installation.

The following additional components are included in Setup and required for Machine Learning Server on Windows.

* Microsoft .NET Core 1.1
* Microsoft MPI 7.1
* AS OLE DB (SQL Server 2016) provider
* Microsoft Visual C++ 2015 Redistributable
* Microsoft R Open 3.4.1 (if you install R Server)

## Running setup on existing installations

The installation path for Machine Learning Server is new: \Program Files\Microsoft\ML Server. However, if R Server 9.x is present, Machine Learning Server 9.2.1 finds R Server at the old path and upgrades it to the new version. 

There is no support for side-by-side installations of older and newer versions, nor is there support for hybrid versions (such as R Server 9.1 and Python 9.2.1). An installation is either entirely 9.2.1 or an earlier version.

## How to install

This section walks you through a Machine Learning Server 9.2.1 deployment using the standalone Windows installer. Under these instructions, your installation will be licensed and serviced as a SQL Server add-on feature.

### 1. Download Machine Learning Server installer

You can get the zipped installation file from one of the following download sites.

| Site | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios. <br/><br/>1. Click **Join or Access Now** and enter your account information.<br/>2. Make sure you're in the right place: *my.visualstudio.com*.<br/>3. Click **Downloads**, and then search for *Machine Learning Server*. |
| [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for "SQL Server 2017 Enterprise edition" <sup>1</sup>, and then choose a per-core or CAL licensing option. A selection for **Machine Learning Server 9.2.1** is provided on this site. |
| [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) | Developer or Enterprise | Subscribers can download software at given subscription levels. Depending on your subscription, you can get either edition. |

<sup>1</sup> Machine Learning Server for Windows is licensed as a SQL Server enterprise feature, even though it's installed independently of SQL Server on a Windows operating system.

### 2. Run Setup

The setup wizard installs, upgrades, and uninstalls all in one workflow.

1. In the Downloads folder, right-click to extract the contents of zipped executable.
2. Double-click **ServerSetup.exe** to start the wizard.
3. In Configure installation, choose components to install:

    + **R Server (Standalone)**. You should check this option. On Windows, most functionality is R-related. **If you clear a checkbox for R Server on a computer that has a previous release, Setup uninstalls your existing R Server instance.**  
    + [**Pre-trained Models**](microsoftml-install-pretrained-models.md) used for image classification and sentiment detection (install the models with R or Python, but not as a standalone component).
    + **Python** adds the Python libraries. You can write local script that calls Python, but your script must set a compute context for Python execution on a SQL Server 2017 instance or Spark cluster that has the interpreter.
    + Other components are listed, but not configurable. These components are required and listed in Setup for visibility.

4. Accept the SQL Server license agreement for Machine Learning Server, as well as the license agreements for Microsoft R Open, Anaconda, and Python.
5. Optionally, change the home directory for Machine Learning Server.
6. At the end of the wizard, click **Install** to run setup.

> [!NOTE]
> By default, telemetry data is collected during your usage of Machine Learning Server. To turn this feature on or off, see [Opting out of data collection](../resources-opting-out.md).

### 3. Check log files

Post-installation, you can check the log files located in the system temp directory. An easy way to get there is typing `%temp%` as a Run command or search operation in Windows. If you installed all components, your log file list looks similar to this screenshot:

  ![Machine Learning Server setup log files](./media/mlserver-setup-log-files.png)

<a name="connect-validate"></a>

### 4. Connect and validate

Machine Learning Server executes on demand as R Server or as a Python application. As a verification step, connect to each application and run a script or function.

**For R**

R Server runs as a background process, as **Microsoft R Server Engine** in Task Manager. Server startup occurs when a client application like [R Tools for Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation) or Rgui.exe connects to the server.

1. Go to C:\Program Files\Microsoft\ML Server\R_SERVER\bin\x64.
2. Double-click Rgui.exe to start the R Console application.
3. At the command line, type `search()` to show preloaded objects, including the `RevoScaleR` package. 
4. Type `print(Revo.version)` to show the software version.
5. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`. 

**For Python**

Python runs when you execute a .py script or run commands in a Python console window. Machine Learning Server for Hadoop (Spark) and SQL Server Machine Learning Server have the Python interpreters for our proprietary Python libraries. On Windows, Setup adds Anaconda 4.2 with Python 3.5. You can write Python script that executes base functionality locally, and proprietrary Python calls on SQL or Spark.

1. Go to C:\Program Files\Microsoft\ML Server\PYTHON_SERVER.
2. Double-click **Python**.
3. At the command line, type `help()` to open interactive help.
4. Type ` revoscalepy` at the help prompt, followed by `microsoftml` to print the function list for each module.

### 5. Enable server to host analytic web services and accept remote connections

Machine Learning Server can be used as-is with an R IDE on the same box, but you can also [enable the server to host web services and to allow remote server connections](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization)

Configure the server to by running the [Administrator Utility](../operationalize/configure-use-admin-utility.md) to configure the server for remote access and execution, web service deployment, or cluster topologies. 

[Remote execution](../r/how-to-execute-code-remotely.md) makes the server accessible to client workstations running R Client or the Python client libraries on your network. Configuration steps are few and the benefit is big, so please take a few minutes to complete this task.

## What's Installed with Machine Learning Server

An installation of Machine Learning Server includes some or all of the following components.

| Component | Description |
|-----------|-------------|
| Microsoft R Open (MRO) | An open source distribution of the base R language, plus the Intel Math Kernel library (int-mkl). The distribution includes standard libraries, documentation, and tools like R.exe and RGui.exe. <br/><br/>Tools for the standard base R (RTerm, Rgui.exe, and RScript) are under `<install-directory>\bin`. Documentation is under `<install-directory>\doc` and in `<install-directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options. |
| Microsoft R Server proprietary libraries and script engine | MRS packages provide libraries of functions. MRS libraries are co-located with R libraries in the `<install-directory>\library` folder. Libraries include RevoScaleR, MicrosoftML, mrsdeploy, olapR, RevoPemaR, and others listed in [Package Reference](../r-reference/introducing-r-server-r-package-reference.md). <br/><br/>On Windows, the default R Server installation directory is `C:\Program Files\Microsoft\ML Server\R_SERVER`. <br/><br/>R Server is engineered for distributed and parallel processing for all multi-threaded functions, utilizing available cores and disk storage of the local machine. R Server also supports the ability to transfer computations to other R Server instances on other platforms through compute context instructions. |
| Python proprietary libraries | Propietary packages provide modules of class objects and static functions. Python libraries are in the `<install-directory>\lib\site-packages` folder. Libraries include revoscalepy, microsoftml, and azureml-model-management-sdk. <br/><br/>On Windows, the default installation directory is `C:\Program Files\Microsoft\ML Server\PYTHON_SERVER`.  |
| Anaconda 4.2 with Python 3.5.2 | An open source distribution of Python.|
| [Admin tool](../operationalize/configure-use-admin-utility.md) | Used for enabling remote execution and web service deployment, operationalizing analytics, and configuring web and compute nodes.| 
| [Pretrained models](microsoftml-install-pretrained-models.md) | Used for sentiment analysis and image detection. |

Consider adding a development tool on the server to build script or solutions using R Server features:

+ [Visual Studio 2017](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://docs.microsoft.com/visualstudio/rtvs/installation) and the [Python for Visual Studio (PTVS)](https://www.visualstudio.com/vs/python/).

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

### See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md) 
+ [R Function Reference](../r-reference/introducing-r-server-r-package-reference.md)
+ [Python Function Reference](../python-reference/introducing-python-package-reference.md)