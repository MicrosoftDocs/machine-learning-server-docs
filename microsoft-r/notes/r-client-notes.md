---

# required metadata
title: "R Client Release Notes"
description: "R Client Readme"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "06/13/2016"
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
ms.technology: "r-client"
ms.custom: ""

---

#What's New in Microsoft R Client

## Microsoft R Client 1.1

The following release notes apply to Microsoft R Client, which can be downloaded from http://aka.ms/rclient/download.

+ New enhanced R Client Getting Started
+ An auto-update feature is now available using `CheckForUpdates()` in your R Console
+ The R Client setup experience was enhanced:
  + Installer was simplified when RTVS was unbundled with the installer. [Learn about available R IDEs...](../r-client-get-started.md#try-r-client)
  + [Offline installations](../r-client-get-started.md#installrclient) are now possible
+ New packages now bundled with Microsoft R Client (and Microsoft R Open):
  + `mrsdeploy` - for remote execution and deployment
  + `MML`
  + `olapR`
  + and the [packages bundled with Microsoft R Open 3.2.2](https://mran.microsoft.com/rro/installed/#enhance)
+ Updated end-user license agreement
+ Telemetry collection is now enabled. To turn this feature off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.
 
## Microsoft R Client 1.0

The following release notes apply to Microsoft R Client 1.0:

+ Microsoft R Client is a free, community-supported, data science tool for building high performance analytics using the full set of ScaleR functions.  

+ R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. 

+ Additionally, R Client introduces the powerful ScaleR technology and its proprietary functions to benefit from parallelization and remote computing. 

+ R Client is in-memory bound, meaning that it can only process datasets that fit into the available local memory. Processing is also limited to up to two threads for ScaleR functions.

+ With Microsoft R Client, you have the ability to push the compute context to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop in order to benefit from disk scalability, greater performance and speed. 

[See here](../index.md#compare-prods) for a comparison between R Client and R Server. 