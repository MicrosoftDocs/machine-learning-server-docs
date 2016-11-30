---

# required metadata
title: mrsdeploy Remote Execution functions in Microsoft R (vignette)"
description: "Remote execution functions in the mrsdeploy package in Microsoft R are used for command line interaction with a remote R Server instance from a console application."
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

# mrsdeploy Remote Execution functions in Microsoft R (vignette)

This article provides the vignette documentation for Remote Execution functions in the [mrsdeploy package](mrsdeploy.md).

## Using the remote command line

The remote command line allows you to directly interact with an R Server 9.0.1 instance on another machine. You can enter 'R' code just as you would in a local R console. R code entered at the remote command line executes on the remote server. Switching between the local command line and the remote command line is done using these functions: `pause()` and `resume()`. To establish a remote session, you must issue a remote login request, which in turn authenticates your user identity on the remote server.

### Creating a remote session

The `remote_login` and `remote_login_aad` functions are used to authenticate against R Server, creating a remote session.

**Example**

```R
#authenticate against the remote R Server
remote_login("https://localhost:12800", session=TRUE, diff=TRUE, commandline=TRUE)

#authenticate against the R Server using Azure Active Directory
remote_login_aad("http://localhost:12800",
                   authuri = "https://login.windows.net",
                   tenantid = "deployrtest.onmicrosoft.com",
                   clientid = "10f6b06f-9a75-4222-8888-ef793969c7d4",
                   resource = "a053a63f-8af5-480b-9510-48bb32e44be8",
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
>resume()  #switches the user back to the remote R session

REMOTE>exit  #logout and terminate the remote R session
>remote_logout()  #logout and terminate the remote R session
```

## Diff report
A `diff` report is available so you can see and manage differences between the local and remote R environments.
The diff reports contains information regarding R versions, R packages installed locally, but not on the remote
session, and differences between R package versions. This report is shown by default when you login, but can be
run anytime by executing the function: `diff_local_remote()`.

## Executing an R script remotely

If you have R scripts on your local machine, you can execute them remotely by using the function `remote_script()`.
This function takes either a path to an R script to be executed remotely. You also have options
to save or display any plots that might have been generated during the execution. The function returns a list
containing the status of the execution (success/failure), the console output generated, and a list of files created.

If your R Script has R Package dependencies, those packages must be installed on the remote
server. You can either have your Administrator install them globally by logging in directly to the server,
or you can install them for the duration of the remote session by using the R function `install.packages()`. Leave the `lib` parameter empty.

If you need more granular control of a remote execution scenario, you can use the `remote_execute()` function.

**Example**

```R
#install a package for the life of the session
REMOTE>install.packages("bitops")

#switch to the local R session
REMOTE>pause()
#execute an R script remotely
>remote_script("c/myScript.R")    
```
## Working with R objects and files remotely

Once you have executed an R code remotely, you may want to retrieve certain R objects and load them into your local R session. For example, if you have an R script that creates a linear model (i.e. `m<-lm(x~y)` ), and you want to work with that model in your local R session, you can retrieve the object `m` by using the function `get_remote_object()`.

Conversely, if you have a local R object that you want to make available to your remote R session, you can use the function `put_local_object()`. If you want to sync your local and remote workspaces, the functions `put_local_workspace()` and `get_remote_workspace()` can be used.

Similar capabilities are available for files that need to be moved between the local and remote R sessions.

The following functions are available for working with files:  `put_local_file()`, `get_remote_file()`, `list_remote_files()` and `delete_remote_file()`.

**Example**

```R
#execute a script remotely that generated 2 R objects we are interested in retrieving
>remote_execute("C:/myScript.R')
#retreive the R objects from the remote R session and load them into our local R session
>get_remote_object(c("model","out"))

#an R script depends upon an R object named `data` to be available. Move the local
#instance of `data` to the remote R session
>put_local_object("data")
#execute an R script remotely
>remote_script("c/myScript2.R")

#push a data file to the remote R session
>put_local_file("C:/data/survey.csv")
#execute an R script remotely
>remote_script("c/myScript2.R")
```

## Snapshots and why they are useful

If you need a prepared environment for your remote script execution that includes R packages, objects, or data files, consider creating a `snapshot`. A snapshot is a image of the working directory and workspace of the remote R session. Snapshots are saved to the R Server and can be loaded into any subsequent remote R session. For example, suppose you want to execute a script that needs 3 R packages, a reference data file, and a model object. Instead of loading these items each time you wish to execute the script, create a snapshot of those items and then just load the snapshot when you need that particular configuration.

The following functions are available for working with snapshots:  `list_snapshots()`, `create_snapshot()`, `load_snapshot(), `download_snapshot()` and `delete_snapshot()`.

**Example**

```R
#configure our remote session
REMOTE>install.packages(c("arules","bitops","caTools"))
>put_local_file("C:/data/survey_reference.csv")
>put_local_object("model")
>snapshot_id<-<-create_snapshot("my modeling environment")

#whenever I need the modeling environment, reload the snapshot
>load_snapshot(snapshot_id)  
#execute an R script remotely
>remote_script("c/myScript2.R")
```

## See also

[mrsdeploy Function Reference](mrsdeploy.md)

[Vignette introduction](mrsdeploy-intro-vignette.md)

[Package Reference](../package-reference.md)
