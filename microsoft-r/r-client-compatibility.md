---

# required metadata
title: "R Client Compatibility Chart"
description: "Microsoft R compatibility with R client"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "07/16/2016"
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
ms.technology: 
  - r-client
ms.custom: ""

---

# Microsoft R Client Compatibility

Microsoft R Client is a free, community-supported, data science tool for high performance analytics.  R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  [Learn more...](microsoft-r-getting-started.md#mrc)

On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as SQL Server R Services and R Server for Hadoop. Learn more about the compatibility in the table below.

Microsoft R Client is free to everyone. Download Microsoft R Client at http://aka.ms/rclient/download and [install](r-client-install.md) it today. Don't miss this [Microsoft R Getting Started](microsoft-r-getting-started.md) guide.

## Microsoft R Client build 1.0.0

Microsoft R Client build 1.0.0 works with the following flavors of R Server. 

> Build 1.0.0 of R Client runs on Windows only. Greater operating system support is coming soon.

|Version Compatibility   |R Client (build 1.0.0)|
|-----------|:--------------------------:|
|Microsoft R Server for Linux|R Server for Linux 8.0.5|
|Microsoft R Server for Teradata DB|R Server for Teradata DB 8.0.5|
|Microsoft R Server for Hadoop|R Server for Hadoop 8.0.5<b>*</b> |
|Microsoft R Server (Standalone - Windows)|SQL Server 2016<br>(RTM)|
|SQL Server R Services|SQL Server 2016<br>(RTM)|
|R Server for HDInsight|R Server for HDInsight<br>(preview)|

><b>*</b> For Microsoft R Client build 1.0.0 to work with Microsoft R Server for Hadoop 8.0.5, you must:
>1. Install the `RTools` package from CRAN onto the machine running R client.
>1. Explicitly add `. /usr/lib64/microsoft-r/8.0/hadoop/RevoHadoopEnvVars.site` to `/etc/profile` file.
