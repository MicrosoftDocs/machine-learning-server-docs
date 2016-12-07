---

# required metadata
title: "mrsdeploy Functions"
description: "mrsdeploy Functions"
keywords: "mrsdeploy package reference"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/02/2016"
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

# mrsdeploy functions

The `mrsdeploy` package provides functions for establishing a remote session in a console application and for publishing and managing a Web service backed by an R code block or script that you provide. It's used to access the [operationalization capabilities](~/operationalize/about.md) in R Server.

Each feature can be used independently but the greatest value is achieved when you can leverage both. You need access to an R Server 9.0.1 instance on your corporate network or in an Azure solution (such as Azure HDInsight) to use `mrsdeploy`. In a remote execution scenario, both nodes must have a copy of the `mrsdeploy` package, installed as part of R Server 9.0.1 or R Client 3.3.2. For more information about requirements and supported configurations, see [Introduction to the mrsdeploy](mrsdeploy-intro-vignette.md).

This topic is a high-level description of package functionality. These functions can be called directly from the command line. For syntax and other details, follow these steps to [view function help pages](#findmore) or vignettes.

## Remote execution functions

The following functions are used to initialize and interact with a session on a [remote R Server](mrsdeploy-remoteexec-vignette.md).

### Login and Logout functions

Remote sessions are created when you log in and closed when you log out.

|Function | Description |
|---------|---------|
|`remoteLogin` |Authenticates the user via Active Directory and creates a remote R session.|
|`remoteLoginAAD `|Authenticates the user via Azure Active Directory and creates a remote R session. |
|`remoteLogout` |Logout of the remote session on the R Server.|

### Execution functions

Use these functions to indicate whether the payload is a code block or script.

|Function | Description |
|---------|---------|
|`remoteExecute`|Base function for executing a block of R code or an R script in the remote R session. |
|`remoteScript `|A simple wrapper function for executing a remote R script.|

### File management functions

|Function | Description |
|---------|---------|
|`listRemoteFiles` |Gets a list of all the files that are in the working directory of the remote session. |
|`putLocalFile` |Uploads a file from the local machine and writes it to the working directory of the remote R session. This function is often used if a data file needs to be accessed by a script running on the remote R session. |
|`getRemoteFile` |Downloads the file from the working directory of the remote R session into the working directory of the local R session. |
|`deleteRemoteFile` |Deletes the file from the working directory of the remote R session. |
|`putLocalObject` |Puts an object from the workspace of the local R session and loads it into the workspace of the remote R session. |

### Object functions

|Function | Description |
|---------|---------|
|`getRemoteObject` |Gets an object from the workspace of the remote R session and loads it into the workspace of the local R session. |
|`putLocalWorkspace`|Takes all objects from the local R session and loads them into the remote R session. |
|`getRemoteWorkspace`|Takes all objects from the remote R session and loads them into the local R session. |

### Snapshot functions

|Function | Description |
|---------|---------|
|`listSnapshots` |Get a list of all the snapshots on the R server that are available to the current user. |
|`createSnapshot` |Creates a snapshot of the remote session and saves it on the server. Both the workspace and the files in the working directory are saved. |
|`loadSnapshot `|Loads the specified snapshot from the server into the remote session. The workspace is updated with the objects saved in the snapshot, and saved files are restored to the working directory. |
|`deleteSnapshot` |Deletes the specified snapshot from the repository on the R Server. |

### Remote command line functions

|Function | Description |
|---------|---------|
|`remoteCommandLine`|Displays the `REMOTE>` command prompt and provides a remote execution context. All R commands entered at the R console will be executed in the remote R session. |
|`resume` |When executed from the local R session, returns the user to the `REMOTE>` command prompt, and sets a remote execution context. |

## Web service functions

The following functions are used to bundle R code or script as a web service. The [web service deployment](mrsdeploy-websrv-vignette.md) can be published to the local server or remotely if you set up a remote session.

|Function | Description |
|---------|-------------|
|`publishService` |Publishes an R code block as a new web service running on R Server. |
|`updateService` |Updates an existing web service on an R Server instance. |
|`listServices` |Lists the different published web services on the R Server instance. |
|`getService` |Gets a web service for consumption. |
|`deleteService `|Deletes a web service on an R Server instance. |

<a name="findmore"></a>
##How to view function help in the package

R Packages often include embedded help pages, documenting the syntax and parameters of each function. To view the list of functions and associated help pages for `mrsdeploy`, follow these steps.

1. Launch an R console with `Rgui.exe` or start another preferred R IDE such as R Tools for Visual Studio (RTVS) or RStudio.
2. If `mrsdeploy` is not loaded, you can load it from the command line by typing `library(mrsdeploy)`.
3. Type `help(package="mrsdeploy")`.

Vignettes are available for [remote execution](mrsdeploy-remoteexec-vignette.md) and [web service deployment](mrsdeploy-websrv-vignette.md).

## See also

[Package Help](~/package-reference.md)

[Introduction to mrsdeploy](mrsdeploy-intro-vignette.md)

[Install R Server](~/rserver.md)

[Install R Client](~/r-client.md)
