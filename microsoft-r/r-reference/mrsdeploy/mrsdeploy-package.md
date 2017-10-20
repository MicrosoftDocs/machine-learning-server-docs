---

# required metadata
title: "mrsdeploy package for R - Machine Learning Server "
description: "Function help reference for the mrsdeploy R package of Machine Learning Server."
keywords: "mrsdeploy package reference"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "09/25/2017"
ms.topic: "reference"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# mrsdeploy package for R
 
The **mrsdeploy** library provides functions for establishing a remote session in a console application and for publishing and managing a web service that is backed by the R code block or script you provided. Each capability can be used independently but the greatest value is achieved when you can leverage both. 

| Package details | |
|--------|-|
| Version: |  1.1.2 |
| Runs on: | [Machine Learning Server 9.2.1](../../what-is-machine-learning-server.md) <br/>[Microsoft R Server 9.1 and earlier](../../what-is-microsoft-r-server.md)   <br/>[SQL Server 2016 and later (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) |
| Built on: | R 3.4.1 (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|


<a name="use-mrsdeploy"></a>

## How to use mrsdeploy

First, configure the server before using this library. For more information, see [Configuring Machine Learning Server to operationalize analytics](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization).

After configuration, you can call **mrsdpeloy** functions from the command line or from script. 

+ On R Client, the mrsdeploy package is installed **and loaded** automatically. You can start a remote session on an operationalized R Server instance once the remote login succeeds.

+ On Machine Learning Server or R Server, the mrsdeploy package is installed, **but not loaded**. Therefore, you'll have to load it before using any  **mrsdeploy** functions. At the R prompt in the Machine Learning Server / R Server session, type `library(mrsdeploy)` to load the package.


<a name="configure"></a>

### Supported configurations

For remote execution, participating nodes can be either of the following configurations:

+ Two machines running the same version of Machine Learning Server or R Server (v9+), even if on different supported platforms, such as one Linux and one Windows.
+ One machine running R Client 3.x and one machine running Machine Learning Server v9+, where the R Client user issues a remote login sequence to the Machine Learning  Server instance. Execution is always on the Machine Learning Server side. It's not possible to set up a remote session that runs on R Client.

The requirements for remote execution include:

+ An R Integrated Development Environment (IDE) [configured to work with Microsoft R Client](../../r-client-get-started.md). 
+ [Authenticated access](../../operationalize/configure-authentication.md) to an instance of Machine Learning Server [configured to operationalize analytics](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization).


## Functions by category

This section lists the functions by category to give you an idea of how each one is used. You can also use the table of contents to find functions in alphabetical order.

## 1-Authentication & remote session functions

To use the functions in the mrsdeploy package, you must log into Machine Learning Server as an authenticated user.  And if using the remote execution functionality, you can also create a remote R session upon login. 

Learn more about these functions and their arguments in the article [Connecting to Machine Learning Server to use mrsdeploy](../../operationalize/how-to-connect-log-in-with-mrsdeploy.md).

|Function | Description |
|---------|---------|
|[remoteLogin](remotelogin.md) |Authenticates the user via Active Directory and creates a remote R session and puts you at the remote command line unless you specify otherwise.|
|[remoteLoginAAD](remoteloginaad.md)  |Authenticates the user via Azure Active Directory and creates a remote R session and puts you at the remote command line unless you specify otherwise.|
|[remoteLogout](remotelogout.md)  |Logout of the remote session on the Machine Learning Server.|

<a name="remote-functions"></a>

## 2-Remote execution functions

The following functions are used to initialize and interact with a session on a [remote Machine Learning Server](../../r/how-to-execute-code-remotely.md). Remote sessions are created when you authenticate and closed when you log out.

Learn more about executing remotely from your local machine in this [Remote Execution](../../r/how-to-execute-code-remotely.md) article.

### 3-Execution functions

Use these functions to indicate whether the payload is a code block or script.

|Function | Description |
|---------|---------|
|[remoteExecute](remoteexecute.md)|Base function for executing a block of R code or an R script in the remote R session. |
|[remoteScript](remotescript.md)|A simple wrapper function for executing a remote R script.|
|[diffLocalRemote](difflocalremote.md)|Generate a 'diff' report between local and remote.|

### 4-Remote command line functions

|Function | Description |
|---------|---------|
|[remoteCommandLine](remoteCommandLine.md)|Displays the `REMOTE>` command prompt and provides a remote execution context. All R commands entered at the R console will be executed in the remote R session. |
|[pause](remoteCommandLine.md) |When executed from the remote R session, returns the user to the `>` command prompt, and sets a local execution context. |
|[resume](remoteCommandLine.md)|When executed from the local R session, returns the user to the `REMOTE>` command prompt, and sets a remote execution context. |

### 5-File management functions

|Function | Description |
|---------|---------|
|[listRemoteFiles](listRemoteFiles.md)|Gets a list of all the files that are in the working directory of the remote session. |
|[putLocalFile](putLocalFile.md) |Uploads a file from the local machine and writes it to the working directory of the remote R session. This function is often used if a data file needs to be accessed by a script running on the remote R session. |
|[getRemoteFile](getRemoteFile.md) |Downloads the file from the working directory of the remote R session into the working directory of the local R session. |
|[deleteRemoteFile](deleteRemoteFile.md) |Deletes the file from the working directory of the remote R session. |

### 6-Object functions

|Function | Description |
|---------|---------|
|[putLocalObject](putLocalObject.md) |Puts an object from the workspace of the local R session and loads it into the workspace of the remote R session. |
|[getRemoteObject](getRemoteObject.md)  |Gets an object from the workspace of the remote R session and loads it into the workspace of the local R session. |
|[putLocalWorkspace](putLocalWorkspace.md) |Takes all objects from the local R session and loads them into the remote R session. |
|[getRemoteWorkspace](getRemoteWorkspace.md) |Takes all objects from the remote R session and loads them into the local R session. |

### 7-Snapshot functions

|Function | Description |
|---------|---------|
|[listSnapshots](listSnapshots.md) |Get a list of all the snapshots on the Machine Learning Server that are available to the current user. |
|[createSnapshot](createSnapshot.md) |Creates a snapshot of the remote session and saves it on the server. Both the workspace and the files in the working directory are saved. |
|[loadSnapshot](loadSnapshot.md) |Loads the specified snapshot from the server into the remote session. The workspace is updated with the objects saved in the snapshot, and saved files are restored to the working directory. |
|[deleteSnapshot](deleteSnapshot.md)  |Deletes the specified snapshot from the repository on the Machine Learning Server. |
|[downloadSnapshot](downloadSnapshot.md) |Downloads a snapshot from Machine Learning Server.|


## 8-Web service functions

The following functions are used to bundle R code or script as a web service. The [web service deployment](../../operationalize/how-to-deploy-web-service-publish-manage-in-r.md) can be published to the local server or remotely if you set up a remote session.

|Function | Description |
|---------|-------------|
|[publishService](publishService.md) | Publishes an R code block as a new web service running on Machine Learning Server. |
|[updateService](updateService.md) | Updates an existing web service on a Machine Learning Server instance. |
|[listServices](listServices.md) | Lists the different published web services on the Machine Learning Server instance. |
|[getService](getService.md) | Gets a web service for consumption. |
|[deleteService](deleteService.md) |Deletes a web service on a Machine Learning Server instance. |

## Next steps

Add R packages to your computer by running setup for Machine Learning Server or R Client: 

+ [R Client](../../r-client/what-is-microsoft-r-client.md) 
+ [Machine Learning Server](../../what-is-machine-learning-server.md)

After you are logged in to a remote server, you can publish a web service or issue interactive commands against the remote Machine Learning Server. For more information, see these links:

+ [Remote Execution](../../r/how-to-execute-code-remotely.md)   
+ [How to publish and manage web services in R](../../operationalize/how-to-deploy-web-service-publish-manage-in-r.md)  
+ [How to interact with and consume web services in R](../../operationalize/how-to-consume-web-service-interact-in-r.md)    
+ [Asynchronous batch execution of web services in R](../../operationalize/how-to-consume-web-service-asynchronously-batch.md)  

## See also

 [R Package Reference](../introducing-r-server-r-package-reference.md) 