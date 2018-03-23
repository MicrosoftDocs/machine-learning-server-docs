---

# required metadata
title: "R Execution Context - Machine Learning Server "
description: "Learn about execution context with `deployr-rserve` is a forked version of RServe maintained by Microsoft. This tool is used when operationalizing analytics with Machine Learning Server"
keywords: "RServe; deployr-rserve;"
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# R Execution Context

**Applies to: Machine Learning Server, Microsoft R Server 9.x**

[`deployr-rserve`](https://github.com/Microsoft/deployr-rserve) is a forked version of RServe maintained by Microsoft. In this forked version, we support parallel R sessions for both Windows and Linux thereby overcoming this limitation in the original rserve package.

This forked version of RServe is the R execution component behind the compute node for Machine Learning Server (and R Server). Compute nodes are used to execute R code as a session or service. Each compute node has its own [pool of R shells](configure-evaluate-capacity.md#pool).  

This RServe fork acts as an interface to R, which by default is single threaded. However, in this context, this RServe fork sits atop of the RevoScaleR package. Therefore, if you use RevoScaleR package functions, you benefit from multi-threaded processing in the R shell.

## The Execution Context

Machine Learning Server provides various [API calls](concept-api.md) that permit the execution of R scripts and R code. All authentication takes place on the web node, and the execution of the R code is managed through Machine Learning's custom version of RServe add-on component. RServe provides a TCP/IP interface to the R Interpreter running on the machine. By default, Rserve runs on the same machine as the compute node. RServe is started by Windows Service (RServeWinService) that runs under a virtual service account with low privileges. RServe inherits the permissions of that virtual service account. In the default configuration, Rserve only accepts socket connections from `localhost`. In other words, only those processes running on the same machine where RServe is running can directly connect to it and execute R code.If your configuration requires additional compute capacity, you can add [additional compute nodes](../operationalize/configure-machine-learning-server-enterprise.md) for more sophisticated load-balancing capabilities.
