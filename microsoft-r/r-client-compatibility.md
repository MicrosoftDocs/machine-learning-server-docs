---

# required metadata
title: "R Client Compatibility Chart"
description: "Microsoft R compatibility with R client"
keywords: ""
author: "j-martens"
manager: "paulette.mckay"
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

# Microsoft R Client / R Server Compatibility

Microsoft R Client is a free, community-supported, data science tool for high performance analytics.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as SQL Server R Services and R Server for Hadoop. Learn more about the compatibility in the table below.

> Build 1.x of R Client runs on Windows only. Greater operating system support is coming soon.

Microsoft R Client works with the following flavors of Microsoft R Server: 


|Version Compatibility   |R Client (build 1.0.0)|R Client (build 1.1.0)|
|-----------|:--------------------------:|:--------------------------:|
|Microsoft R Server for Linux|R Server for Linux 2016<br>(build 8.0.5)|R Server for Linux 2016<br>(build 8.0.5)|
|Microsoft R Server for Teradata DB|R Server for Teradata DB 2016<br>(build 8.0.5)|R Server for Teradata DB 2016<br>(build 8.0.5)|
|Microsoft R Server for Hadoop|R Server for Hadoop 2016<br>(build 8.0.5) _with tweaks_ <b>*</b> |R Server for Hadoop 2016<br>(build 8.0.5)|
|Microsoft R Server (Standalone - Windows)|SQL Server 2016<br>(RTM)|SQL Server 2016<br>(RTM)|
|SQL Server R Services|SQL Server 2016<br>(RTM)|SQL Server 2016<br>(RTM)|
|R Server for HDInsight|R Server for HDInsight<br>(preview)|R Server for HDInsight<br>(preview)|

<br>

><b>*</b> <b>Build 1.0.0</b> will work with Microsoft R Server for **Hadoop** 2016 build 8.0.5 if you do the following:
>1. Install the `RTools` package from CRAN onto the machine running R client.
>1. Explicitly add `. /usr/lib64/microsoft-r/8.0/hadoop/RevoHadoopEnvVars.site` to `/etc/profile` file.