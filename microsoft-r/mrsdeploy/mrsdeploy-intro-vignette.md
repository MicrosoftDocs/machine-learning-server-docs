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

# Introduction to mrsdeploy

The **mrsdeploy** package provides functions that give you command line execution against a remote Microsoft R Server, plus the ability to deploy R script or code as a standalone web service, on a local or remote R Server instance.

Each feature can be used independently but the greatest value is achieved when you can leverage both. You need access to an R Server 9.0 instance on your corporate network or in an Azure solution (such as Azure HDInsight) to use remote execution or web service deployment. Both nodes must have a copy of the `mrsdeploy` package, installed as part of R Client or R Server 9.0.1.

## How to get and use mrsdeploy

The mrsdeploy package is available in installations of [Microsoft R Client 9.0](https://msdn.microsoft.com/microsoft-r/r-client) and [Microsoft R Server 9.0](https://msdn.microsoft.com/microsoft-r/rserver), on all [supported platforms](https://msdn.microsoft.com/microsoft-r/rserver-install-supported-platforms).

The `mrsdeploy` package is not loaded automatically. In an R console application, run `install.packages("mrsdeploy")` and `library(mrsdeploy)` before calling its functions. We recommend [R Tools for Visual Studio (RTVS)](https://www.visualstudio.com/vs/rtvs/), RStudio, or another third-party tool.

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

## How to login using an on-premises Active Directory service

You must have a Microsoft R Server version 9.0 on your network or in an Azure solution or service. Every user must have a valid user account in Active Directory. Given a server and valid account, you can use the `remote_login` or `remote_login_aad` function to authenticate against an R server.

**Arguments for remote_login**

- `deployr_endpoint` - (Required) The HTTP/HTTPS endpoint for the target R Server 9.0 instance deployed on your network or in the cloud, including the port number.
- `session` - (Optional) If `TRUE`, creates a remote session.
- `diff` - (Optional) If `TRUE`, creates a 'diff' report showing differences between the local and remote sessions. Parameter is only valid if session parameter is `TRUE`.
- `commandline` - (Optional) If `TRUE`, adds a "REMOTE" label to the command prompt in an R console application. The "REMOTE" command line is used to interact with the remote R session. This parameter is only valid if the session parameter is `TRUE`.
- `username` - (Optional) If `NULL`, the user will be prompted to enter username  (assuming the tool accepts prompted credentials).
- `password` - (Optional) If `NULL`, the user will be prompted to enter a password (assuming the tool accepts prompted credentials.

**Response**

If authentication is successful, a valid OAuth token is returned. If the `session` parameter is `TRUE`, a remote session on the server will be created.

**Example for on-premises Active Directory service**


```R
remote_login("https://localhost:1280", session=TRUE, diff=TRUE, commandline=TRUE)

```
Once the `REMOTE>` command line is displayed in the R console, any R commands entered will be executed on the remote R session.

To switch back to the local R session, type `pause()`. You can go back to the remote R session by typing `resume()`.

To terminate the remote R session, type `exit` at the `REMOTE>` prompt. Also, from local R session, you can terminate the remote R session by typing `remote_logout()`

## How to login using Azure Active Directory

You must have a Microsoft R Server version 9.0 on your network or in an Azure solution or service. Every user must have a valid user account in Azure Active Directory. Given a server and valid account, you can use the `remote_login_aad` function to authenticate against an R server.

**Arguments for remote_login_aad**

- `deployr_endpoint` - (Required) The HTTP/HTTPS endpoint for the target R Server 9.0 instance deployed on your network or in the cloud, including the port number.
- `authuri` - (Required) The URI of the authentication service for Azure Active Directory.
- `tenantid` - (Required) The tenant ID of the Azure Active Directory account being used to authenticate.
- `clientid` - (Required) The client ID of the Application for the Azure Active Directory account.
- `resource` - (Required) The resource ID of the Application for the Azure Active Directory account.
- `session` - (Optional) If `TRUE`, creates a remote session.
- `diff` - (Optional) If `TRUE`, creates a 'diff' report showing differences between the local and remote sessions. Parameter is only valid if session parameter is `TRUE`.
- `commandline` - (Optional) If `TRUE`, adds a "REMOTE" label to the command prompt in an R console application. The "REMOTE" command line is used to interact with the remote R session. This parameter is only valid if the session parameter is `TRUE`.
- `username` - (Optional) If `NULL`, the user will be prompted to enter username  (assuming the tool accepts prompted credentials).
- `password` - (Optional) If `NULL`, the user will be prompted to enter a password (assuming the tool accepts prompted credentials.


**Response**

If authentication is sucessful, a valid OAuth token is returned.  If the `session` parameter is `TRUE`, a remote session on the DeployR server will be created.

**Example for Azure Active Directory**

```R
remote_login_aad("http://localhost:12800",
                   authuri = "https://login.windows.net",
                   tenantid = "deployrtest.onmicrosoft.com",
                   clientid = "10f6b06f-9a75-4222-8888-ef793969c7d4",
                   resource = "a053a63f-8af5-480b-9510-48bb32e44be8",
                   session=TRUE,
                   diff=TRUE,
                   commandline=TRUE
```
## Next steps

After you are logged in to a remote server, you can publish a web service or issue interactive commands against the remote R Server. For more information, see the other vignettes in this package.

## See also

[mrsdeploy Function Reference](mrsdeploy.md)

[Package Reference](../package-reference.md)
