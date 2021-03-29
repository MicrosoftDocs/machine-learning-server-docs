---
title: "Add or remove R and Python on Machine Learning Server for Linux"
description: "How to install or uninstall R and Python packages on Machine Learning Server forLinux."
keywords: 
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 07/31/2018
ms.topic: "how-to"
ms.prod: "mlserver"
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

**Applies to:  Machine Learning Server 9.2.1 | 9.3 | 9.4**

You can add open-source and third-party R and Python packages to the same local repository containing the product-specific packages for Machine Learning Server, allowing you to call functions from any library in the same script. Any packages you add to the repository must be compatible with the base R and Anaconda distributions upon which Microsoft's R and Python libraries are built.

## Version requirements

Product-specific packages like RevoScaleR and revoscalepy are built on base libraries of R and Python respectively.  Any new packages that you add must be compatible with the base libraries installed with the product. 

Upgrading or downgrading the base R or Python libraries installed by setup is not supported. Microsoft's proprietary packages are built on specific distributions and versions of the base libraries. Substituting different versions of those libraries could destabilize your installation.

Base R is distributed through Microsoft R Open, as installed by Machine Learning Server or R Server. Python is distributed though Anaconda, also installed by Machine Learning server.

| Product version | R version | Anaconda/Python version |
|-----------------|-----------|-------------------------|
| 9.4             | 3.5.2 | /3.7.1 |
| 9.3             | 3.4.3 |  4.2/3.5 |
| 9.2             | 3.4.1 |  4.2/3.5 |
| 9.1             | 3.3.3 |  not applicable |

To verify the base library versions on your system, start a command line tool and check the version information displayed when the tool starts. 

For R, from `/Home` or any other working directory: `[<path>] $ Revo64`

`R version 3.4.3 (2017-11-30) -- "Kite-Eating Tree"
Copyright (C) 2017 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)`

For Python, do this: `[<path>] $ mlserver-python`

`Python 3.5.2 |Anaconda 4.2.0 (64-bit)| (default, Jul  2 2016, 17:53:06) [GCC 4.4.7 20120313] on linux
Type "help", "copyright", "credits" or "license" for more information.`


## Package location

On Linux, packages installed and used by Machine Learning Server can be found at these locations:

+ For R: `/opt/microsoft/mlserver/9.4.7/runtime/R/library`

+ For Python: `/opt/microsoft/mlserver/9.4.7/runtime/python/pkgs`

## Add or remove R packages

R packages tend to have with multiple dependencies so we generally recommend using a tool like miniCran. For more information and alternative methodologies, see [R package management](../operationalize/configure-manage-r-packages.md).

## Add or remove Python packages

Anaconda includes **pip** and **conda** that you can use to add or remove Python packages. When adding or removing packages, keep the following points in mind:

+ Install as root or super user.
+ For utilities not in the PATH, prepend with `mlserver-python -m`, as in `mlserver-python -m pip install <package-name>` (or equivalent for **conda**).  Alternatively, you could do this: `./pip install <package-name>`.

**Using pip**

```
# Add a package
cd /opt/microsoft/mlserver/9.4.7/runtime/python/bin/
pip install <packagename>

# Remove a package
cd /opt/microsoft/mlserver/9.4.7/runtime/python/bin/
pip uninstall <packagename>
```

**Using conda**

```
# Add a package
cd /opt/microsoft/mlserver/9.4.7/runtime/python/bin/
conda install <packagename>

# Remove a package
cd /opt/microsoft/mlserver/9.4.7/runtime/python/bin/
conda uninstall <packagename>
```

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)