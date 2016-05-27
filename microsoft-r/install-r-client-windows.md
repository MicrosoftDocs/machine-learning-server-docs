---

# required metadata
title: "Install Microsoft R Client on Windows"
description: "Windows installation of Microsoft R Client."
keywords: ""
author: "j-martens"
manager: "paulette.mckay"
ms.date: "05/17/2016"
ms.topic: "get-started-article"
ms.prod: "rclient"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

#Install Microsoft R Client on Windows

Microsoft R Client is a free, data science tool that runs locally on your computer and allows you to do high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the powerful ScaleR technology and its proprietary functions to benefit from parallelization and remote computing. R client is optimized to work with all Microsoft R Server versions. For a side by side comparison of R features in Microsoft R Server, R Client, and R Open, [see here](index.md#compare-prods).

>Currently, Microsoft R Client is available only on Windows.

##System Requirements

+ **Operating Systems**:   64-bit versions of Microsoft Windows 7, Windows 8.1, and Windows 10

+ **Free disk space**: 600+ MB recommended, after installation of all prerequisites       

+ **RAM**: 4+ GB recommended

+ **Internet access**:  Needed to download R Client and any dependencies     

<br>
##Install Microsoft R Client

>You must install Microsoft R Client to a local drive on your computer. 

>These instructions assume that you have access to the Microsoft R Client installer, which is available through Visual Studio Dev Essentials.

>You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.

**To install Microsoft R Client**:

1. Log in to the machine with administrator privileges.

1. Close any other programs running on the system. 

1. Run **Microsoft R Client** setup.

1. Accept the Microsoft R Client license terms.

1. In addition to Microsoft R Client, select the components to install. 
   + [**Microsoft R Open**](index.md#mro), Microsoft's enhanced distribution of R, is a _required dependency_. 
   
   + **R Tools for Visual Studio**, an integrated development environment, is an _optional dependency_. R Tools for Visual Studio is a free add-in for Visual Studio that works in all editions of Visual Studio. This option is only available if the supported version of Visual Studio is already installed.

1. Accept the license terms for downloading and installing Microsoft R Open.

1. Accept the default installation path or choose another location.

1. When the installation finishes, click **Finish**.  A welcome screen opens to introduce you to the product and documentation.

>After you have installed R Client and installed your favorite R IDE, you can begin developing your solution using the `RevoScaleR` package. The functions and APIs in this package enable you to send R commands to a remote server for execution. Learn more in the [Microsoft R Getting Started](microsoft-r-getting-started.md) guide.


<br>
##Launch Microsoft R Client

**To launch Microsoft R Client**:

After you have installed the software, you launch Microsoft R Client as follows.

+ For Windows 7 and Windows 10:

  + From the **Task Bar**, choose **Start > All Programs > Microsoft R Client > Rgui**.

<br>
+ For Windows 8.1:

  1. Move the pointer to the lower left corner of the Desktop until the **Start** icon appears.
  
  1. Click **Start** to view the **Start** screen.

  1. Locate and click the tile for **Microsoft R Client**.

<br>
##Testing the Install
The `RevoIOQ` package provides a set of tests to verify correct installation and operation of R Client. A fresh install of Microsoft R Client should yield an error-free and failure-free report in your Web browser, though there may be some _Deactivated Tests_.

**To run these tests:**

Run the following commands from your `R` prompt:

	library(RevoIOQ)
	RevoIOQ()

<br>
##What's Installed & Where to Find R Packages

Setup for Microsoft R Client installs the R base packages and a set of enhanced R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop.

<br>
**R packages**
The R libraries are installed under the R Client installation directory, `C:\Program Files\Microsoft\R Client\R_SERVER`. Additionally, in this directory you will find documentation for the R base packages, sample data, and the R library.

<br>
**R command-line tools and GUI editor**
All of tools for the standard base R are included with Microsoft R Client under `<R-Client-Install-Directory>\bin`. Documentation for these tools can be found in the setup folder: `<R-Client-Install-Directory>\doc` and in `<R-Client-Install-Directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options. 

Those standard R tools are:

+ `RTerm`: A command-line tool for running R scripts. Rterm.exe can allocate more memory when running in 64-bit Windows.

+ `RGui.exe`: A simple interactive editor for R. The command-line arguments are the same for RGui.exe and RTerm.

+ `RScript`: A command-line tool for running R scripts in batch mode.

<br>
**The R Integrated Development Environment (IDE)**

The R IDE options are:
+ If you installed _R Tools for Visual Studio (RTVS)_ along with Microsoft R Client, then you'll also have access to an integrated R development environment. _RTVS_ is a free add-in for all editions of _Visual Studio_.

+ If you did not install _RTVS_, it's not too late. You can always install and use _RTVS_, _RStudio_, or any other development environment. [Learn more on installing R Tools for Visual Studio...](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1)

<br>
##Tutorials & Getting Started Guides

You can learn more and explore tutorial introductions to R in the [Microsoft R Getting Started Guide](microsoft-r-getting-started.md) and the [RevoScaleR Getting Started Guide](scaler-getting-started.md).