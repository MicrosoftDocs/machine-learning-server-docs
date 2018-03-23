---

# required metadata
title: "R Execution Security Considerations and user isolation - Machine Learning Server "
description: "Learn about security considerations with `deployr-rserve` which is a forked version of RServe maintained by Microsoft. This tool is used when operationalizing analytics with Machine Learning Server"
keywords: "RServe; deployr-rserve; user isolation"
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

# R Execution Security Considerations

**Applies to: Machine Learning Server, Microsoft R Server 9.x**

[`deployr-rserve`](https://github.com/Microsoft/deployr-rserve) is a forked version of RServe maintained by Microsoft. In this forked version, parallel R sessions are supported for both Windows and Linux thereby overcoming this limitation in the original rserve package.

This forked version of RServe is the R execution component behind the compute node for Machine Learning Server (and R Server). Compute nodes are used to execute R code as a session or service. Each compute node has its own [pool of R shells](configure-evaluate-capacity.md#pool).

This RServe fork acts as an interface to R, which by default is single threaded. However, if you use [RevoScaleR package functions](https://docs.microsoft.com/en-us/machine-learning-server/r-reference/revoscaler/revoscaler#functions-by-category), you benefit from multi-threaded processing in the R shell.

## The Execution Context

Machine Learning Server provides various [API calls](concept-api.md) that permit the execution of R scripts and R code. All authentication takes place on the web node, and the execution of the R code is managed through Machine Learning Server's custom version of RServe. RServe provides a TCP/IP interface to the R Interpreter running on the machine. By default, Rserve runs on the same machine as the compute node. RServe is started by Windows Service (RServeWinService) that runs under a virtual service account with low privileges. RServe inherits the permissions of that virtual service account. In the default configuration, Rserve only accepts socket connections from `localhost`. In other words, only those processes running on the same machine where RServe is running can directly connect to it and execute R code. The connection from the Compute node to RServe can be further secured by using a username and a password. If your configuration requires additional compute capacity, you can add [additional compute nodes](../operationalize/configure-machine-learning-server-enterprise.md) for more sophisticated load-balancing capabilities.

<a name="isolation"></a>	
	
## Directory & User Isolation Considerations

In the R language, users can change files in the file system, download content from the web, download packages, and so on.	

In order to mitigate some of the risks associated with RServe, the service is set up to run using **a single locked down account with write permissions** ONLY to the R working directory <MRS_home>\o16n\Rserve\workdir, which is the directory under which R sessions and service calls store artifacts, files, and workspaces. While the custom Rserve service can only write to the working directory, there is no user isolation between the session folders. However, all sessions execution requests can only be initiated by authenticated users and you can further control user permissions to services using [RBAC](https://docs.microsoft.com/en-us/machine-learning-server/operationalize/configure-roles).