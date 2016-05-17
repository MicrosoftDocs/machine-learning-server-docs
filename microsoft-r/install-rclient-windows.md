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

You can host distributed, high performance R solutions on a Windows desktop computer. You can also use Microsoft R Server to develop solutions for deployment to an instance of SQL Server that supports R script execution.

When you install R Client, you get the same enhanced R packages and connectivity tools that are provided in R Server.

>[!NOTE]
>Microsoft R Client is available only on Windows.
>
>If you have installed Microsoft R Client previously, you must uninstall it first. 

This document assumes you have access to the Microsoft R Client installer, which is available either through your Volume Licensing agreement or an MSDN subscription.

##OPEN QUESTIONS

1.  Where do I get the release notes?

1.  When to use Revo*** and when to drop it?

1.  Difference between RevoScaleR and ScaleR??

1.  When you start up R CLient what happens? A dialog? A windows? Nothing? 

1.  I didn’t see a EULA or consent to install RTVS in the powerpoint slides. Will there be one in the installer?
 
2.       Are we limited to 64-bit only?
 
3.       Unlike Microsoft R Server, R Client is NOT supported on Win Server 2008 or 2012, correct?
 
4.       When you start up R CLient what happens? A dialog? A windows? Nothing?
 
5.       For the offline install situation,
·         What do they need to download and preinstall for R Open?
·         What do they need to download and preinstall for RTVS?

DeployR: We should put a link to the DeployR Rserve component in the EULA window in case they have an offline install?


- [**Download the Release Notes**](http://packages.revolutionanalytics.com/doc/8.0.0/README_RevoEnt_Windows_8.0.0.pdf) for more information on Revolution R Enterprise 2016 for Windows, including known issues.

##System Requirements

R Client for Windows includes RevoScaleR, RevoTreeView, and RevoPemaR. This version is supported on 64-bit Windows 7, Windows 8.1, Windows 10, Windows Server 2008, and Windows Server 2012.

Approximately 600MB free disk space is required for a full install of R Client, after installation of all prerequisites. We recommend at least 4GB of RAM to use R Client.

##Install Microsoft R Client

> To install the files, you must be logged in with **Administrator** privileges, and you must install to a local drive on your computer. 

1. Log in to the machine with **Administrator** privileges.

1. Close any other programs running on the system and disable any antivirus software you may have running, such as McAfee Total Protection or Norton AntiVirus.

1. Run **Microsoft R Client** setup.

1. Accept the license terms for downloading and installing Microsoft R Client.

1. In addition to **Microsoft R Client**, select the components to install. **Microsoft R Open**, the enhanced distribution of R, is required. You can also choose to install **R Tools for Visual Studio**.

1. Accept the license terms for downloading and installing **Microsoft R Open**.

1. Accept the default installation path or choose another location.

1. Setup of the R components used by Microsoft R Client requires an Internet connection for access to files that are provided either on the Microsoft Download Center or another trusted site. If you are performing an offline install, Microsoft R Client cannot access the links for installing required R components. To avoid this problem, you can download a copy of the installers locally and complete setup as described here:

   1. Pause the Microsoft R Client setup wizard without closing it.

    [!INCLUDE] setup should display a dialog box with links to the installers for the required components.

    On opening the link, download begins immediately. By default installers are saved to the Downloads folder.
    System_CAPS_ICON_tip.jpg Tip

    Microsoft R Open for R Server

    http://go.microsoft.com/fwlink/?LinkId=733805&lcid=1033

1. Installation of these components (and any prerequisites they might require) might take a while. When the Accept button becomes unavailable, you can click Next.

1. On the **Ready to Install** page, verify your selections. Click **Install**.


##What is Installed and Where to Find R Packages

Setup for Microsoft R Client installs the R base packages and a set of enhanced R packages that support parallel processing, improved performance, and connectivity to **data sources including SQL Server and Hadoop.**

###R packages

The R libraries are installed together with other tools and utilities that are installed with Microsoft SQL Server 2016. For example:

C:\Program Files\Microsoft SQL Server\130\R_SERVER

Additionally, in this section you will find documentation for the R base packages, sample data, and the R library.

If you have installed an instance of SQL Server with R Services (In-Database) on the same computer, the R libraries and tools are installed into a different folder: C:\Program Files\Microsoft SQL Server\130\R_SERVER

Do not use the R packages or utilities associated with the SQL Server instance. Always use the R tools and packages in the R_SERVER folder.

###R tools

An R development IDE is not installed as part of setup. You can install RStudio, R Tools for Visual Studio, or any other development environment you prefer.

However, additional tools aren't required. All the standard base R tools are included in C:\Program Files\Microsoft SQL Server\130\R_SERVER\bin.

    For more information, see Setup or Configure R Tools.

Upgrading from an Older Version of Microsoft R Server


##Troubleshooting

###Testing the Install
The RevoIOQ package provides a set of tests to verify correct installation and operation of R Client. To run these tests, run the following commands from your R prompt:

	library(RevoIOQ)
	RevoIOQ()

A fresh install of Microsoft R Client should yield an error-free and failure-free report in your Web browser, though there may be some _Deactivated Tests_.

###Installation Errors
1. **Installation fails with error "ERROR TEXT".**


##Launching Microsoft R Client

After you have installed the software, you launch Microsoft R Client as follows.**

+ **For Windows 7 and Windows Server 2008**

  1. From the **Task Bar**, click **Start**.
  1. Click **All Programs**.
  1. Click **Revolution R**.
  1. Click **Revolution R Enterprise 8.0 (64)**.

+ **For Windows 8 and Windows Server 2012:**

  1. Move the pointer to the lower left corner of the Desktop until the **Start** icon appears.
  1. Click **Start** to view the **Start** screen.
  1. Locate and click the tile for **Revolution R Enterprise 8.0 (64)**.

+ **For Windows 10**

  1. From the **Task Bar**, click **Start**.
  1. Click **All apps**.
  1. Click **Revolution R**.
  1. Click **Revolution R Enterprise 8.0 (64)**.


[Microsoft R Getting Started Guide](microsoft-r-getting-started.md) and the [RevoScaleR Getting Started Guide](scaler-getting-started.md). These provide tutorial introductions to working with Microsoft R Client.


## Sample Data

Sample data sets for use with Revolution R Enterprise can be found [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409).