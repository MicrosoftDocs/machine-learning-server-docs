---

# required metadata
title: "Introducing Microsoft R Server"
description: "Microsoft R Server introduction"
keywords: ""
author: "HeidiSteen"
manager: "paulettem"
ms.date: "09/02/2016"
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
  - r-server
ms.custom: ""

---

# Introducing Microsoft R Server

**R Server** is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers (Linux and Windows) and clusters (Hadoop and Apache Spark). R Server is the execution engine for solutions built on ScaleR functions, targeting statistical and analytical operations. ScaleR can detect and use all available processors on the same machine or on nodes within the same cluster.

In a progression of server capacity and capability, R Server supercedes R Client. A solution that runs locally against R Client, bound by the memory and storage of a single machine, can be deployed to R Server with minimal or no changes. While both R Client and R Server support ScaleR, R Server can use more processors and multiple disks, enabling big-data scenarios for which R Server is designed.

R Server is the next generation of the former Revolution R Enterprise server, acquired in recent years by Microsoft and distributed commercially for these platforms: Linux, Hadoop, Teradata. It's also available for Windows as a feature of SQL Server. See [Supported Platforms](supported-platforms.md) for details.

## See Also

[Learning Resources](microsoft-r-more-resources.md)

[Getting Started with ScaleR](scaler-getting-started.md)
