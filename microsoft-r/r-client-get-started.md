---

# required metadata
title: "Get Started with Microsoft R Client"
description: "Microsoft R Client Getting Started Guide."
keywords: "R Client, R IDE configuration, RTVS, R Tools for Visual Studio, Microsoft R Client"
author: "j-martens"
manager: "paulette.mckay"
ms.date: "08/10/2016"
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

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility.](r-client-compatibility.md)

**Getting started with Microsoft R Client is as easy as 1-2-3.**

<!--<br>
<div align=center>
<img src="./media/rclient/easy-as-1-2-3-a.jpg" width="80%"/>
</div>-->

1. First [download](http://aka.ms/rclient/download) and [install R Client](#installrclient).
1. Then, [configure your favorite R IDE](#configure-ide) to point to the R engine for R Client.
1. And, finally, start running your R scripts in your IDE using the R Client’s engine. 

<!--[Try out our tutorial.](#try-r-client)-->

<br>
Check out this video introduction to Microsoft R Client.

<br>

<div align=center><iframe src="https://channel9.msdn.com/blogs/MicrosoftR/Microsoft-Introduces-new-free-Microsoft-R-Client/player" width="600" height="380" allowFullScreen frameBorder="0"></iframe></div>

<br><a name="installrclient"></a>

## Step 1: Install Microsoft R Client

[Download Microsoft R Client](http://aka.ms/rclient/download), available through Visual Studio Dev Essentials, and install it today. 

[!include[Install](./includes/r-client/r-client-install.md)]

<a name="configure-ide"></a>

## Step 2: Configure Your IDE

Once you've [installed R Client](#installrclient), the next step is to configure your favorite R integrated development environment (IDE) to point to the R Client R executable. This way, whenever you execute your R code, you'll do so using R Client and benefit from the proprietary packages installed with R Client.  R IDE options include [R Tools for Visual Studio](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1) (Recommended), RStudio, or any other R development environment.<br>

#### Set Up RTVS for R Client

[R Tools for Visual Studio](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1) (RTVS) is an integrated development environment available as a free add-in for any edition of Visual Studio.  

  >If you don't have RTVS installed yet, [follow these steps](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1). RTVS is only available if the supported version of Visual Studio is already installed.

  1. Launch RTVS.
  1. From the **R Tools** menu, choose **Change R to Microsoft R Client**.
  1. When you launch RTVS, R Client will now be the default R engine.

<br> 

#### Set Up RStudio for R Client

  >If you don't have RStudio installed yet, [get it here](https://www.rstudio.com/products/rstudio/download2/).

  1. Launch RStudio.
  1. [Update the path to R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R).
     1. From the **Tools** menu, choose **Global Options**.
     1. In the  **General** tab, update the path to R to point to `C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64`.
  1. When you launch RStudio, R Client will now be the default R engine.
   
<br><a name="try-r-client"></a>

## Step 3: Try Out R Client

Now that you've installed R Client and set up your IDE to use R Client, you can start building and running your R code. Launch your IDE now and you'll see that Microsoft R Client is running in the console window. 

Start developing your own solutions using the [`RevoScaleR` package functions](https://msdn.microsoft.com/en-us/microsoft-r/scaler/scaler) and APIs. When ready, you can run that R code using R Client in your IDE or even send those R commands to a remote R Server for execution. 

<!--## Step 3: Try Our Sample Script

Now that you've installed R Client and set up your IDE to use R Client, you can start building and running your R code.

In your newly configured IDE, you can start developing your own solutions using the [`RevoScaleR` package functions](https://msdn.microsoft.com/en-us/microsoft-r/scaler/scaler) and APIs. When ready, you can run that R code using R Client in your IDE or even send those R commands to a remote server for execution. 

Here's an example you use to try out R Client... -->


##Learn More

You can learn more with these guides:

+ [Get Started with Microsoft R Client](r-client-get-started.md) 

+ [Microsoft R Getting Started](microsoft-r-getting-started.md) 

+ [RevoScaleR Getting Started](scaler-getting-started.md)