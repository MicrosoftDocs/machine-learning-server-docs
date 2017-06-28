---

# required metadata
title: "Introducing Microsoft R Client"
description: "Microsoft R Client"
keywords: "R Client, Microsoft R Client, Introduction, Get Started with R Client"
author: "j-martens"
manager: "jhubbard"
ms.date: "5/15/2017"
ms.topic: "get-started-article"
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
ms.technology: "r-client"
ms.custom: ""

---

# About Microsoft R Client

Microsoft R Client is a free, [community-supported](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MicrosoftR), data science tool for high performance analytics.  R Client is built on top of [Microsoft R Open](https://mran.microsoft.com/open/) so you can use any open source R package to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](../scaler-getting-started-data-import-exploration.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for RevoScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as [SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility](compatibility-with-server.md). 

You can offload heavy processing to R Server or test your analytics during their developmentYou by running your code remotely using [remoteLogin() or remoteLoginAAD()](../r/how-to-execute-code-remotely.md) from the `mrsdeploy` package. 

<div align=center><iframe src="https://channel9.msdn.com/blogs/MicrosoftR/Microsoft-Introduces-new-free-Microsoft-R-Client/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe></div>
 

## R Server vs R Client

Microsoft R Server and Microsoft R Client offer virtually identical packages, but each one targets different scenarios. R Client is intended for data scientists who create solutions that run locally. R Server is commercial software that runs on a range of platforms, at much greater scale, with infrastructure for handling major workloads, on client-server topologies that support remote access over authenticated connections. 

You can work with R Client standalone. You can also use it with R Server, where you learn and develop on R Client, and then migrate your work to R Server or execute it remotely on an R Server whenever you need the scale, support, and infrastructure of an operationalized server. 

## Get started with R Client

Getting started with Microsoft R Client is as easy as 1-2-3. Click a step to get started:
<br>
<div align=center>
<a href="#installrclient" title="Click Step 1"><img src="./media/r-client/Step1.png" width=200 /></a>&nbsp;&nbsp;
<a href="#configure-ide" title="Click Step 2"><img src="./media/r-client/Step2.png" width=200  /></a>&nbsp;&nbsp;
<a href="#try-r-client" title="Click Step 3"><img src="./media/r-client/Step3.png" width=200  /></a>&nbsp;&nbsp;
</div>

<br><a name="installrclient"></a>

### 1. Install R Client 

The first step is to download Microsoft R Client for your operating system and install it. To learn more about the supported platforms or installation steps, please see the following articles:

+ [Install Microsoft R Client on Windows](../deployr/install-on-windows.md)

+ [Install Microsoft R Client for Linux](../deployr/install-on-linux.md)

+ [Compatibility with R Server](compatibility-with-server.md)

<br><a name="configure-ide"></a>

### 2. Configure Your IDE

While R is a command line driven program, you can also use your favorite R integrated development environment (IDE) to interact with Microsoft R Client. To do so, you must point that IDE to the R Client R executable. This way, whenever you execute your R code, you'll do so using R Client and benefit from the proprietary packages installed with R Client.  R IDE options include R Tools for Visual Studio on Windows (Recommended), RStudio, or any other R development environment.

+ **Set up RTVS for R Client on Windows**: [R Tools for Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation) (RTVS) is an integrated development environment available as a free add-in for any edition of Visual Studio.  To make R Client the default R engine for RTVS, choose Change R to Microsoft R Client from the R Tools menu.  

+ **Set up RStudio for R Client on Windows or Linux**: [RStudio](https://www.rstudio.com/products/rstudio/download2/) is another popular R IDE. To make R Client the default R engine for RStudio, [update the path to R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R). For example, point to `C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64` on Windows. 

After you configure the IDE, a message appears in the console signaling that the Microsoft R Client packages were loaded.

>[!IMPORTANT]
>You can connect remotely from your local IDE to an R Server instance using [functions from the `mrsdeploy` package](../r/how-to-execute-code-remotely.md). Then, the R code you enter at the remote command line executes on the remote server. This is very convenient when you need to offload heavy processing on server or to test your analytics during their development. Your [R Server administrator must configure R Server](../install/operationalize-r-server-enterprise-config.md) for this functionality.

<br><a name="try-r-client"></a>

### 3. Try Out R Client

Now that you've installed R Client, you can start building and running some R code. Launch R on the command line or in your IDE and:

+ Run the sample R code as described in this [quickstart guide](../quickstart-r-code.md). 

+ Or, develop your own solutions using [`RevoScaleR` R package functions](../revoscaler.md), [`MicrosoftML` R package functions](../r-reference/microsoftml/microsoftml-package.md), and APIs. 

When ready, you can run that R code using R Client or even send those R commands to a [remote R Server](../r/how-to-execute-code-remotely.md) for execution if Microsoft R Server is also installed in your organization. 


## Learn More

You can learn more with these guides:

+ [Overview of Microsoft R](../index.md) 

+ [Quickstart: Running R code in Microsoft R](../quickstart-r-code.md) (example)

+ [Compatibility with R Server](compatibility-with-server.md)

+ [Diving into data analysis with Microsoft R](../r/how-to-introduction.md)

+ [RevoScaleR R package reference](../microsoft-r-tutorials.md)

+ [MicrosoftML R package reference](../r/concept-what-is-the-microsoftml-package.md)

+ [mrsdeploy R package reference](../r-reference/mrsdeploy/mrsdeploy-package.md)

+ [Execute code on remote R Server](../r/how-to-execute-code-remotely.md)
