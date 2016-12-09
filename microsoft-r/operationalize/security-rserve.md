---

# required metadata
title: "R Execution Security Considerations | Microsoft R Server Docs"
description: "R Execution Security Considerations for Operationalization with Microsoft R Server"
keywords: "RServe"
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
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
  - deployr
  - r-server
ms.custom: ""
---

# R Execution Security Considerations

RServe is the R execution component for the operationalization compute node for Microsoft R Server.

>[!IMPORTANT]
>Microsoft R Server's operationalization feature is not designed for multi-tenancy. Please follow your organization's best practices to prevent data leakage.

## The Execution Context

As per the standard usage of R, the current user starts the R executable and interacts with the application via the R Language and the R Interpreter. The R language provides OS-level access via the `system` function. With this function, a user can execute an OS command such as `system(“rmdir –r C:\\tmp”)`. While this is useful functionality for individual users, **it is also a potential entry point through which the computer's security could be compromised.**

R Server provides various [API calls](api.md) that permit the execution of R scripts and R code. All authentication takes place on the operationalization web node, and the execution of the R code is managed through R Server's custom version of RServe add-on component. Rserve provides a TCP/IP interface to the R Interpreter running on the machine. By default, Rserve runs on the same machine as the operationalization compute node. RServe is started by Windows Service (RServeWinService) that runs under a virtual service account with low privileges. RServe inherits the permissions of that virtual service account. In the default configuration, Rserve will only accept socket connections from `localhost`. In other words, only thoses processes running on the same machine where RServe is running can directly connect to it and execute R code.

>[!Important]
>The operationalization compute node should, ideally, be the only local process that connects to RServe. To help ensure this is the case, a username and password is required to validate any connection between RServe and a client process. 
>
>However, there exist several vulnerabilities of which you should be aware. They are:
>
>-   RServe only accepts usernames and passwords in plain text from connecting clients.
>-   RServe uses a plain text configuration file to store the username and password.

If your operationalization configuration requires additional compute capacity, [additional compute nodes](configuration-initial.md#add-compute-nodes) can be added to provide sophisticated load-balancing capabilities. 

<a name="isolation"></a>

## Directory & User Isolation Considerations

In the R language, users can change files in the file system, download content from the web, download packages, and so on. 

In order to mitigate some of the risks associated with RServe, the service is setup to run using **a single account with restricted privileges**:

+ Read-only permissions to the R library to prevent users from installing packages from their R scripts

+ Write permissions to the R working directory `<MRS_home>\deployr\Rserve\workdir`, which is the directory under which R sessions and service calls will store artifacts, files, and workspaces
<br> 

>[!Important]
>While the custom Rserve service can only write to the working directory, **there is no user isolation between the session folders**. Any user familiar with the directory structure could in theory access another user’s session folder from their R script. 