---

# required metadata
title: "Remote Execution | Microsoft R Server Docs"
description: "Remote execution for Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
ms.topic: "get-started-article"
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

# Remote execution in Microsoft R Server

**Applies to:  Microsoft R Server 9.0.1**

Remote execution is the ability to issue commands from either R Server or R Client to a remote R Server instance. Remote execution is supported via the command line in console applications, R scripts that call functions from the `mrsdeploy` package, or code that calls the operationalization APIs. All nodes must be version 9.0.1.

## Using the remote command line

The remote command line allows you to directly interact with an R Server 9.0.1 instance on another machine. You can enter 'R' code just as you would in a local R console. R code entered at the remote command line executes on the remote server. Switching between the local command line and the remote command line is done using these functions: `pause()` and `resume()`.

To establish a remote session, you must issue a remote login request, which in turn authenticates your user identity on the remote server.

## Supported configurations

For remote execution, participating nodes can be either of the following configurations:

+ Two machines running R Server 9.0.1
+ One machine running R Server 9.0.1 and one machine running R Client 3.3.2, where the R Client user issues a remote login sequence to the R Server instance. Execution is always on the R Server side. It's not possible to set up a remote session that runs on R Client.

R Server can be any platform, as long as both are version 9.0.1. For example, you can establish a remote connection between Linux and Windows as along as both are running R Server 9.0.1, or the Windows machine has R Client 3.3.2.

## How to get and use mrsdeploy

