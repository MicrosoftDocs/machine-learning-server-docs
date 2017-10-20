--- 
 
# required metadata 
title: " RevoPemaR: Parallel External Memory Algorithms in R " 
description: "A package that provides a framework for creating Parallel External Memory Algorithms in R using R Reference Classes." 
keywords: "RevoPemaR, RevoPemaR-package, package" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "08/21/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 #RevoPemaR package

The **RevoPemaR** package provides a framework for creating Parallel External Memory Algorithms in R using R Reference Classes.

| Package details | |
|--------|-|
| Version: |  10.0.0 |
| Runs on: | [Machine Learning Server (Hadoop)](../../install/machine-learning-server-hadoop-install.md)  
| Built on: | R 3.4.1 (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|
 
## How to use RevoPemaR

When used with the **RevoScaleR** package, analyses can be distributed automatically on Hadoop clusters using Cloudera's CDH or Hortonworks' HDP. For additional documentation, see [Get started with PemaR functions in Microsoft R](https://msdn.microsoft.com/microsoft-r/pemar-getting-started).

In an R session, load **RevoPemaR** from the command line by typing`library(RevoPemaR)`.

> [!Note]
> You can load this library on computer that does not have Hadoop (for example, on an R Client instance) if you change the compute context to Hadoop MapReduce or Spark and execute the code in that compute context.

## Function list

|Class | Description |
|------|-------------|
|[PemaBaseClass](pemabaseclass-class.md) |A base reference class generator for parallel external memory algorithms.|
|[setPemaClass](setpemaclass.md)|Returns a generator function for creating a parallel external memory algorithm reference class.|
|[pemaCompute](pemacompute.md) |Estimates a parallel external memory algorithm as described by a PEMA reference class object. |

## Next steps

Add R packages to your computer by running setup for R Server or R Client: 

+ [R Client](../../r-client/what-is-microsoft-r-client.md) 
+ [R Server](../../what-is-microsoft-r-server.md)

## See also

 [Package Reference](../introducing-r-server-r-package-reference.md)    