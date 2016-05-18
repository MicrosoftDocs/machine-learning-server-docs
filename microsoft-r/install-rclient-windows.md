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

When you install R Client, you get the same enhanced R packages and connectivity tools that are provided in R Server.

This document assumes you have access to the Microsoft R Client installer, which is available either through your Volume Licensing agreement or an MSDN subscription.

>Currently, Microsoft R Client is available only on Windows.

##OPEN QUESTIONS

1. What is R Client in a nutshell?

1. Where do I get the release notes?

1. When you start up R CLient what happens? A dialog? A windows? Nothing? 

1. When you start up R CLient what happens? A dialog? A windows? Nothing?
 
1. For the offline install situation,
   + What do they need to download and preinstall for R Open?
   + What do they need to download and preinstall for RTVS?

- [**Download the Release Notes**](http://packages.revolutionanalytics.com/doc/8.0.0/README_RevoEnt_Windows_8.0.0.pdf) for more information on Revolution R Enterprise 2016 for Windows, including known issues.

##System Requirements

+ **Operating Systems**:   64-bit versions of Microsoft Windows 7, Windows 8.1, and Windows 10
+ **Free disk space**: 600+ GB recommended, after installation of all prerequisites       
+ **RAM**: 4+ GB recommended
+ **Internet access**:  To download R Client and any dependencies     



|System Requirement   |Value                                                              | 
|---------------------|-------------------------------------------------------------------|
|Operating Systems    |64-bit versions of Microsoft Windows 7, Windows 8.1, and Windows 10|
|Free disk space      |600+ GB recommended, after installation of all prerequisites       |
|RAM                  |4+ GB recommended                                                  |
|Internet access      |To download R Client and any dependencies                          |


##Install Microsoft R Client

> You must install Microsoft R Client to a local drive on your computer. 

1. Log in to the machine with administrator privileges.

1. Close any other programs running on the system and disable any antivirus software you may have running, such as McAfee Total Protection or Norton AntiVirus.

1. Run Microsoft R Client setup.

1. Accept the license terms for downloading and installing Microsoft R Client.

1. In addition to Microsoft R Client, select the components to install. 
   + **Microsoft R Open**, Microsoft's enhanced distribution of R, is a _required dependency_. 
   
   + **R Tools for Visual Studio**, an integrated development environment, is an _optional dependency_. R Tools for Visual Studio is a free add-in for Visual Studio that works in all editions of Visual Studio. 

1. Accept the license terms for downloading and installing Microsoft R Open.

1. Accept the default installation path or choose another location.

1. When the installation finishes, click **Finish**.  

<!--1. Setup of the R components used by Microsoft R Client requires an Internet connection for access to files that are provided either on the Microsoft Download Center or another trusted site. If you are performing an offline install, Microsoft R Client cannot access the links for installing required R components. To avoid this problem, you can download a copy of the installers locally and complete setup as described here:

   1. Pause the Microsoft R Client setup wizard without closing it.

    [!INCLUDE] setup should display a dialog box with links to the installers for the required components.

    On opening the link, download begins immediately. By default installers are saved to the Downloads folder.
    System_CAPS_ICON_tip.jpg Tip

    Microsoft R Open for R Server

    http://go.microsoft.com/fwlink/?LinkId=733805&lcid=1033

1. On the **Ready to Install** page, verify your selections. Click **Install**.
-->

<br>

>[!WARNING]
>For help with any installation issues, please check out these [troubleshooting tips](#troubleshooting) or post questions on the [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).

<br>
##Launching Microsoft R Client

After you have installed the software, you launch Microsoft R Client as follows.**

+ **For Windows 7 and Windows 10:**

  1. From the **Task Bar**, choose **Start > All Programs > Microsoft R Client > Rgui**.


+ **For Windows 8.1:**

  1. Move the pointer to the lower left corner of the Desktop until the **Start** icon appears.
  1. Click **Start** to view the **Start** screen.
  1. Locate and click the tile for **Microsoft R Client**.

<br>
##Learn More about Microsoft R Client

<br>
###Tutorials and Getting Started Guides

You can learn more and explore tutorial introductions to R in the [Microsoft R Getting Started Guide](microsoft-r-getting-started.md) and the [RevoScaleR Getting Started Guide](scaler-getting-started.md).

<br>
###What is Installed and Where to Find R Packages

Setup for Microsoft R Client installs the R base packages and a set of enhanced R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop.

<br>
####R packages

The R libraries are installed under `C:\Program Files\Microsoft SQL Server\130\R_SERVER`.

Additionally, in this directory you will find documentation for the R base packages, sample data, and the R library.

<br>
####R tools

All the standard base R tools are included in `C:\Program Files\Microsoft SQL Server\130\R_SERVER\bin`:

+ **RTerm**: A command-line tool for running R scripts. Rterm.exe can allocate more memory when running in 64-bit Windows.

+ **RGui.exe**: A simple interactive editor for R. The command-line arguments are the same for RGui.exe and RTerm.

+ **RScript**: A command-line tool for running R scripts in batch mode.

If you installed _R Tools for Visual Studio (RTVS)_ along with Microsoft R Client, then you'll also have access to an integrated R development IDE. _RTVS_ is a free add-in for all editions of _Visual Studio_.

If you did not install _RTVS_, it's not too late. You can always install and use _RTVS_, _RStudio_, or any other development environment. [Learn more on installing R Tools for Visual Studio...](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1)

>[!TIP]
>**Need more help?** Documentation for these tools can be found in the setup folder: `C:\Program Files\Microsoft SQL Server\R_SERVER\doc` and in `C:\Program Files\Microsoft SQL Server\R_SERVER\doc\manual`. One easy way to open these files is to open RGui, click Help, and select one of the options.

<br>
##Troubleshooting

###Testing the Install
The RevoIOQ package provides a set of tests to verify correct installation and operation of R Client. To run these tests, run the following commands from your R prompt:

	library(RevoIOQ)
	RevoIOQ()

A fresh install of Microsoft R Client should yield an error-free and failure-free report in your Web browser, though there may be some _Deactivated Tests_.

###Installation Errors
1. **Installation fails with error "ERROR TEXT".**