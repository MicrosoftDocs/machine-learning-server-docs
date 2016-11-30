---

# required metadata
title: "Introduction to the mrsdeploy package in Microsoft R (vignette)"
description: "An introduction to functions in the mrsdeploy package in Microsoft R"
keywords: "mrsdeploy package vignette"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/26/2016"
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

# Introduction to the mrsdeploy package in Microsoft R (vignette)

The `mrsdeploy` package provides functions that give you command line execution against a remote Microsoft R Server, plus the ability to deploy R script or code as a standalone web service, on a local or remote R Server instance.

Each feature can be used independently but the greatest value is achieved when you can leverage both. You need access to an R Server 9.0 instance on your corporate network or in an Azure solution (such as Azure HDInsight) to use remote execution or web service deployment. Both nodes must have a copy of the `mrsdeploy` package, installed as part of R Server 9.0.1 or R Client 3.3.2.

## How to get and use mrsdeploy

The `mrsdeploy` package is available in installations of [Microsoft R Client](https://msdn.microsoft.com/microsoft-r/r-client) and [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver), on all [supported platforms](https://msdn.microsoft.com/microsoft-r/rserver-install-supported-platforms).

The `mrsdeploy` package is not loaded automatically.

To use it for the duration of a session in an R console application, run `install.packages("mrsdeploy")` and `library(mrsdeploy)` before calling its functions. We recommend [R Tools for Visual Studio (RTVS)](https://www.visualstudio.com/vs/rtvs/) on a Windows computer, or RStudio or another R IDE on a non-Windows workstation.

1. Open an R project in RTVS.
2. In the **R Interactive** window at the command prompt, type `install.packages("mrsdeploy")`.
3. Also in **R Interactive**, type `library(mrsdeploy)` to load the package.

## Authenticated access to mrsdeploy operations

Deployment and administration tasks require an authenticated connection. Authentication of a user identity is handled via Active Directory. Neither R Server, nor the former DeployR server component that is now embedded in R Server, will store or manage user names and passwords.

For authentication, you can choose from the following approaches, which are valid on all supported platforms:

- **remote_login()** using an on premises Active Directory server on your network.
- **remote_login_aad()** using Azure Active Directory in the cloud.

For on premises Active Directory, users will need to authenticate via the `/user/login` API, passing a username and password.

Both Active Directory and Azure Active Directory return an access token. This access token is then passed in the request header of every subsequent mrsdeploy request. If the user does not provide a valid login, an HTTP 401 status code is returned.

In general, all mrsdeploy operations are available to authenticated users. There is currently no role-based authorization model that specifically allows or denies specific operations. Destructive tasks, such as deleting a web service from a remote execution command line, are available only to the user who initially created the service.

## Next steps

After you are logged in to a remote server, you can publish a web service or issue interactive commands against the remote R Server. For more information, see the other vignettes in this package:

+ [Remote Execution vignette](mrsdeploy-remoteexec-vignette.md)
+ [Web Service vignette](mrsdeploy-websrv-vignette.md)

## See also

[mrsdeploy Function Reference](mrsdeploy.md)

[Package Reference](../package-reference.md)
