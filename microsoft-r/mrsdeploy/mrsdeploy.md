---

# required metadata
title: "mrsdeploy Functions"
description: "mrsdeploy Functions"
keywords: "mrsdeploy package reference"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/30/2016"
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

# mrsdeploy Functions

The `mrsdeploy` package provides functions for establishing a remote session in a console application and for publishing and managing a Web service backed by an R code block or script that you provide. The package is installed as part of R Server 9.0 and R Client 9.0.1 on all supported platforms.

Command line interaction between local and remote sessions requires `mrsdeploy` on both ends. The package version must be 9.0.1 or later.

This topic is a high-level description of package functionality. These functions can be called directly from the command line. For syntax and other details, follow these steps to [view function help pages](#findmore) or vignettes.

## Remote Execution Functions

|Category |Function | Description |
|---------|---------|-------------|
|Login/Logout|remoteLogin |Authenticates the user via Active Directory and creates a remote R session.|
|Login/Logout|remoteLoginAAD |Authenticates the user via Azure Active Directory and creates a remote R session. |
|Login/Logout|remoteLogout |Logout of the remote session on the R Server.|
|Execute|remoteExecute|Base function for executing a block of R code or an R script in the remote R session. |
|Execute|remoteScript |A simple wrapper function for executing a remote R script.|
|Files|listRemoteFiles |Get a list of all the files that are in the working directory of the remote session. |
|Files|putLocalFile |Uploads a file from the local machine and writes it to the working directory of the remote R sesion. This function is often used if a data file needs to be accessed by a script running on the remote R session. |
|Files|getRemoteFile |Downloads the file from the working directory of the remote R session into the working directory of the local R session. |
|Files|deleteRemoteFile |Deletes the file from the working directory of the remote R session. |
|Objects|putLocalObject |Put an object from the workspace of the local R session and load it into the workspace of the remote R session. |
|Objects|getRemoteObject |Get an object from the workspace of the remote R session and load it into the workspace of the local R session. |
|Objects|putLocalWorkspace|Take all objects from the local R session and load them into the remote R session. |
|Objects|getRemoteWorkspace|Take all objects from the remote R session and load them into the local R session. |
|Snapshots|listSnapshots |Get a list of all the snapshots on the R server that are available to the current user. |
|Snapshots|createSnapshot |Creates a snapshot of the remote session and saves it on the server. Both the workpace and the files in the working directory are saved. |
|Snapshots|loadSnapshot |Loads the specified snapshot from the server into the remote session. The workpace is updated with the objects saved in the snapshot, and saved files are restored to the working directory. |
|Snapshots|deleteSnapshot |Deletes the specified snapshot from the repository on the R Server. |
|Remote command line|remoteCommandLine|Displays the `REMOTE>` command prompt and provides a remote execution context. All R commands entered at the R console will be executed in the remote R session. |
|Remote command line|resume |When executed from the local R session, returns the user to the `REMOTE>` command prompt, and sets a remote execution context. |

## Web Service Functions

|Function | Description |
|---------|-------------|
|publishService |Publishes an R code block as a new web service running on R Server. |
|updateService |Updates an existing web service on an R Server instance. |
|listServices |List the different published web services on the R Server instance. |
|getService |Get a web service for consumption. |
|deleteService |Delete a web service on an R Server instance. |

<a name="findmore"></a>
##How to view function help in the package

R Packages often include embedded help pages, documenting the syntax and parameters of each function. To view the list of functions and associated help pages for `mrsdeploy`, follow these steps.

1. Launch an R console with `Rgui.exe` or start another preferred R IDE such as R Tools for Visual Studio (RTVS) or RStudio.
2. At the command line, type `library(mrsdeploy)`.
3. Type `help(mrsdeploy)`.

Vignettes are available for [remote execution]() and [web service deployment]().

## See also

[Package Help](../package-reference.md)

[Introduction to mrsdeploy](mrsdeploy-intro-vignette.md)

[Install R Server](rserver.md)

[Install R Client](r-client.md)
