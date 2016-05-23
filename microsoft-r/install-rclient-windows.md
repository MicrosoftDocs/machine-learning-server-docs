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

Microsoft R Client is a free tool to enable data scientists to connect to and explore production data as well as create models and algorithms using ScaleR APIs.  When you install R Client, you get the same enhanced R packages and connectivity tools that are provided in Microsoft R Server. And while R Client can be used on its own, you can benefit from the hybrid memory, disk scalability, performance and speed when you push the compute context to a Microsoft R Server instance. 

R client is optimized to work with all Microsoft R Server versions. For a comparison of R Client and R Server, see the [Microsoft R Getting Started Guide](microsoft-r-getting-started.md)

>Currently, Microsoft R Client is available only on Windows.

##System Requirements

+ **Operating Systems**:   64-bit versions of Microsoft Windows 7, Windows 8.1, and Windows 10

+ **Free disk space**: 600+ GB recommended, after installation of all prerequisites       

+ **RAM**: 4+ GB recommended

+ **Internet access**:  To download R Client and any dependencies     

<br>
##Install Microsoft R Client

>You must install Microsoft R Client to a local drive on your computer. 

>These instructions assume that you have access to the Microsoft R Client installer, which is available through Visual Studio Dev Essentials.

**To install Microsoft R Client**:

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

>After you have installed R Client and installed your favorite R IDE, you can begin developing your solution using the RevoScaleR package. These APIs let you send R commands to a remote server for execution. Learn more in the [Getting Started Guide](rserver-getting-started.md).

<br>

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
##Learn about Microsoft R Client
<br>
###Tutorials & Getting Started Guides

You can learn more and explore tutorial introductions to R in the [Microsoft R Getting Started Guide](microsoft-r-getting-started.md) and the [RevoScaleR Getting Started Guide](scaler-getting-started.md).
<br>
###What's Installed & Where to Find R Packages

Setup for Microsoft R Client installs the R base packages and a set of enhanced R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop.

####R packages
The R libraries are installed under `C:\Program Files\Microsoft SQL Server\130\R_SERVER`. Additionally, in this directory you will find documentation for the R base packages, sample data, and the R library.

####R tools
All the standard base R tools are included with Microsoft R Client under `C:\Program Files\Microsoft SQL Server\130\R_SERVER\bin`. Documentation for these tools can be found in the setup folder: `C:\Program Files\Microsoft SQL Server\R_SERVER\doc` and in `C:\Program Files\Microsoft SQL Server\R_SERVER\doc\manual`. One easy way to open these files is to open RGui, click Help, and select one of the options. 

Those standard R tools are:

+ **RTerm**: A command-line tool for running R scripts. Rterm.exe can allocate more memory when running in 64-bit Windows.

+ **RGui.exe**: A simple interactive editor for R. The command-line arguments are the same for RGui.exe and RTerm.

+ **RScript**: A command-line tool for running R scripts in batch mode.

If you installed _R Tools for Visual Studio (RTVS)_ along with Microsoft R Client, then you'll also have access to an integrated R development IDE. _RTVS_ is a free add-in for all editions of _Visual Studio_.

If you did not install _RTVS_, it's not too late. You can always install and use _RTVS_, _RStudio_, or any other development environment. [Learn more on installing R Tools for Visual Studio...](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1)

<br>
##Troubleshooting

###Testing the Install
The RevoIOQ package provides a set of tests to verify correct installation and operation of R Client. A fresh install of Microsoft R Client should yield an error-free and failure-free report in your Web browser, though there may be some _Deactivated Tests_.

**To run these tests:**

Run the following commands from your `R` prompt:

	library(RevoIOQ)
	RevoIOQ()

###Installation Errors

1. **Installation fails with error "ERROR TEXT".**