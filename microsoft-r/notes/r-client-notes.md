---

# required metadata
title: "R Client Release Notes"
description: "R Client Readme"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "4/19/2017"
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

# What's New in Microsoft R Client

The following release notes apply to Microsoft R Client.

[See here](../overview-interoperability.md#compare-prods) for a comparison between R Client and R Server.


## Microsoft R Client 3.3.3

This release of R Client, built on open source R 3.3.3. Key features in this release include the following:

+ Download R Client from http://aka.ms/rclient (Windows) or http://aka.ms/rclientlinux (Linux).

+ Installations on Linux are now possible for R Client

+ Several packages installed with this release have been updated. Learn more about [the new and updated packages in this release](../rserver-whats-new.md#rclient333-package-updates).

## Microsoft R Client 3.3.2

The following release notes apply to Microsoft R Client, which can be downloaded from http://aka.ms/rclient/download.

+ New enhanced Microsoft R Client [Getting Started](../r-client/what-is-microsoft-r-client.md) guide
+ A new 'version check' and auto-update is now possible using CheckForUpdates() in your R Console 
+ [Offline installations](../r-client/what-is-microsoft-r-client.md#installrclient) are now possible for R Client
+ New packages now bundled with Microsoft R Client (and Microsoft R Open):
  + `mrsdeploy` - adds remote execution and web service deployment from R Client 3.3.2 to a remote R Server 9.0.1 instance
  + `MicrosoftML` - adds machine learning algorithms to R script that executes on either R Client or R Server
  + `olapR`- adds MDX query support through connections to OLAP cubes on a SQL Server 2016 Analysis Services instance
  + and the packages bundled with [Microsoft R Open 3.3.2](https://mran.microsoft.com/rro/installed/#enhance)
  
  Learn more about [the new and updated packages in this release](../rserver-whats-new.md#new-and-updated-packages).
+ Updated end-user license agreement
+ Telemetry collection is now enabled. [Learn more](../rserver-optout-telemetry.md) about this feature and how to turn it off.

## Microsoft R Client 1.0.0

The following release notes apply to Microsoft R Client 1.0:

+ Microsoft R Client is a free, community-supported, data science tool for building high performance analytics using the full set of ScaleR functions.  

+ R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics.

+ Additionally, R Client introduces the powerful ScaleR technology and its proprietary functions to benefit from parallelization and remote computing.

+ R Client is in-memory bound, meaning that it can only process datasets that fit into the available local memory. Processing is also limited to up to two threads for ScaleR functions.

+ With Microsoft R Client, you have the ability to push the compute context to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop in order to benefit from disk scalability, greater performance and speed.