Functions used in remote execution are provided in the `mrsdeploy` package, available in installations of [Microsoft R Client](https://msdn.microsoft.com/microsoft-r/r-client) and [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver), on all [supported platforms](https://msdn.microsoft.com/microsoft-r/rserver-install-supported-platforms).

+ On R Client, the `mrsdeploy` package is loaded automatically. You can initiate a remote session on an operationalized R Server instance once you complete a remote login.

+ On R Server, the `mrsdeploy` package is enabled and configured through R Server operationalization. This configuration step is required. For more information, see [Configuring R Server for Operationalization](~/operationalize/configuration-initial.md).

To use remote execution for the duration of a session in an R console application, run `install.packages("mrsdeploy")` and `library(mrsdeploy)` before calling its functions. We recommend [R Tools for Visual Studio (RTVS)](https://www.visualstudio.com/vs/rtvs/) on a Windows computer, or RStudio or another R IDE on a non-Windows workstation.

1. Open an R project in RTVS.
2. In the **R Interactive** window at the command prompt, type `install.packages("mrsdeploy")`.
3. Also in **R Interactive**, type `library(mrsdeploy)` to load the package.

## About authenticated access to mrsdeploy operations

Deployment and administration tasks require an authenticated connection. Authentication of a user identity is handled via Active Directory. Neither R Server, nor the former DeployR server component that is now embedded in R Server, will store or manage user names and passwords.

For authentication, you can choose from the following approaches, which are valid on all supported platforms:

- **remote_login()** using an on premises Active Directory server on your network.
- **remote_login_aad()** using Azure Active Directory in the cloud.

For on premises Active Directory, users will need to authenticate via the `/user/login` API, passing a username and password.

Both Active Directory and Azure Active Directory return an access token. This access token is then passed in the request header of every subsequent mrsdeploy request. If the user does not provide a valid login, an HTTP 401 status code is returned.

In general, all mrsdeploy operations are available to authenticated users. There is currently no role-based authorization model that specifically allows or denies specific operations. Destructive tasks, such as deleting a web service from a remote execution command line, are available only to the user who initially created the service.

The remote command line allows you to directly interact with an R Server 9.0.1 instance on another machine. You can enter 'R' code just as you would in a local R console. R code entered at the remote command line executes on the remote server. Switching between the local command line and the remote command line is done using these functions: `pause()` and `resume()`. To establish a remote session, you must issue a remote login request, which in turn authenticates your user identity on the remote server.

## How to create a remote session

The `remote_login` and `remote_login_aad` functions are used to authenticate against R Server, creating a remote session.

**Example**

```R
#authenticate against Microsoft R Server
remote_login("https://localhost:12800", session=TRUE, diff=TRUE, commandline=TRUE)

#authenticate against the R Server using Azure Active Directory
remoteLoginAAD("http://localhost:12800",
                   authuri = "https://login.windows.net",
                   tenantid = "myMRSServer.contoso.com",
                   clientid = "00000000-0000-0000-0000-000000000000",
                   resource = "00000000-0000-0000-0000-000000000000",
                   session=TRUE,
                   diff=TRUE,
                   commandline=TRUE)


```
Once the `REMOTE>` command line is displayed in the R console, any R commands entered will be executed on the
remote R session. To switch back to the local R session, type `pause()`. To terminate the remote R session,
type `exit`, at the `REMOTE>` prompt. If you have switched to the local R session, you can go back to the
remote R session by typing `resume()`. Also, from local R session, you can terminate the remote R session by
typing `remote_logout()`

**Example**

```R
#execute some R commands on the remote session
REMOTE>x<-rnorm(1000)
REMOTE>hist(x)

REMOTE>pause()  #switches the user to the local R session
>resume()  

REMOTE>exit  #logout and terminate the remote R session
>remoteLogout()  
```

## Create a diff report
A `diff` report is available so you can see and manage differences between the local and remote R environments.
The diff report contains information regarding R versions, R packages installed locally, but not on the remote
session, and differences between R package versions. This report is shown by default when you log in, but can be
run anytime by executing the function: `diffLocalRemote()`.

## Execute an R script remotely

If you have R scripts on your local machine, you can execute them remotely by using the function `remoteScript()`.
This function takes a path to an R script to be executed remotely. You also have options
to save or display any plots that might have been generated during script execution. The function returns a list
containing the status of the execution (success/failure), the console output generated, and a list of files created.

If your R Script has R Package dependencies, those packages must be installed on the Microsoft R server. You can either have your Administrator install them globally by logging in directly to the server,
or you can install them for the duration of the remote session by using the R function `install.packages()`. Leave the `lib` parameter empty.

If you need more granular control of a remote execution scenario, you can use the `remoteExecute()` function.

**Example**

```R
#install a package for the life of the session
REMOTE>install.packages("bitops")

#switch to the local R session
REMOTE>pause()
#execute an R script remotely
>remoteScript("c/myScript.R")    
```
## Work with R objects and files remotely

Once you have executed an R code remotely, you may want to retrieve certain R objects and load them into your local R session. For example, if you have an R script that creates a linear model (i.e. `m<-lm(x~y)` ), and you want to work with that model in your local R session, you can retrieve the object `m` by using the function `getRemoteObject()`.

Conversely, if you have a local R object that you want to make available to your remote R session, you can use the function `putLocalObject()`. If you want to sync your local and remote workspaces, the functions `putLocalWorkspace()` and `getRemoteWorkspace()` can be used.

Similar capabilities are available for files that need to be moved between the local and remote R sessions.

The following functions are available for working with files:  `putLocalFile()`, `getRemoteFile()`, `listRemoteFiles()` and `deleteRemoteFile()`.

**Example**

```R
#execute a script remotely that generated 2 R objects we are interested in retrieving
>remoteExecute("C:/myScript.R')
#retreive the R objects from the remote R session and load them into our local R session
>getRemoteObject(c("model","out"))

#an R script depends upon an R object named `data` to be available. Move the local
#instance of `data` to the remote R session
>putLocalObject("data")
#execute an R script remotely
>remoteScript("c/myScript2.R")

#push a data file to the remote R session
>putLocalFile("C:/data/survey.csv")
#execute an R script remotely
>remoteScript("c/myScript2.R")
```
## A word on plot

When you plot remotely, the default plot size is 400 x 400 pixels. If you desire higher-resolution output, you must tell the remote session the size of plot to create. On a local session, you might do the following:

```R
> png(filename="myplot.png", width=1440, height=900)
> ggplot(aes(x=value, group=am, colour=factor(am)), data=mtcarsmelt) + geom_density() + facet_wrap(~variable, scales="free")
> dev.off()
```

When working on the REMOTE command line, you need to combine these 3 statements together:

```R
REMOTE> png(filename="myplot.png", width=1440, height=900);ggplot(aes(x=value, group=am, colour=factor(am)), data=mtcarsmelt) + geom_density() + facet_wrap(~variable, scales="free");dev.off()
```

As an alternative you can use the remoteScript function. Do the following:

```R
#Open a new script window in your IDE
#Enter the commands on separate lines

png(filename="myplot.png", width=1440, height=900)
ggplot(aes(x=value, group=am, colour=factor(am)), data=mtcarsmelt) + geom_density() + facet_wrap(~variable, scales="free")
dev.off()
```

```R
#Save the script to a file (i.e. myscript.R )
#Switch from the remote session to the local session by typing "pause()"" on the REMOTE command line
REMOTE> pause()
>
```

```R
#From the local command prompt, execute your remote script
> remote_script("myscript.R")
```

## Snapshots and why they are useful

If you need a prepared environment for remote script execution that includes any of the following: R packages, R objects and data files, consider creating a **snapshot**. A snapshot is an image of a remote R session saved to Microsoft R Server, which includes:

+ The session's workspace along with the installed R packages
+ Any files and artifacts in the working directory

A snapshot can be loaded into any subsequent remote R session for the user who created it. For example, suppose you want to execute a script that needs three R packages, a reference data file, and a model object.  Instead of loading these items each time you want to execute the script, create a snapshot of an R session containing them. Then, you can save time later by retrieving this snapshot using its ID to get the session contents exactly as they were at the time the snapshot was created.

Snapshots are only accessible to the user who creates them and cannot be shared across users.

The following functions are available for working with snapshots:  
`listSnapshots()`, `createSnapshot()`, `loadSnapshot()`, `downloadSnapshot()` and `deleteSnapshot()`.


**Example**

```R
#configure our remote session
REMOTE>install.packages(c("arules","bitops","caTools"))
>putLocalFile("C:/data/survey_reference.csv")
>putLocalObject("model")
>snapshot_id<-<-createSnapshot("my modeling environment")

#whenever I need the modeling environment, reload the snapshot
>loadSnapshot(snapshot_id)  
#execute an R script remotely
>remoteScript("c/myScript2.R")
```

## Next steps

Once you understand the mechanics of remote execution, consider incorporating web service capabilities. You can publish an R web service composed of arbitrary R code block that runs on the remote R Server. For more information, see the [Web Service vignette](mrsdeploy-websrv-vignette.md).
