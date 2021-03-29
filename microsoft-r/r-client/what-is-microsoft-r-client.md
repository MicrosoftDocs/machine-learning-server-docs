---
title: "What is Microsoft R Client? - Machine Learning Server "
description: "Microsoft R Client is a free data science tool for high performance analytics. With it, you can use any open-source R package."
keywords: R Client, Microsoft R Client, Introduction, Get Started with R Client
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 02/16/2018
ms.topic: "conceptual"
ms.prod: "mlserver"
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# About Microsoft R Client

Microsoft R Client is a free, [community-supported](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MicrosoftR), data science tool for high performance analytics.  R Client is built on top of [Microsoft R Open](https://mran.microsoft.com/open/) so you can use any open-source R package to build your analytics. Additionally, R Client includes the [powerful RevoScaleR technology](../r/tutorial-revoscaler-data-import-transform.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of RevoScaleR functions, but there are some constraints. Data must fit in local memory, and processing is limited to two threads for RevoScaleR functions. To work with larger data sets or offload heavy processing, you can access a remote production instance of [Machine Learning Server](../what-is-machine-learning-server.md) from the command line or push the [compute context](../r/concept-what-is-compute-context.md) to the remote server. [Learn more about its compatibility](compatibility-with-server.md). 

<iframe src="https://channel9.msdn.com/blogs/MicrosoftR/Microsoft-Introduces-new-free-Microsoft-R-Client/player"  width="600" height="400"  allowFullScreen frameBorder="0"></iframe> 

## Machine Learning Server vs R Client

Machine Learning Server and Microsoft R Client offer virtually identical [R packages](../r-reference/introducing-r-server-r-package-reference.md#r-function-libraries), but each one targets different scenarios. R Client is intended for data scientists who create solutions that run locally. Machine Learning Server is commercial software that runs on a range of platforms, at much greater scale, with infrastructure for handling major workloads, on client-server topologies that support remote access over authenticated connections. 

You can work with R Client standalone. You can also use it with Machine Learning Server, where you learn and develop on R Client, and then migrate your work to Machine Learning Server or execute it remotely on a Machine Learning Server whenever you need the scale, support, and infrastructure of a server configured for operationalization. 

+ From R Client, shift data-centric RevoScaleR operations to a remote Machine Learning Server by creating a [remote compute context](../r/concept-what-is-compute-context.md). Remote compute context is supported for [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services) or a [Spark cluster](../install/machine-learning-server-hadoop-install.md). Typically, you shift the compute context to bring computations to where the data resides, thus avoiding data transfer over the network.
+ From R Client, run arbitrary R code on a remote production instance of Machine Learning Server. This is a general-purpose capability: from a command line, you can switch between local and remote sessions interactively, useful for testing, administration, or  to use the additional processing power of a production server. For remote code execution, use [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md)and [remoteLogin()](../r-reference/mrsdeploy/remotelogin.md) or [remoteLoginAAD()](../r-reference/mrsdeploy/remoteloginaad.md). For more information, see [Execute on a remote server ](../r/how-to-execute-code-remotely.md).

## Get started with R Client

Getting started with Microsoft R Client is as easy as 1-2-3. Click a step to get started:

 ![Step 1](./media/what-is-microsoft-r-client/Step1.png)

<a name="installrclient"></a>

### 1. Install R Client 

The first step is to download Microsoft R Client for your operating system and install it. To learn more about the supported platforms or installation steps, please see the following articles:

+ [Install Microsoft R Client on Windows](install-on-windows.md)

+ [Install Microsoft R Client for Linux](install-on-linux.md)

+ [Compatibility with Machine Learning Server & R Server](compatibility-with-server.md)

<a name="configure-ide"></a>

### 2. Configure Your IDE

While R is a command-line driven program, you can also use your favorite R integrated development environment (IDE) to interact with Microsoft R Client. To do so, you must point that IDE to the R Client R executable. This way, whenever you execute your R code, you'll do so using R Client and benefit from the proprietary packages installed with R Client.  R IDE options include RStudio, or any other R development environment.

+ **Set up RStudio for R Client on Windows or Linux**: [RStudio](https://www.rstudio.com/products/rstudio/download2/) is another popular R IDE. To make R Client the default R engine for RStudio, [update the path to R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R). For example, point to `C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64` on Windows. 

Similarly, on Linux add "R_LIBS_SITE=/opt/microsoft/rclient/3.5.2/libraries/RServer" on bottom of the file "/opt/microsoft/rclient/3.5.2/runtime/R/etc/Renviron". [Source](https://github.com/rstudio/rstudio/issues/2455#issuecomment-375327109).

After you configure the IDE, a message appears in the console signaling that the Microsoft R Client packages were loaded.

>[!IMPORTANT]
>You can connect remotely from your local IDE to an Machine Learning Server instance using [functions from the mrsdeploy package](../r/how-to-execute-code-remotely.md). Then, the R code you enter at the remote command line executes on the remote server. This is very convenient when you need to offload heavy processing on server or to test your analytics during their development. Your [Machine Learning Server administrator must configure Machine Learning Server](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) for this functionality.

<a name="try-r-client"></a>

### 3. Try Out R Client

Now that you've installed R Client, you can start building and running some R code. Launch R on the command line or in your IDE and:

+ Run the sample R code as described in this [quickstart guide](../r/quickstart-run-r-code.md). 

+ Or, develop your own solutions using [RevoScaleR functions](../r-reference/revoscaler/revoscaler.md), [MicrosoftML functions](../r-reference/microsoftml/microsoftml-package.md), and APIs. 

When ready, you can run that R code using R Client or even send those R commands to a [remote Machine Learning Server](../r/how-to-execute-code-remotely.md) for execution if Machine Learning Server is also installed in your organization. 


<a name="r-client-whats-new"></a>

## What's new in Microsoft R Client

### Microsoft R Client 3.5.2

This release of R Client, built on open-source R 3.5.2, is at the same functional level as Machine Learning Server 9.4. Download R Client from https://aka.ms/rclient (Windows) or https://aka.ms/rclientlinux (Linux). 

### Microsoft R Client 3.4.3

This release of R Client, built on open-source R 3.4.3, is at the same functional level as Machine Learning Server 9.3. 

R Client includes these enhancements:

+ R Client (Linux) now supports a remote SQL Server compute context on Windows.
+ [sqlrutils](../r-reference/sqlrutils/sqlrutils.md) is now supported for R Client (Linux).

### Microsoft R Client 3.4.1

This release of R Client, built on open-source R 3.4.1, is at the same functional level as Machine Learning Server 9.2.1. For more information about features in that release, see [here](../whats-new-in-machine-learning-server.md#921).

### Microsoft R Client 3.3.3

This release of R Client, built on open-source R 3.3.3. Key release features include:

+ Installations on Linux are now possible for R Client

+ Several of the packages installed have been updated. Learn more about [here](../whats-new-in-r-server.md).

### Microsoft R Client 3.3.2

The following release notes apply to Microsoft R Client, which can be downloaded from https://aka.ms/rclient/download.

+ New enhanced Microsoft R Client [Getting Started](what-is-microsoft-r-client.md) guide
+ A new 'version check' and auto-update is now possible using CheckForUpdates() in your R Console 
+ [Offline installations](what-is-microsoft-r-client.md#installrclient) are now possible for R Client
+ New packages now bundled with Microsoft R Client:
  + `mrsdeploy` - adds remote execution and web service deployment from R Client 3.3.2 to a remote R Server 9.0.1 instance
  + `MicrosoftML` - adds machine learning algorithms to R script that executes on either R Client or R Server
  + `olapR`- adds MDX query support through connections to OLAP cubes on a SQL Server 2016 Analysis Services instance
  + and the packages bundled with [Microsoft R Open 3.3.2](https://mran.microsoft.com/rro/installed/#enhance)
  
  Learn more about [the new and updated packages in this release](../whats-new-in-r-server.md).
+ Updated end-user license agreement
+ Telemetry collection is now enabled. [Learn more](../resources-opting-out.md) about this feature and how to turn it off.



## Learn More

You can learn more with these guides:

+ [Quickstart: Running R code in Microsoft R](../r/quickstart-run-r-code.md) (example)

+ [Compatibility with Machine Learning Server](compatibility-with-server.md)

+ [How-to guides in Machine Learning Server](../r/how-to-introduction.md)

+ [RevoScaleR R package reference](../r/tutorial-introduction.md)

+ [MicrosoftML R package reference](../r-reference/microsoftml/microsoftml-package.md)

+ [mrsdeploy R package reference](../r-reference/mrsdeploy/mrsdeploy-package.md)

+ [Execute R code on remote Machine Learning Server](../r/how-to-execute-code-remotely.md)
