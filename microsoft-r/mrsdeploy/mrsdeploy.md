---

# required metadata
title: "mrsdeploy Functions"
description: "mrsdeploy Functions"
keywords: "mrsdeploy package reference"
author: "j-martens"
manager: "jhubbard"
ms.date: "02/08/2017"
ms.topic: "reference"
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

**Applies to:  Microsoft R Server 9.x**

The `mrsdeploy` package provides functions for establishing a remote session in a console application and for publishing and managing a web service that is backed by the R code block or script you provided.  Each feature can be used independently but the greatest value is achieved when you can leverage both. 

This topic is a high-level description of package functionality. These functions can be called directly from the command line. For syntax and other details, follow these steps to [view function help pages](#findmore) or vignettes.

<a name="configure"></a>

## Supported configurations

For remote execution, participating nodes can be either of the following configurations:

+ Two machines running the same version of R Server (v9+), even if on different supported platforms, such as one Linux and one Windows.
+ One machine running R Client 3.3.2 and one machine running R Server v9+, where the R Client user issues a remote login sequence to the R Server instance. Execution is always on the R Server side. It's not possible to set up a remote session that runs on R Client.

The requirements for remote execution include:

+ An R Integrated Development Environment (IDE) [configured to work with Microsoft R Client](../r-client-get-started.md). 
+ [Authenticated access](../operationalize/security-authentication.md) to an instance of Microsoft R Server [configured to operationalize analytics](../operationalize/configuration-initial.md).

<a name="use-mrsdeploy"></a>

## How to use mrsdeploy

**The `mrsdeploy` package can only be used once Microsoft R Server has been configured to operationalize analytics**.  For more information, see [Configuring R Server to operationalize analytics](../operationalize/configuration-initial.md).

+ On R Client, the `mrsdeploy` package is installed **and loaded** automatically. You can start a remote session on an operationalized R Server instance once the remote login succeeds.

+ On R Server, the `mrsdeploy` package is installed, **but not loaded**. Therefore, you'll have to load it before using any  `mrsdeploy` functions. At the R prompt in the R Server session, type `library(mrsdeploy)` to load the package.

For a list of all `mrsdeploy` functions, including those for remote execution, see [mrsdeploy Functions](../mrsdeploy/mrsdeploy.md).


## Authentication functions and creating remote sessions

To use the functions in the `mrsdeploy` package, you must log into R Server as an authenticated user.  And if using the remote execution functionality, you can also create a remote R session upon login. 

Learn more about these functions and their arguments in the article "[Connecting to R Server to use mrsdeploy](../operationalize/mrsdeploy-connection.md)".

|Function | Description |
|---------|---------|
|`remoteLogin` |Authenticates the user via Active Directory and creates a remote R session and puts you at the remote command line unless you specify otherwise.|
|`remoteLoginAAD `|Authenticates the user via Azure Active Directory and creates a remote R session and puts you at the remote command line unless you specify otherwise.|
|`remoteLogout` |Logout of the remote session on the R Server.|


<a name="remote-functions"></a>

## Remote execution functions

The following functions are used to initialize and interact with a session on a [remote R Server](../operationalize/remote-execution.md).  Remote sessions are created when you authenticate and closed when you log out.

Learn more about executing remotely from your local machine in this "[Remote Execution](../operationalize/remote-execution.md)" article.

#### Execution functions

Use these functions to indicate whether the payload is a code block or script.

|Function | Description |
|---------|---------|
|`remoteExecute`|Base function for executing a block of R code or an R script in the remote R session. |
|`remoteScript `|A simple wrapper function for executing a remote R script.|
|`diffLocalRemote`|Generate a 'diff' report between local and remote.|

#### Remote command line functions

|Function | Description |
|---------|---------|
|`remoteCommandLine`|Displays the `REMOTE>` command prompt and provides a remote execution context. All R commands entered at the R console will be executed in the remote R session. |
|`pause` |When executed from the remote R session, returns the user to the `>` command prompt, and sets a local execution context. |
|`resume` |When executed from the local R session, returns the user to the `REMOTE>` command prompt, and sets a remote execution context. |

#### File management functions

|Function | Description |
|---------|---------|
|`listRemoteFiles` |Gets a list of all the files that are in the working directory of the remote session. |
|`putLocalFile` |Uploads a file from the local machine and writes it to the working directory of the remote R session. This function is often used if a data file needs to be accessed by a script running on the remote R session. |
|`getRemoteFile` |Downloads the file from the working directory of the remote R session into the working directory of the local R session. |
|`deleteRemoteFile` |Deletes the file from the working directory of the remote R session. |

#### Object functions

|Function | Description |
|---------|---------|
|`putLocalObject` |Puts an object from the workspace of the local R session and loads it into the workspace of the remote R session. |
|`getRemoteObject` |Gets an object from the workspace of the remote R session and loads it into the workspace of the local R session. |
|`putLocalWorkspace`|Takes all objects from the local R session and loads them into the remote R session. |
|`getRemoteWorkspace`|Takes all objects from the remote R session and loads them into the local R session. |

#### Snapshot functions

|Function | Description |
|---------|---------|
|`listSnapshots` |Get a list of all the snapshots on the R server that are available to the current user. |
|`createSnapshot` |Creates a snapshot of the remote session and saves it on the server. Both the workspace and the files in the working directory are saved. |
|`loadSnapshot `|Loads the specified snapshot from the server into the remote session. The workspace is updated with the objects saved in the snapshot, and saved files are restored to the working directory. |
|`deleteSnapshot` |Deletes the specified snapshot from the repository on the R Server. |
|`downloadSnapshot`|Downloads a snapshot from R Server.|


## Web service functions

The following functions are used to bundle R code or script as a web service. The [web service deployment](../operationalize/data-scientist-manage-services.md) can be published to the local server or remotely if you set up a remote session.

|Function | Description |
|---------|-------------|
|`publishService` |Publishes an R code block as a new web service running on R Server. |
|`updateService` |Updates an existing web service on an R Server instance. |
|`listServices` |Lists the different published web services on the R Server instance. |
|`getService` |Gets a web service for consumption. |
|`deleteService `|Deletes a web service on an R Server instance. |

<a name="findmore"></a>
## Get help on mrsdeploy functions from the R console

To see the **mrsdeploy** functions that can be called from the R console:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
2. Load `mrsdeploy` from the command line by typing `library(mrsdeploy)`.
1. In the console, open the package help by typing the following at the R prompt: `help(package="mrsdeploy")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.
 
> [!NOTE]
> To list all public functions, type library(help="mrsdeploy") at the R prompt.
>


## Next steps

After you are logged in to a remote server, you can publish a web service or issue interactive commands against the remote R Server. For more information, see these links:

+ [Remote Execution](../operationalize/remote-execution.md)

+ [Web Service](../operationalize/data-scientist-manage-services.md)

+ [Asynchronous batch execution of web services in R](../operationalize/data-scientist-batch-mode.md)

## See also

[Package Reference](~/package-reference.md)

[Install R Server](~/rserver.md)

[Install R Client](~/r-client.md)