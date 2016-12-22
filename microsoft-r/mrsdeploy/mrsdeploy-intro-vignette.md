---

# required metadata
title: "Introduction to the mrsdeploy package in Microsoft R"
description: "An introduction to functions in the mrsdeploy package in Microsoft R"
keywords: "mrsdeploy package"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/19/2016"
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
ms.technology: "r-server"
ms.custom: ""

---

# Introduction to the mrsdeploy package in Microsoft R

The `mrsdeploy` package provides functions that give you command line execution against a remote Microsoft R Server 9.0.1 instance, plus the ability to deploy R script or code as a standalone web service, on a local or remote R Server instance.

The package is installed as part of R Server 9.0 or R Client 3.3.2 on all supported platforms, but you must have R Server to use it.

## Supported configurations

For remote execution, participating nodes can be either of the following configurations:

+ Two machines running R Server 9.0.1
+ One machine running R Server 9.0.1 and one machine running R Client 3.3.2, where the R Client user issues a remote login sequence to the R Server instance. Execution is always on the R Server side. It's not possible to set up a remote session that runs on R Client.

R Server can be any platform, as long as both are version 9.0.1. For example, you can establish a remote connection between Linux and Windows as along as both are running R Server 9.0.1, or the Windows machine has R Client 3.3.2.

## How to get and use mrsdeploy

The `mrsdeploy` package is available in installations of [Microsoft R Client](https://msdn.microsoft.com/microsoft-r/r-client) and [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver), on all [supported platforms](https://msdn.microsoft.com/microsoft-r/rserver-install-supported-platforms).

+ On R Client, the `mrsdeploy` package is loaded automatically and can be used to initiate a remote session on an R Server instance.

+ On R Server, the `mrsdeploy` package is enabled and configured through R Server operationalization. For more information, see [Configuring R Server for Operationalization](~/operationalize/configuration-initial.md).

To use it for the duration of a session in an R console application, run `install.packages("mrsdeploy")` and `library(mrsdeploy)` before calling its functions. We recommend [R Tools for Visual Studio (RTVS)](https://www.visualstudio.com/vs/rtvs/) on a Windows computer, or RStudio or another R IDE on a non-Windows workstation.

1. Open an R project in RTVS.
2. In the **R Interactive** window at the command prompt, type `install.packages("mrsdeploy")`.
3. Also in **R Interactive**, type `library(mrsdeploy)` to load the package.

## Authenticated access to mrsdeploy operations

Deployment and administration tasks require an authenticated connection. Authentication of a user identity is handled via Active Directory. Neither R Server, nor the former DeployR server component that is now embedded in R Server, will store or manage user names and passwords.

For authentication, you can choose from the following approaches, which are valid on all supported platforms:

- **remoteLogin()** using an on premises Active Directory server on your network.
- **remoteLoginAAD()** using Azure Active Directory in the cloud.

For on premises Active Directory, users will need to authenticate via the `/user/login` API, passing a username and password.

Both Active Directory and Azure Active Directory return an access token. This access token is then passed in the request header of every subsequent mrsdeploy request. If the user does not provide a valid login, an HTTP 401 status code is returned.

In general, all mrsdeploy operations are available to authenticated users. There is currently no role-based authorization model that specifically allows or denies specific operations. Destructive tasks, such as deleting a web service from a remote execution command line, are available only to the user who initially created the service.

## Next steps

After you are logged in to a remote server, you can publish a web service or issue interactive commands against the remote R Server. For more information, see these links:

+ [Remote Execution](mrsdeploy-remoteexec-vignette.md)
+ [Web Service](mrsdeploy-websrv-vignette.md)

## See also

[mrsdeploy Function Reference](mrsdeploy.md)

[Package Reference](../package-reference.md)
