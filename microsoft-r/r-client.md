---

# required metadata
title: "Introducing Microsoft R Client"
description: "Microsoft R Client"
keywords: "R Client, Microsoft R Client, Introduction, Get Started with R Client"
author: "j-martens"
manager: "jhubbard"
ms.date: "5/15/2017"
ms.topic: "get-started-article"
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

# About Microsoft R Client

Microsoft R Client is a free, [community-supported](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MicrosoftR), data science tool for high performance analytics.  R Client is built on top of [Microsoft R Open](https://mran.microsoft.com/open/) so you can use any open source R package to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](scaler-getting-started.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as [SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility](r-client-compatibility.md). 

You can also run your code remotely on R Server using [remoteLogin() or remoteLoginAAD()](operationalize/remote-execution.md) from the `mrsdeploy` package to offload heavy processing on server or to test your analytics during their development. 
 
Learn how to [install and get started with Microsoft R Client](r-client-get-started.md).

## How do you use R Client

Microsoft R Server and Microsoft R Client offer virtually identical packages, but each one targets different scenarios. R Client is intended for data scientists who create solutions that run locally. R Server is commercial software that runs on a range of platforms, at much greater scale, with infrastructure for handling major workloads, on client-server topologies that support remote access over authenticated connections. 

You can work with R Client standalone. You can also use it with R Server, where you learn and develop on R Client, and then migrate your work to R Server or execute it remotely on an R Server whenever you need the scale, support, and infrastructure of an operationalized server. 

## Compatibility with R Server

Microsoft R Client works with the following flavors of Microsoft R Server:

|R Server 9.1 Offerings|Compatible with R Client 3.3.3|
|-----------|:--------------------------:|
|Microsoft R Server for Linux|8.0.5 - 9.1|
|Microsoft R Server for Teradata DB|8.0.5 - 9.1|
|Microsoft R Server for Hadoop|8.0.5 - 9.1|
|Microsoft R Server for Windows|9.1|
|SQL Server R Services <br>SQL Server Machine Learning Services<br>(in-database & standalone)|SQL Server 2016 RTM - <br>SQL Server 2017 CTP2|
|HDInsight (cluster type: R Server)|8.0.x - 9.1|