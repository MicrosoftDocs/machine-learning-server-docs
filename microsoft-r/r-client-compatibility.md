---

# required metadata
title: "R Client Compatibility Chart"
description: "Microsoft R compatibility with R client"
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
ms.technology:
  - r-client
ms.custom: ""

---

# Microsoft R Client / R Server Compatibility

Microsoft R Client is a free, [community-supported](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MicrosoftR), data science tool for high performance analytics.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as SQL Server Machine Learning Services and R Server for Hadoop. Learn more about the compatibility in the table below.

Microsoft R Client can be downloaded from http://aka.ms/rclient (Windows) and http://aka.ms/rclientlinux (Linux). Learn more in this [Microsoft R Getting Started](microsoft-r-getting-started.md) guide.

Microsoft R Client works with the following flavors of Microsoft R Server:

|R Server 9.1 Offerings|Compatible with R Client 3.3.3|
|-----------|:--------------------------:|
|Microsoft R Server for Linux|8.0.5 - 9.1|
|Microsoft R Server for Teradata DB|8.0.5 - 9.1|
|Microsoft R Server for Hadoop|8.0.5 - 9.1|
|Microsoft R Server for Windows|9.1|
|SQL Server R Services <br>SQL Server Machine Learning Services<br>(in-database & standalone)|SQL Server 2016 RTM - <br>SQL Server 2017 CTP2|
|HDInsight (cluster type: R Server)|8.0.x - 9.1|