---

# required metadata
title: "Install Microsoft R Client on Windows"
description: "Windows installation of Microsoft R Client."
keywords: ""
author: "j-martens"
manager: "paulette.mckay"
ms.date: "05/17/2016"
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

#Install Microsoft R Client on Windows

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the powerful ScaleR technology and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility.](r-client-compatibility.md)

## Where do I get it?

Microsoft R Client is free to everyone. [Download Microsoft R Client](http://aka.ms/rclient/download), available through Visual Studio Dev Essentials, and install it today. 


[!include[Installing R Client](/includes/r-client/r-client-install.md)]


##System Requirements

>Currently, Microsoft R Client is available only on Windows.

+ **Operating Systems**:   64-bit versions of Microsoft Windows 7, Windows 8.1, and Windows 10

+ **Free disk space**: 600+ MB recommended, after installation of all prerequisites       

+ **RAM**: 4+ GB recommended

+ **Internet access**:  Needed to download R Client and any dependencies     

<br>
##Install Microsoft R Client

>You must install Microsoft R Client to a local drive on your computer. 

>You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.

**To install Microsoft R Client**:

1. Log in to the machine with administrator privileges.

1. [Download Microsoft R Client](http://aka.ms/rclient/download).

1. Close any other programs running on the system. 

1. Run **Microsoft R Client** setup.

1. Accept the Microsoft R Client license terms.

   To install Microsoft R Client, you'll need [Microsoft R Open](index.md#mro), Microsoft's enhanced distribution of R. The setup will install it for you automatically.

 1. If desired, select the option of installing **R Tools for Visual Studio**, an integrated development environment. [R Tools for Visual Studio](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1) is a free add-in for Visual Studio that works in all editions of Visual Studio. This option is only available if the supported version of Visual Studio is already installed.

1. Accept the license terms for Microsoft R Open. And, if you've selected to install it as well, accept the terms for R Tools for Visual Studio.

1. Accept the default installation path for Microsoft R Client or choose another location.

1. When the installation finishes, click **Finish**.  A welcome screen opens to introduce you to the product and documentation.

>After you have installed R Client and installed your favorite R IDE, you can begin developing your solution using the `RevoScaleR` package. [The functions](scaler/scaler.md) and APIs in this package enable you to send R commands to a remote server for execution. Learn more in the [Microsoft R Getting Started](microsoft-r-getting-started.md) guide.


<br>
##Launch Microsoft R Client

**To launch Microsoft R Client**:

After you have installed the software, you launch Microsoft R Client as follows.

For Windows 10:

+ Choose **All Apps > Microsoft R Client > Rgui**.


For Windows 8.1:

1. Move the pointer to the lower left corner of the Desktop until the **Start** icon appears.
  
1. Click **Start** to view the **Start** screen.

1. Locate and click the tile for **Microsoft R Client**.


For Windows 7:

+ From the **Task Bar**, choose **Start > All Programs > Microsoft R Client > Rgui**.

<br>
##Testing the Install
The `RevoIOQ` package provides a set of tests to verify correct installation and operation of R Client. A fresh install of Microsoft R Client should yield an error-free and failure-free report in your Web browser, though there may be some _Deactivated Tests_.

**To run these tests:**

Run the following commands from your `R` prompt:

	library(RevoIOQ)
	RevoIOQ()


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

+ If you did not install _RTVS_, it's not too late. You can always install and use _RTVS_, _RStudio_, or any other development environment. [Learn more about installing R Tools for Visual Studio.](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1)

<br>
##Tutorials & Getting Started Guides

You can learn more and explore tutorial introductions to R in:

+ The [Microsoft R Getting Started Guide](microsoft-r-getting-started.md) 

+ The [RevoScaleR Getting Started Guide](scaler-getting-started.md)