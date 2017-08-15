---

# required metadata
title: " Security | DeployR 8.x | Microsoft Docs"
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "05/06/2016"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "deployr"
#ms.custom: ""

---

# RServe Execution Context

As per the standard usage of R, the current user starts the R executable and interacts with the application via the R Language and the R Interpreter. The R language provides OS-level access via the `system` function. With this function, a user can execute an OS command such as `system(“rmdir –r C:\\tmp”)`. While this is useful functionality for individual users, **it is also a potential entry point through which the computer's security could be compromised.**

DeployR provides various [API calls](deployr-api-reference.md) that permit the execution of R scripts and R code. The R scripts stored on the DeployR server can have different [levels of permissions](deployr-repository-manager-files.md#about-file-properties) dictating what a client can do. While public scripts can be executed by either anonymous or authenticated clients, private scripts can only be executed by the authenticated user that created the script. Raw R code execution, via the DeployR API, can only be executed by an authenticated user that has the [`POWER_USER` role](deployr-admin-console-permissions-with-roles.md).

All authentication takes place on the DeployR server, and the execution of the R code is managed through the DeployR RServe add-on component. Rserve provides a TCP/IP interface to the R Interpreter running on the machine. By default, Rserve runs on the same machine as the DeployR Server. RServe is started by Windows Service (RServeWinService) that runs under a virtual service account. RServe inherits the permissions of that virtual service account. In the default configuration, Rserve will only accept socket connections from `localhost`. In other words, only thoses processes running on the same machine where RServe is running can directly connect to it and execute R code.

>[!Important]
>The DeployR Server should, ideally, be the only local process that connects to RServe. To help ensure this is the case, a username and password is required to validate any connection between RServe and a client process. 
>
>However, there exist several vulnerabilities of which you should be aware. They are:
>
>-   RServe only accepts usernames and passwords in plain text from connecting clients.
>-   RServe uses a plain text configuration file to store the username and password.
>-   RServe has the permissions of the virtual service account, so it may have unwanted access to resources on the computer.

If a DeployR instance requires additional compute capacity, [a network of grid nodes](deployr-admin-managing-the-grid.md) can be added to provide sophisticated load-balancing capabilities. A grid node is simply a DeployR server with R installed and the RServe component installed and configured to accept connections from remote clients. Whenever remote grid nodes exist, a firewall should also be configured to accept only connections from the DeployR Server IP address.