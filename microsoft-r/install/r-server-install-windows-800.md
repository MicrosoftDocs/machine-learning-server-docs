---

# required metadata
title: "Installation Guide for Revolution R Enterprise 2016 (version 8.0) for Windows"
description: "How to install Microsoft R Server 8.0 on computers running the Windows operating system."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "11/17/2016"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# Install Revolution R Enterprise 2016 (version 8.x) for Windows

>  Support for this version of R Server is defined in the [Microsoft Modern Lifecycle Policy](https://support.microsoft.com/help/30881/modern-lifecycle-policy). For more information about how this applies to Microsoft R Server, see [Support for R Server versions](https://msdn.microsoft.com/microsoft-r/rserver-servicing-support). For installation instructions for newer versions, see [Install Microsoft R Server for Windows](r-server-install-windows.md).

Revolution R Enterprise 2016 (version 8.0) for Windows includes RevoScaleR, RevoTreeView, RevoPemaR, DeployR, and the R Productivity Environment.

We recommend that you get the [**modified GPL/LGPL source code**](http://go.microsoft.com/fwlink/?LinkId=715643&clcid=0x409) (rre-gpl-src.8.0.0.tar.gz - 94.2MB) for this release. This is made available in compliance with the GNU General Public License, but is not required to install or use Revolution R Enterprise.

Approximately 600MB free disk space is required for a full install of Revolution R Enterprise, after installation of all prerequisites. We recommend at least 4GB of RAM to use Revolution R Enterprise, and at least 4GB to use DeployR.

DeployR is an optional component and is supported on Windows Server 2008 R2 (Service Pack 1) and Windows Server 2012 only. For the full list of system requirements for DeployR, check the [DeployR 8.0.0 Installation Guide](~/deployr/deployr-installing-configuring.md).

## Installation Instructions

Before running the installer, close any other programs running on the system and disable any antivirus software you may have running, such as McAfee Total Protection or Norton AntiVirus.

You must be logged in with **Administrator** privileges.

You must install to a local drive on your computer. (Installing to a network drive is not currently supported.)

Revolution R Enterprise 2016 for Windows is installed in two steps: first, you install Microsoft R Open for Revolution R Enterprise, and then install Revolution R Enterprise itself. This document assumes you have access to the Revolution R Enterprise installer, which is available either through your Volume Licensing agreement or an MSDN subscription.

- [Install Microsoft R Open for Revolution R Enterprise](http://go.microsoft.com/fwlink/?LinkId=699383) if you have not already done so.
- Double-click **Revolution-R-Enterprise-8.0.0-Windows.exe** and follow the on-screen prompts. This installs the Revolution R Enterprise product.

**After you have installed the software, you launch Revolution R Enterprise as follows.**

**For Windows 7 and Windows Server 2008**

-   From the **Task Bar**, click **Start**.
-   Click **All Programs**.
-   Click **Revolution R**.
-   Click **Revolution R Enterprise 8.0 (64)**.

**For Windows 8 and Windows Server 2012:**

-   Move the pointer to the lower left corner of the Desktop until the **Start** icon appears.
-   Click **Start** to view the **Start** screen.
-   Locate and click the tile for **Revolution R Enterprise 8.0 (64)**.

**For Windows 10**

-   From the **Task Bar**, click **Start**.
-   Click **All apps**.
-   Click **Revolution R**.
-   Click **Revolution R Enterprise 8.0 (64)**.

The Revolution R Productivity Environment opens. The Revolution R program group includes a variety of documents, including the Microsoft R Getting Started Guide and the RevoScaleR Getting Started Guide. These provide tutorial introductions to working with Microsoft R Server.

The RevoIOQ package provides a set of tests to verify correct installation and operation of Revolution R Enterprise. To run these tests, run the following commands from your R prompt:

	library(RevoIOQ)
	RevoIOQ()

A fresh install of Revolution R Enterprise should yield a report (which will appear in a browser window) that contains no Errors or Failures, though there may be some Deactivated Tests.

To install DeployR, follow the instructions [here](http://go.microsoft.com/fwlink/?LinkID=708387&clcid=0x409).

## Sample Data

Sample data sets for use with Revolution R Enterprise can be found [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409).
