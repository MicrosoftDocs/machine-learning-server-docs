--- 
 
# required metadata 
title: " RevoPemaR: Parallel External Memory Algorithms in R" 
description: "A package that provides a framework for creating Parallel External Memory Algorithms in R using R Reference Classes." 
keywords: "RevoPemaR, RevoPemaR-package, package" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
ms.topic: "reference" 
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
ms.technology: "r-server" 
ms.custom: "" 
 
--- 
 
 #`RevoPemaR-package`:  RevoPemaR: Parallel External Memory Algorithms in R 

 Applies to version 8.0.3 of package RevoPemaR.
 
 ##Description
 
A package that provides a framework for creating Parallel External Memory Algorithms in R using R Reference Classes.

When used with the **RevoScaleR** package, analyses can be distributed automatically on Hadoop clusters using Cloudera's CDH or Hortonworks' HDP, Teradata appliances, IBM Platform Computing LSF clusters, and Windows HPC Server clusters. For additional documentation, see [Get started with PemaR functions in Microsoft R](https://msdn.microsoft.com/microsoft-r/pemar-getting-started).

 
 ##Details
 
| Col  1 | Col  2 |
| :---| :--- |
|  Package:  |  RevoPemaR |
|  Type:  |  Package |
|  Version:  |  0.1.0 |
|  License:  |  Apache License 2.0 |
|  LazyLoad:  |  yes |

## Class library

|Class | Description |
|------|-------------|
|[`PemaBaseClass`](pemabaseclass-class.md) |A base reference class generator for parallel external memory algorithms.|
|[`setPemaClass`](../../pemar/packagehelp/setpemaclass.md)|Returns a generator function for creating a parallel external memory algorithm reference class.|
|[`pemaCompute`](pemacompute.md) |Estimates a parallel external memory algorithm as described by a PEMA reference class object. |

## Get help on RevoPemaR functions from the R console

To see the **RevoPemaR** functions that can be called from the R console:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
2. Load `RevoPemaR` from the command line by typing `library(RevoPemaR)`.
1. In the console, open the package help by typing the following at the R prompt: `help(package="RevoPemaR")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.
 
> [!NOTE]
> To list all public functions, type library(help="RevoPemaR") at the R prompt.
>

## See also

[Package Reference](../../package-reference.md)

[Install R Server](../../rserver.md)

[Install R Client](../../r-client/what-is-microsoft-r-client.md)