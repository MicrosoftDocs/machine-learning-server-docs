---

# required metadata
title: "Install Microsoft R Client on Windows"
description: "Microsoft R Client Install Guide Windows"
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

# Install Microsoft R Client on Windows

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](scaler-getting-started.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is capped at two threads for RevoScaleR functions. 

To benefit from disk scalability, performance and speed, you can push the compute context using rxSetComputeContext() to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility.](r-client-compatibility.md)


## System Requirements

|   |   |
| - | - |
|Operating Systems|64-bit versions of Microsoft Windows 7, 8.1, and 10|
|Free disk space|600 MB recommended, after installation of all prerequisites<br>1.2 GB recommended if pretrained models are installed|
|Available RAM|4+ GB recommended|
|Internet access|Needed to download R Client and any dependencies|


## Setup Requirements

On the machine onto which you are installing, follow this guidance before you begin installing:

1. @@The [.NET Framework 4.5.2](https://www.microsoft.com/download/details.aspx?id=42642) component must be installed to run setup. Use the link provided in the setup wizard. Installing this component requires a computer restart.

1. You must install Microsoft R Client to a local drive on your computer.

1. You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.

1. Close any other programs running on the system.

## How to install (with internet access)

1. Log in to the machine with administrator privileges.

1. Download Microsoft R Client from the following link: http://aka.ms/rclient/

1. @@Run the Microsoft R Client setup and follow the prompts:

    + Accept the default installation path for Microsoft R Client or choose another location.

    + Review the components that will be installed as part of Microsoft R Client. 
    
      While most are required, you can choose to add additional components such as [**pretrained models**](deploy-pretrained-microsoftml-models.md). 

    + Accept the Microsoft R Client license terms.

    + Accept the Microsoft R Open license term. Microsoft R Client is built on [Microsoft R Open](r-open.md), Microsoft's enhanced distribution of R. Setup installs it for you automatically.

    + Optionally, install [R Tools for Visual Studio](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1), an integrated development environment available as a free add-in for any edition of Visual Studio 2015. This option is only available if the supported version of Visual Studio is already installed.  If you've selected to install it as well, accept the terms for R Tools for Visual Studio.

    + Click **Finish** when installation is finished. A welcome screen opens to introduce you to the product and documentation.

## How to offline install (without internet access)

1. @@On a machine with _**unrestricted**_ internet access:

   + Download Microsoft R Client from the following link: http://aka.ms/rclient/

   + Download the Microsoft R Open ( *.cab) needed to install R Client from the following link: https://go.microsoft.com/fwlink/?LinkId=834568&clcid=1033

   + Download the prerequisites, including the .NET Framework and other components previously listed.

   + Copy the .cab file, component executables, and R Client installer to a network share or portable drive.

1. On the machine with _**restricted**_ internet access:

   + Log in with administrator privileges.

   + Copy the .cab file, component executables, and R Client installer from the network share/portable drive on the first machine to the machine that has restricted internet access. Put those files under `%temp%`. 

   + Install the prerequisites first. 
   
   + Restart your computer is you installed the .NET Framework.

   + Run `RClientSetup.exe`, which then finds the cab file in the temp folder.
   
   + Accept the default installation path for Microsoft R Client or choose another location.

   + Review the components that will be installed as part of Microsoft R Client. 
    
     While most are required, you can choose to add additional components such as [**pretrained models**](deploy-pretrained-microsoftml-models.md). 

1. Without internet access, we recommend disabling the _auto-update check_ feature so that R Client can launch more quickly. Do so in the `Rprofile.site` file by adding a comment character (#) at the start of the line: `mrupdate::mrCheckForUpdates()`

    
<a name=silent></a> 

## Silent and Passive Installs

@@To install Microsoft R Client from a script on Windows, use the following commandline switches. 

|Mode        |Install Command|Description|
|-----------|-------------------------------|--------------|
|Passive|`RClientSetup.exe /passive`|No prompts, but a progress indicator|
|Silent|`RClientSetup.exe /quiet`|No prompts nor progress indicator|
 

## What's Installed with R Client

The Microsoft R Client setup installs the R base packages and a set of enhanced and proprietary R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop. The R libraries are installed under the R Client installation directory, @@`C:\Program Files\Microsoft\R Client\R_SERVER`. Additionally, in this directory you will find documentation for the R base packages, sample data, and the R library.

All of tools for the standard base R (RTerm, Rgui.exe, and RScript) are also included with Microsoft R Client under `<install-directory>\bin`. Documentation for these tools can be found in the setup folder: `<install-directory>\doc` and in `<install-directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options.

> [!NOTE]
> By default, telemetry data is collected during your usage of R Client. To turn this feature off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.

## Learn More

You can learn more with these guides:

+ [Get Started with Microsoft R Client](r-client-get-started.md) 

+ [Microsoft R Getting Started](microsoft-r-getting-started.md) 

+ [Diving into Data Analysis with Microsoft R](data-analysis-in-microsoft-r.md)

+ [Get started with RevoScaleR](microsoft-r-get-started-node.md)

+ [Get started with MicrosoftML](microsoftml-get-started.md)

+ [Get started with mrsdeploy](mrsdeploy/mrsdeploy.md)
