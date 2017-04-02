--- 
 
# required metadata 
title: " RevoPemaR: Parallel External Memory Algorithms in R " 
description: " A package that provides a framework for creating Parallel External Memory Algorithms in R using R Reference Classes.  When used with the **RevoScaleR** package, analyses can be distributed automatically  on Hadoop clusters using Cloudera's CDH or Hortonworks' HDP, Teradata appliances, IBM Platform Computing LSF clusters, and Windows HPC Server clusters.    " 
keywords: "RevoPemaR, RevoPemaR-package, package" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
A package that provides a framework for creating Parallel External Memory Algorithms in R
using R Reference Classes.

When used with the **RevoScaleR** package, analyses can be distributed automatically 
on Hadoop clusters using Cloudera's CDH or Hortonworks' HDP, Teradata appliances,
IBM Platform Computing LSF clusters, and Windows HPC Server clusters. 
 
 
 
 ##Details
 
| Col  1 | Col  2 |
| :---| :--- |
|  Package:  |  RevoPemaR |
|  Type:  |  Package |
|  Version:  |  0.1.0 |
|  License:  |  Apache License 2.0 |
|  LazyLoad:  |  yes |
 
To get started, read the manual "RevoPemaR Getting Started Guide"
(doc/RevoPemaR_Getting_Started.pdf). The key functions in the package
are as follows (to list all public functions, type library(help="RevoPemaR")
at the R prompt):


###`PemaBaseClass`
a base reference class generator for parallel external memory algorithms.


###`setPemaClass`
returns a generator function for creating a parallel external  memory algorithm reference class


###`pemaCompute`
estimates a parallel external memory algorithm as described by a PEMA reference class object



 
 
 ##Author(s)
 Microsoft Corporation [mrspack@microsoft.com](mrspack@microsoft.com)
 
 
 
