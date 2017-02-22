---

# required metadata
title: "R Package Management with R Server | Microsoft R Server Docs"
description: "R Package Management with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "02/08/2017"
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
ms.technology: 
  - deployr
  - r-server
ms.custom: ""
---

# R Package Management for Operationalization

**Applies to:  Microsoft R Server 9.0.1**

One of the strengths of the R language is the thousands of third-party packages that have been made publicly available via CRAN, the Comprehensive R Archive Network. R includes a number of functions that make it easy to download and install these packages. Whenever you or your users are writing, testing, and deploying R scripts, it is imperative that the packages (and their dependencies) needed by that R code are available at runtime or the execution will fail. 

By adopting one or both R package management approaches described below, your data scientists can avoid such issues where a script they've tested locally now fails due to missing packages or package dependencies when executed in the remote environment.

As the R Server administrator, one of your responsibilities is to ensure the R code that runs on the compute node(s) has access to the R package dependencies declared within that code. This means that the right set of R package versions have been installed for the organization and are accessible to all users.  

However, in many enterprise environments, access to the Internet is limited or non-existent. In such environments, it is useful to create a local package repository that users can access from within the corporate firewall using [Option 1](#offline).

You can also manually install the packages using a master script using [Option 2](#master).

Of course, data scientists can also test out new packages without risk to the production environment using [Option 3 with the `mrsdeploy` package](#mrsdeploy). 

<br>

<a name="offline"></a>
## Option 1: Local Package Repository (Offline Solution)

>**Audience:** System administrator
>
>**Applies to:** Development -or- Production Environments

You can create a local R package repository of the R packages you need using the R package `miniCRAN`. You can then copy this repository to all compute nodes and then install directly from this repository. 

This production-safe approach provides an excellent way to:
+ Keep a standard, sanctioned library of R packages for production use
+ Allow packages to be installed in an offline environment


**To use the miniCRAN method:**

1. On the machine with Internet access:

   1. Launch your preferred R IDE or an R tool such as Rgui.exe.

   1. At the R prompt, install the `miniCRAN` package on a computer that has Internet access.
      ```
      if(!require("miniCRAN")) install.packages("miniCRAN")
      if(!require("igraph")) install.packages("igraph")
      library(miniCRAN)
      ```   
   
   1. To point to a different snapshot, set the `CRAN_mirror` value. By default, the CRAN mirror specified by your version of Microsoft R Open will be used. For example, for Microsoft R Server 9.0.1 that date is 2016-11-01.

      ```
      # Define the package source: a CRAN mirror, or an MRAN snapshot
      CRAN_mirror <- c(CRAN = "https://mran.microsoft.com/snapshot/2016-11-01")
      ```

   1. Create a miniCRAN repository in which the packages will be downloaded and installed.  This will create the folder structure that you need to copy the packages to each compute node later.
      ``` 
      # Define the local download location
      local_repo <- "~/my-miniCRAN-repo"
      ```

   1. Download and install the packages you need to this computer. 
      ```       
      # List the packages you need 
      # Do not specify dependencies
      pkgs_needed <- c("Package-A", "Package-B", "Package-...")
      ```

1. On each compute node:
   1. Copy the miniCRAN repository from the machine with Internet connectivity to the R_SERVICES library on the SQL Server instance.

   1. Launch your preferred R IDE or an R tool such as Rgui.exe.
   
   1. At the R prompt, run the R command `install.packages()`. 

   1. At the prompt, specify a repository and specify the directory containing the files you just copied; that is, the local miniCRAN repository.
      ```
      pkgs_needed <- c("Package-A", "Package-B", "Package-...")
      local_repo  <- "~/my-miniCRAN-repo"
         
      install.packages(pkgs_needed, 
	          repos = file.path("file://", normalizePath(local_repo, winslash = "/")),
	          dependencies = TRUE
      )
      ```

   1. Verify that the packages were installed by running the following R command and reviewing the list of packages returned:  
      ```
      installed.packages()
      ```

<br>

<a name="master"></a>

## Option 2: R Script with List of Approved Packages

>**Audience:** System administrator
>
>**Applies to:** Development -or- Production Environments

As we mentioned before, it is imperative that the right set of R package versions have been installed and are accessible to all users. To achieve this end, another recommended option involves using an R script to install a master list of default packages across the configuration on behalf of your users. That master script ensures that the same versions of each declared package (along with all of its required package dependency) are installed each time it is run. 

To produce the list of packages, consider which R packages (and versions) are needed and sanctioned for production. Also consider requests from users to add new R packages or update package versions.

This production-safe approach provides an excellent way to
+ Keep a standard (and sanctioned) library of R packages for production use.
+ Manage R package dependencies as well as package versions.
+ Schedule timely updates to R packages.

This option does require the  machines hosting the compute node have access to the Internet to install the packages.

**To use a master script to install packages:**

1. Create the master list of packages (and versions) in an R script format.  For example:
   ```
   install.packages("Package-A")
   install.packages("Package-B")
   install.packages("Package-C")
   install.packages("Package-...")
   ```

1. Manually run this R script on each compute node.

>You must update and manually rerun this script on each compute node whenever a new package or version is needed in the server environment.

<br>

<a name="mrsdeploy"></a>
## Option 3: Remote Session Testing

>**Audience:** Data Scientist or the system administrator
>
>**Applies to:** Development Environments

To avoid issues where a script you've tested locally now fails due to missing package dependencies when executed in the R Server environment, you can install the needed R packages into the workspace of a remote R session yourself. 

This remote execution and snapshotting approach provides an excellent way to:
+ Try out new package or package versions without posing any risks to a stable production environment
+ Install packages without requiring any intervention from the administrator

The packages you install using this method do not 'contaminate' the production environment for other users since they are only available in the context of the given R session. Those packages remain installed for the lifecycle of the R session. You can prolong this lifecycle by saving the session workspace and working directory into a **snapshot** and then recalling the snapshot using its ID later whenever you want access to the workspace, the installed R packages, and the files in the working directory as they were at the time the snapshot was created. 

[Learn more about snapshots and remote execution...](remote-execution.md)

>[!Important]
>For optimal performance, consider the size of the snapshot carefully especially when publishing a service. Before creating a snapshot, ensure that keep only those workspace objects you need and purge the rest. 

**Note:** After you've sufficiently tested the package(s) as described in this section, you can request that a package be installed across the configuration for all users by the administrator.

**To install packages within an R session:**

1. Develop and test your code locally.

1. Load the `mrsdeploy` package.
   ```
   > library(mrsdeploy)
   ```

1. Authenticate to create the remote session.  Learn more about the authentication functions and their arguments in the article: ["Connecting to R Server from mrsdeploy"](../operationalize/mrsdeploy-connection.md).  

   + For example, for Azure Active Directory:
     ```
     > remoteLoginAAD("http://localhost:12800",
                   authuri = "https://login.windows.net",
                   tenantid = "myMRSServer.contoso.com",
                   clientid = "00000000-0000-0000-0000-000000000000",
                   resource = "00000000-0000-0000-0000-000000000000",
                   session=FALSE,
                   diff=TRUE,
                   commandline=FALSE)
   
     REMOTE> 
     ```

   + For example, for Active Directory/LDAP:
     ```
     > remoteLogin("https://localhost:12800", 
                   session=TRUE, 
                   diff=TRUE, 
                   commandline=TRUE)
   
     REMOTE> 
     ```

   A new remote session is started.

1. Run R code in the remote environment:

   1. Install new R packages and upload any needed R objects and files into the remote R session. 
      ```
      REMOTE> install.packages("ggplot2")
      ```
   
   1. Pause the remote session and execute your R script(s) to test the code and newly installed packages in the remote environment. 
      ```
      REMOTE> pause()
      > remoteScript("my-script.R")
      ```

1. To allow the workspace and working directory to be reused later, create a session snapshot. A snapshot is a prepared environment image of a R session saved to Microsoft R Server, which includes the session's R packages, R objects and data files. This snapshot can be loaded into any subsequent remote R session for the user who created it. [Learn more about snapshots.](remote-execution.md)

   ```
   REMOTE>pause()
   >create_snapshot>snapshotId<-createSnapshot("my-snapshot-name")
   $snapshotId
   [1] "123456789-abcdef-123456789"
   ```

   >Take note of the `snapshotId` to call this snapshot later.


**To reuse those installed packages, objects and files:**

You can reload the snapshot (installed packages, objects, files) within the context of a remote session. 

```
> loadSnapshot("123456789-abcdef-123456789")
```

You can request that a package be installed across the configuration for all users by the administrator once you've sufficiently tested the package(s) as described in this section.