---

# required metadata
title: "Set compute context in Microsoft R"
description: "Push a compute context to R Server on a different platform for remote execution."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/30/2017"
ms.topic: "article"
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

# Set compute context in Microsoft R

**Applies to: Microsoft R Client, Microsoft R Server**

*Compute context* refers to the location of RevoScaleR computations, as executed by Microsoft R product. In Microsoft R, every calculation has a compute context. The default is a local compute context, available in both R Client and R Server, but if you have R Server as a second resource, you can push computations from the local system to the other R Server instance.

Use case | Description | 
---------|-------------|
R Client to R Server | Write and run script locally in R Client, pushing specific computations to a remote R Server instance. |
R Server to R Server | Push platform-specific computations to an R Server on a different platform. Supported platforms include SQL Server, Teradata, Hadoop (Spark or MapReduce) |

## Prerequisites

Use same-version R Server instances, or compatible co-released versions of R Client and R Server (for example, R Client 3.3.3. and R Server 9.1). The specific dependency is to have the same version of RevoScaleR, but since RevoScaleR is only installed through R Client or R Server, the version prerequisite is satisfied in the product installation.

## Get compute context

At an R command prompt, run `rxGetComputeContext()` to return the compute context. Every platform supports the default local compute context: **RxLocalSeq**. Every R session that loads the RevoScaleR function library has a base **RxComputeContext** object.

## Compute context on various platforms and data sources

matrix

Common properties and operations



## See Also

 [Introduction to R Server](rserver.md) 
 [Install R Server on Windows](rserver-install-windows.md)  
 [Install R Server on Linux](rserver-install-linux-server.md)  
 [Install R Server on Hadoop](rserver-install-hadoop.md)