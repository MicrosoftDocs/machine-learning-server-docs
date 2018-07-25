---
# required metadata
title: "Package management for R and Python on Machine Learning Server for Windows"
description: "How to add or remove R and Python packages on Machine Learning Server for Windows."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "07/24/2018"
ms.topic: "conceptual"
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

# How to add or remove R and Python packages on Machine Learning Server for Windows

**Applies to:  Machine Learning Server 9.2.1 | 9.3**

You can add open source and third-party R and Python packages to the local repository for Machine Learning Server. Any packages you add to the repository must be compatible with the base R and Anaconda distributions upon which Microsoft's machine learning libraries are built.

## Base R and Python versions

For Machine Learning Server 9.2.1 and 9.3, base R is Microsoft R Open (MRO) distribution as installed by the Machine Learning Server setup program. Setup also installs Anaconda when you add Python support.. Version information for each language is documented in the installation instructions. You can also get version information from specific packages.

You cannot independently upgrade, delete, or substitute the packages installed by Setup. Microsoft packages such as RevoScaleR and revoscalepy are built on specific versions of R and Python, respectively. Modifying the base language distributions that form the basis of your installation is destabilizing, unsupported, and could prevent the future application of service packs and cumulative updates on your computer. 

Upgrades to R and Python base languages are rolled out in version upgrades to Machine Learning Server. For example, release 9.2.1 was built on R 3.3.3, release 9.3 was built on 3.4.1.


## Package location

Python packages can be found at `C:\Program Files\Microsoft\ML Server\PYTHON_SERVER\Lib\site-packages`

For other package management tips, see []() and []().

## Add or remove R packages

## Add or remove Python packages

To install Python 3.5 compatible packages, run the following command with elevated permissions: `C:\Program Files\Microsoft\ML Server\PYTHON_SERVER\Scripts\pip.exe install <packagename>`

To uninstall any Python packages that you previously added, reverse the action using the same executable (also with elevated permissions): `C:\Program Files\Microsoft\ML Server\PYTHON_SERVER\Scripts\pip.exe uninstall <packagename>`

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)