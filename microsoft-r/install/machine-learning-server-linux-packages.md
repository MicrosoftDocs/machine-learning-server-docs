---
# required metadata
title: "Add or remove R and Python on Machine Learning Server for Linux"
description: "How to install or uninstall R and Python packages on Machine Learning Server forLinux."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "07/31/2018"
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

You can add open-source and third-party R and Python packages to the same local repository containing the product-specific packages for Machine Learning Server, allowing you to call functions from any library in the same script. Any packages you add to the repository must be compatible with the base R and Anaconda distributions upon which Microsoft's machine learning libraries are built.

## Version requirements

Product-specific packages like RevoScaleR and revoscalepy are built on base libraries of R and Python respectively.  Any new packages that you add must be compatible with the base libraries installed with the product. Upgrading or downgrading the version of R or Python installed by Machine Learning Server setup is not supported.

Base R is distributed through Microsoft R Open, as installed by Machine Learning Server or R Server.

| Product version | R version |
|-----------------|-----------|
| 9.3             | 3.4.3 |
| 9.2             | 3.4.1 |
| 9.1             | 3.3.3 |

Python is distributed though Anaconda, also installed by Machine Learning server.

| Product version | Anaconda/Python version |
|-----------------|-------------------------|
| 9.3             | 4.2/3.5 |
| 9.2.1           | 4.2/3.5 |

To verify the base library versions on your system, start a command line tool and check the version information displayed when the tool starts. 

For R, from `/Home` or any other working directory: `[<path>] $ Revo64`

    ```
    R version 3.4.3 (2017-11-30) -- "Kite-Eating Tree"
    Copyright (C) 2017 The R Foundation for Statistical Computing
    Platform: x86_64-w64-mingw32/x64 (64-bit)
    ```

For Python, do this: `[<path>] $ mlserver-python`

    ```
    Python 3.5.2 |Anaconda 4.2.0 (64-bit)| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)] on win32
    Type "help", "copyright", "credits" or "license" for more information.
    ```


## Package location

On Linux, packages installed and used by Machine Learning Server can be found at these locations:

+ For R: `/opt/microsoft/mlserver/9.3.0/`

+ For Python: `/opt/microsoft/mlserver/9.3.0/runtime/python/pkgs`

## Add or remove R packages

R packages tend to have with multiple dependencies so we generally recommend using a tool like miniCran. For more information and alternative methodologies, see [R package management](../operationalize/configure-manage-r-packages.md).

## Add or remote Python packages

To install Python 3.5 compatible packages, run the following command with sudo or root permissions: `/opt/microsoft/mlserver/9.3.0/runtime/python/bin/pip install <packagename>`

To uninstall any Python packages that you previously added, reverse the action using the same executable (also with elevated permissions): `/opt/microsoft/mlserver/9.3.0/runtime/python/bin/pip uninstall <packagename>`

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)