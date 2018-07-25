---
# required metadata
title: "Package management for R and Python on Machine Learning Server for Linux"
description: "How to add or remove R and Python packages on Machine Learning Server forLinux."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "07/25/2018"
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

# How to add or remove R and Python packages on Machine Learning Server for Linux

**Applies to:  Machine Learning Server 9.2.1 | 9.3**

You can add open-source and third-party R and Python packages to the same local repository containing the product-specfic packages for Machine Learning Server, allowing you to call functions from any library in the same script. 

To add packages, start by getting the following information:

> [!div class="checklist"]
> * Versions of the base R and Python distributions installed with Machine Learning Server
> * Versions of any new packages you want to install (to verify compatibility)
> * Location of the local repository for R and Python packages

As a developer or analyst, you might have multiple versions of R and Python on your computer. If you are augmenting your Machine Learning Sever installation with other packages, make sure the packages you add can run on the R and Python execution environment of the server. Any new packages must be placed in the same location as those provided in Machine Learning Server.

## Base R and Python distributions installed with Machine Learning Server

Machine Learning Server includes R and Python execution environments in addition to the proprietary packages like RevoScaleR, revoscalepy, and others. Proprietary packages are built on top of specific versions of R and Python. 

For this reason, you cannot manually upgrade the R and Python distributions installed with the server. Modifying the base language distributions that form the basis of your installation is destabilizing, unsupported, and could prevent the future application of service packs and cumulative updates on your computer. 

Upgrades to R and Python base languages are rolled out in version upgrades to Machine Learning Server. 

| Server release | R version | Python version| 
|----------------|-----------|---------------+
| 9.3        | Microsoft R Open (MRO) 3.4.3 | Anaconda 4.2 with Python 3.5.2 |
| 9.2        | MRO 3.4.1 | Anaconda  4.2 with Python 3.5.2 |

 On your system, you can get base R and Python version information any number of ways using built-in tools and utilities. For example, for R, you could type `library(help="base")` at an R command prompt. For more information about getting package information, see [R package reference for Machine Learning Server](../r-reference/introducing-r-server-r-package-reference.md) and [Python package reference for Machine Learning Server](../python-reference/introducing-python-package-reference.md).

## Local repository for package installation

Packages installed and used by Machine Learning Server can be found at these locations:

+ For R:   

+ For Python: `/opt/microsoft/mlserver/9.3.0/runtime/python/pkgs`

## Add or remove R packages


## Add or remove Python packages

To install Python 3.5 compatible packages, run the following command with sudo or root permissions: `/opt/microsoft/mlserver/9.3.0/runtime/python/bin/pip install <packagename>`

To uninstall any Python packages that you previously added, reverse the action using the same executable (also with elevated permissions): `/opt/microsoft/mlserver/9.3.0/runtime/python/bin/pip uninstall <packagename>`

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)