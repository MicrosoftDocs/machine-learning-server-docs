---

# required metadata
title: "Install Microsoft R Client on Windows"
description: "Guide to installing Microsoft R Client on Windows. R Client is a free, data science tool for high performance analytics."
keywords: "R Client, R IDE configuration, RTVS, R Tools for Visual Studio, Microsoft R Client"
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Install Microsoft R Client on Windows

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](../r/tutorial-revoscaler-data-import-transform.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is capped at two threads for RevoScaleR functions. 

To benefit from disk scalability, performance and speed, push the compute context using rxSetComputeContext() to a production instance of Microsoft R Server (or R Server) such as [SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and Machine Learning Server for Hadoop. [Learn more about its compatibility.](compatibility-with-server.md)
  
You can offload heavy processing to Machine Learning Server or test your analytics during their development by running your code remotely using [remoteLogin() or remoteLoginAAD()](../r/how-to-execute-code-remotely.md) from the mrsdeploy package. 

For a What's New for Microsoft R Client, see [here](what-is-microsoft-r-client.md#r-client-whats-new).

## System Requirements

|   |   |
| - | - |
|Operating&nbsp;Systems|64-bit versions of Microsoft Windows 7, 8.1, and 10|
|Free disk space|600 MB recommended, after installation of all prerequisites<br>If pre-trained models are installed, 1.2 GB is recommended|
|Available RAM|4+ GB recommended|
|Internet access|Needed to download R Client and any dependencies|
|.NET Framework 4.5.2| Framework component must be installed to run setup. Use the link provided in the setup wizard. Installing this component requires a computer restart.|

The following additional components are included in Setup and required:
+ Microsoft R Open 3.4.1
+ Microsoft MPI 7.1
+ AS OLE DB (SQL Server 2016) provider
+ Microsoft Visual C++ 2015 Redistributable

## Setup Requirements

On the machine onto which you are installing, follow this guidance before you begin installing:

1. Always install Microsoft R Client to a local drive on your computer.

1. You may need to disable your antivirus software. If you do, be sure to turn it back on immediately after installing.

1. Close any other programs running on the system.

## How to install (with internet access)

1. Log in to the machine with administrator privileges.

1. Download Microsoft R Client from the following link: http://aka.ms/rclient/

1. Launch the Microsoft R Client setup and follow the prompts.

1. Accept the default installation path for Microsoft R Client or choose another location.

1. Review the components that are installed as part of Microsoft R Client. 
    
   While most are required, you can choose to add additional components such as [**pre-trained models**](../install/microsoftml-install-pretrained-models.md). 

1. Accept the Microsoft R Client license terms.

1. Accept the Microsoft R Open license term. Microsoft R Client is built on Microsoft R Open, Microsoft's enhanced distribution of R. Setup installs the correct version of R Open for you automatically.

1. Click **Finish** when installation is finished. A welcome screen opens to introduce you to the product and documentation.

## How to offline install (without internet access)

>[!WARNING]
>Microsoft R Open is a requirement of Microsoft R Client. In offline scenarios when no internet connection is available on the target machine, you must manually download the R Open installer. Use only the link specified in the installer or installation guide. Do NOT go to MRAN and download it from there or you may inadvertently get the wrong version for your Microsoft R product. 

1. On a machine with _**unrestricted**_ internet access:

   1. Download Microsoft R Client from http://aka.ms/rclient/.

   1. Download the Microsoft R Open ( *.cab) needed to install R Client from https://go.microsoft.com/fwlink/?LinkId=852724.

   1. Download .NET Framework 4.5.2 from https://www.microsoft.com/download/details.aspx?id=42642.

   1. Download, if desired, the .cab file for pre-trained machine learning models from: https://go.microsoft.com/fwlink/?LinkId=852727. Learn more about these [pre-trained models](../install/microsoftml-install-pretrained-models.md). 
   
   1. Copy the downloaded files to a network share or portable drive.

1. On the machine with _**restricted**_ internet access:

   1. Log in with administrator privileges.

   1. Copy the .cab file and R Client installer from the network share/portable drive on the first machine to the machine that has restricted internet access. Put the CAB files in the setup user's temp folder under `%temp%`. 

   1. Copy and install the .NET Framework.  Restart your computer if you installed the .NET Framework.

   1. Run `RClientSetup.exe`, which finds the cab file in the temp folder for you.
   
   1. Accept the default installation path for Microsoft R Client or choose another location.

   1. Review the components that are installed as part of Microsoft R Client. 
    
     While most are required, you can choose to add additional components such as [**pre-trained models**](../install/microsoftml-install-pretrained-models.md). 

   1. Accept the Microsoft R Client license terms.

   1. Accept the Microsoft R Open license term. Microsoft R Client is built on Microsoft R Open, Microsoft's enhanced distribution of R. Setup installs the correct version of R Open for you automatically.

1. Without internet access, we recommend disabling the _auto-update check_ feature so that R Client can launch more quickly. Do so in the `Rprofile.site` file by adding a comment character (#) at the start of the line: mrupdate::mrCheckForUpdates()

>[!IMPORTANT]
>Review the recommendations in [Package Management](../operationalize/configure-manage-r-packages.md#offline) for instructions on how to set up a local package repository using MRAN or miniCRAN.

<a name="silent"></a> 

## Silent and Passive Installs

To install Microsoft R Client from a script on Windows, use the following command line switches:

|Mode        |Install Command|Description|
|-----------|-------------------------------|--------------|
|Passive|`RClientSetup.exe /passive`|No prompts, but a progress indicator|
|Silent|`RClientSetup.exe /quiet`|No prompts nor progress indicator|
 

## What's Installed with R Client

Microsoft R Client installs the R base packages and a set of enhanced and proprietary R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop. The R libraries are installed under the R Client installation directory, `C:\Program Files\Microsoft\R Client\R_SERVER`. Additionally, this directory contains the documentation for the R base packages, sample data, and the R library.

All tools for the standard base R (RTerm, Rgui.exe, and RScript) are also included with Microsoft R Client under `<install-directory>\bin`. Documentation for these tools can be found in the setup folder: `<install-directory>\doc` and in `<install-directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options.

> [!NOTE]
> By default, telemetry data is collected during your usage of R Client. To turn off this feature, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.

## How to uninstall

+ Remove Microsoft R Client like other applications using the Add/Remove dialog on Windows.

+ Alternately, you can use the same setup file used to install R Client to remove the program by specifying the /uninstall option on the command line such as:
  ```RClientSetup.exe /uninstall```


## Learn More

You can learn more with these guides:

+ [Overview of Microsoft R Client](../r-client-get-started.md) 

+ [Quickstart: Running R code in Microsoft R](../r/quickstart-run-r-code.md) (example)

+ [How-to guides in Machine Learning Server](../r/how-to-introduction.md)

+ [RevoScaleR R package reference](../r/tutorial-introduction.md)

+ [MicrosoftML R package reference](../r-reference/microsoftml/microsoftml-package.md)

+ [mrsdeploy R package reference](../r-reference/mrsdeploy/mrsdeploy-package.md)

+ [Execute code on remote Machine Learning Server](../r/how-to-execute-code-remotely.md)
