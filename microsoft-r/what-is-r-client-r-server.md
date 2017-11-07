---

# required metadata
title: "Microsoft R Server vs R Client: Scale"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "05/10/2017"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
  - r-client
  - r-server
#ms.custom: ""

---


# R Server vs. R Client: Scale

[Microsoft R Server](what-is-microsoft-r-server.md) and [Microsoft R Client](r-client/what-is-microsoft-r-client.md) offer virtually identical packages, but each one targets different scenarios. R Client is intended for data scientists who create solutions that run locally. R Server is commercial software that runs on a range of platforms, at much greater scale, with infrastructure for handling major workloads, on client-server topologies that support remote access over authenticated connections. You can learn and develop on R Client, and then migrate your work to R Server or execute it remotely on an R Server whenever you need the scale, support, and infrastructure of an operationalized server.

On R Server, the ScaleR technology in the RevoScaleR package offers almost unbounded scale in running R workloads in parallel and distributed configurations. Although you can call ScaleR functions on a system having just R Client, ScaleR is throttled on R Client: datasets must fit in memory, and processing is capped at a maximum of two processors on the local system. Only R Server gives you ScaleR at full capacity, with support for remote compute context, data chunking, parallelization, and distributed workloads.

Given a platform that supports it, functions in ScaleR provide high performance, parallelized, and distributable analytics functions that scale from small data sets in memory to huge data sets stored on disk on a cluster of computers. The analytics functions provided include summary statistics, cubes and crosstabs, linear models, logistic regression, generalized linear models, kmeans clustering, decision trees, and decision forests. These algorithms are parallelized and distributed automatically, and process data in chunks so that all of your data does not need to be in memory at one time; you can use the same analysis code for your giant data set as you do for a small data set in memory.

ScaleR also provides traditional high performance computing (tools if you prefer to construct your own distributed computations. In addition, in many environments, there are full-featured tools for data cleaning and manipulation.

Functions in ScaleR are prefixed with ‘rx’ for analysis and data manipulation. Additionally, use ‘rxExec’ for high performance computing. If you are computing decision trees, also check out the included RevoTreeView package that allows you to interactively visualize your decision trees.

## See Also

[What's new in R Server](whats-new-in-r-server.md)
[What's new in R Client](r-client/whats-new-in-r-client.md)