---

# required metadata
title: "Get Started with Microsoft R Client"
description: "Microsoft R Client Getting Started Guide."
keywords: "R Client, R IDE configuration, RTVS, R Tools for Visual Studio, Microsoft R Client"
author: "j-martens"
manager: "jhubbard"
ms.date: "4/19/2017"
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

# Get Started with Microsoft R Client

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](scaler-getting-started.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is capped at two threads for RevoScaleR functions. 

To benefit from disk scalability, performance and speed, you can push the compute context using rxSetComputeContext() to a production instance of Microsoft R Server such as [SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility.](r-client-compatibility.md) 

You can also run your code remotely on R Server using [remoteLogin() or remoteLoginAAD()](/operationalize/remote-execution.md) from the `mrsdeploy` package to offload heavy processing on server or to test your analytics during their development.

**Getting started with Microsoft R Client is as easy as 1-2-3.**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click a step to get started:
<br>
<div align=center>
<a href="#installrclient" title="Click Step 1"><img src="./media/rclient/Step1.png" width=200 /></a>&nbsp;&nbsp;
<a href="#configure-ide" title="Click Step 2"><img src="./media/rclient/Step2.png" width=200  /></a>&nbsp;&nbsp;
<a href="#try-r-client" title="Click Step 3"><img src="./media/rclient/Step3.png" width=200  /></a>&nbsp;&nbsp;
</div>

<br>

**Check out this video introduction to Microsoft R Client.**

<br>

<div align=center><iframe src="https://channel9.msdn.com/blogs/MicrosoftR/Microsoft-Introduces-new-free-Microsoft-R-Client/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe></div>
 
<br><a name="installrclient"></a>

## Step 1: Install Microsoft R Client 

The first step is to download Microsoft R Client for your operating system and install it.

To learn more about the supported platforms or installation steps, please see the following articles:

+ [Download and install Microsoft R Client for Windows](r-client-install-windows.md)

+ [Download and install Microsoft R Client for Linux](r-client-install-linux.md)


<a name="configure-ide"></a>

## Step 2: Configure Your IDE

While R is a command line driven program, you can also use your favorite R integrated development environment (IDE) to interact with Microsoft R Client. 

After you've [installed R Client](#installrclient), you can configure your favorite R integrated development environment (IDE) to point to the R Client R executable. This way, whenever you execute your R code, you'll do so using R Client and benefit from the proprietary packages installed with R Client.  R IDE options include [R Tools for Visual Studio](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1) on Windows (Recommended), RStudio, or any other R development environment.<br>

>[!IMPORTANT]
>You can connect remotely from your local IDE to an R Server instance using [functions from the `mrsdeploy` package](/operationalize/remote-execution.md). Then, the R code you enter at the remote command line executes on the remote server. This is very convenient when you need to offload heavy processing on server or to test your analytics during their development. Your [R Server administrator must configure R Server](/operationalize/configure-enterprise.md) for this functionality.

**Set up RTVS for R Client on Windows**

[R Tools for Visual Studio](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1) (RTVS) is an integrated development environment available as a free add-in for any edition of Visual Studio.  

  1. After installing R Client, launch RTVS.
  1. From the R Tools, choose Change R to Microsoft R Client.
  1. When you launch RTVS, R Client is now the default R engine. In the console window, you'll see a message stating that the Microsoft R Client packages were loaded.

<br>

**Set up RStudio for R Client on Windows or Linux**

  1. After installing R Client, launch [RStudio](https://www.rstudio.com/products/rstudio/download2/).
  1. [Update the path to R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R). For example, point to `C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64` on Windows.
  1. When you launch RStudio, R Client is now the default R engine. In the console window, you'll see a message stating that the Microsoft R Client packages were loaded.

<br><a name="try-r-client"></a>

## Step 3: Try Out R Client

Now that you've installed R Client, you can start building and running your R code. Start R on the command-line or in your IDE. 

Start working with R Client by predicting flight delays as described in this [R Client Quick Start guide](microsoft-r-quick-start.md). Or, develop your own solutions with [`RevoScaleR` R package functions](/scaler/scaler.md), [`MicrosoftML` R package functions](/microsoftml/microsoftml.md), and APIs. 

When ready, you can run that R code using R Client or even send those R commands to a [remote R Server](/operationalize/remote-execution.md) for execution if Microsoft R Server is also installed in your organization. 

<br>

## Next steps

You can learn more with these guides:

+ [Quick Start for Microsoft R Client](microsoft-r-quick-start.md) 

+ [Microsoft R Getting Started](microsoft-r-getting-started.md) 

+ [Diving into Data Analysis with Microsoft R](data-analysis-in-microsoft-r.md)

+ [Get started with RevoScaleR](microsoft-r-get-started-node.md)

+ [Get started with MicrosoftML](microsoftml-get-started.md)

+ [Get started with mrsdeploy](mrsdeploy/mrsdeploy.md)
