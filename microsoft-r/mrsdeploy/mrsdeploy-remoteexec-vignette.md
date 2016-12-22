---

# required metadata
title: mrsdeploy Remote Execution functions in Microsoft R"
description: "Remote execution functions in the mrsdeploy package in Microsoft R are used for command line interaction with a remote R Server instance from a console application."
keywords: "mrsdeploy package"
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

# mrsdeploy Remote Execution functions in Microsoft R

This article describes the Remote Execution functions in the [mrsdeploy package](mrsdeploy.md).

## Using the remote command line

The remote command line allows you to directly interact with an R Server 9.0.1 instance on another machine. You can enter 'R' code just as you would in a local R console. R code entered at the remote command line executes on the remote server.

To establish a remote session, issue a remote login request, which in turn authenticates your user identity on the remote server. Once the session is established, you can switch between local and remote command lines through the `pause()` and `resume()` functions.

### Create a remote session

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

## See also

[mrsdeploy Function Reference](mrsdeploy.md)

[mrsdeploy introduction](mrsdeploy-intro-vignette.md)

[Package Reference](../package-reference.md)
