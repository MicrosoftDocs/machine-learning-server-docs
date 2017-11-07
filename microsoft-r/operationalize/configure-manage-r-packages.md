---

# required metadata
title: "R Package Management when operationalizing - Machine Learning Server "
description: "R Package Management with Machine Learning Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "9/25/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""
---

# R package management when operationalizing analytics with Machine Learning Server

**Applies to:  Machine Learning Server, Microsoft R Server 9.x**

One of the strengths of the R language is the thousands of third-party packages that have been made publicly available via CRAN, the Comprehensive R Archive Network. R includes various functions that make it easy to download and install these packages. Whenever you deploy an R script, the packages (and their dependencies) needed by that R code must be available at runtime, or else the execution fails. 

By adopting the R package management approaches discussed herein, data scientists can avoid issues such as a locally tested script tested failing due to missing package dependencies in the remote environment.

As the Machine Learning Server administrator, you must ensure that the R code running on any compute nodes has access to the R package dependencies declared within that code. The right set of R package versions must be installed for the organization and accessible to all users.  

However, in many enterprise environments, access to the Internet is limited or non-existent. In such environments, it is useful to create a local package repository that users can access from within the corporate firewall using [Option 1](#offline).

You can also manually install the packages using a master script using [Option 2](#master).

Data scientists can also test out packages without risk to the production environment using [the mrsdeploy package option (3)](#mrsdeploy). 

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
      ```R
      if(!require("miniCRAN")) install.packages("miniCRAN")
      if(!require("igraph")) install.packages("igraph")
      library(miniCRAN)
      ```   
   
   1. To point to a different snapshot, set the `CRAN_mirror` value. By default, the CRAN mirror specified by your version of Microsoft R Open is used. For example, for Machine Learning Server 9.2.1 that date is 2017-09-01.

      ```R
      # Define the package source: a CRAN mirror, or an MRAN snapshot
      CRAN_mirror <- c(CRAN = "https://mran.microsoft.com/snapshot/2016-11-01")
      ```

   1. Create a miniCRAN repository in which the packages are downloaded and installed.  This repository creates the folder structure that you need to copy the packages to each compute node later.
      ```R
      # Define the local download location
      local_repo <- "~/my-miniCRAN-repo"
      ```

   1. Download and install the packages you need to this computer. 
      ```R     
      # List the packages you need 
      # Do not specify dependencies
      pkgs_needed <- c("Package-A", "Package-B", "Package-...")
      ```

1. On each compute node:
   1. Copy the miniCRAN repository from the machine with Internet connectivity to the R_SERVICES library on the SQL Server instance.

   1. Launch your preferred R IDE or an R tool such as Rgui.exe.
   
   1. At the R prompt, run the R command install.packages(). 

   1. At the prompt, specify a repository and specify the directory containing the files you copied. That is, the local miniCRAN repository.
      ```R
      pkgs_needed <- c("Package-A", "Package-B", "Package-...")
      local_repo  <- "~/my-miniCRAN-repo"
         
      install.packages(pkgs_needed, 
	          repos = file.path("file://", normalizePath(local_repo, winslash = "/")),
	          dependencies = TRUE
      )
      ```

   1. Run the following R command and reviewing the list of installed packages:  
      ```R
      installed.packages()
      ```

<br>

<a name="master"></a>

## Option 2: R Script with List of Approved Packages

>**Audience:** System administrator
>
>**Applies to:** Development -or- Production Environments

As we mentioned before, it is imperative that the right set of R package versions are installed and accessible to all users. Option 2 makes use of a master R script containing the list of specific package versions to install across the configuration on behalf of your users. Using a master script ensures that the same packages (along with all its required package dependency) are installed each time. 

To produce the list of packages, consider which R packages (and versions) are needed and sanctioned for production. Also consider requests from users to add new R packages or update package versions.

This production-safe approach provides an excellent way to
+ Keep a standard (and sanctioned) library of R packages for production use.
+ Manage R package dependencies and package versions.
+ Schedule timely updates to R packages.

This option does require the  machines hosting the compute node have access to the Internet to install the packages.

**To use a master script to install packages:**

1. Create the master list of packages (and versions) in an R script format.  For example:
   ```R
   install.packages("Package-A")
   install.packages("Package-B")
   install.packages("Package-C")
   install.packages("Package-...")
   ```

1. Manually run this R script on each compute node.

>Update and manually rerun this script on each compute node each time a new package or version is needed in the server environment.

<br>

<a name="mrsdeploy"></a>
## Option 3: Remote Session Testing

>**Audience:** Data Scientist or the system administrator
>
>**Applies to:** Development Environments

To avoid issues where a locally tested script fails in the Machine Learning Server environment due to missing package dependencies, install the R packages into the workspace of a remote R session yourself. 

This remote execution and snapshot approach provides an excellent way to:
+ Try out new package or package versions without posing any risks to a stable production environment
+ Install packages without requiring any intervention from the administrator

The packages you install using this method do not 'contaminate' the production environment for other users since they are only available in the context of the given R session. Those packages remain installed for the lifecycle of the R session. You can prolong this lifecycle by saving the session workspace and working directory into a **snapshot**. Then, whenever you want to access the workspace, the installed R packages, and the working directory files as they were, you can recall the snapshot using its ID. 

[Learn more about snapshots and remote execution...](../r/how-to-execute-code-remotely.md)

>[!Important]
>For optimal performance, consider the size of the snapshot carefully especially when publishing a service. Before creating a snapshot, ensure that keep only those workspace objects you need and purge the rest. 

**Note:** After you've sufficiently tested the packages as described in this section, you can request that the administrator install a package across the configuration for all users.

**To install packages within an R session:**

1. Develop and test your code locally.

1. Load the mrsdeploy package.
   ```R
   > library(mrsdeploy)
   ```

1. Authenticate to create the remote session.  Learn more about the authentication functions and their arguments in the article: "[Connecting to Machine Learning Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md). " 

   + For example, for Azure Active Directory:
     ```R
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
     ```R
     > remoteLogin("https://localhost:12800", 
                   session=TRUE, 
                   diff=TRUE, 
                   commandline=TRUE)
   
     REMOTE> 
     ```

   A new remote session is started.

1. Run R code in the remote environment:

   1. Install new R packages and upload any needed R objects and files into the remote R session. 
      ```R
      REMOTE> install.packages("ggplot2")
      ```
   
   1. Pause the remote session and execute your R scripts to test the code and newly installed packages in the remote environment. 
      ```R
      REMOTE> pause()
      > remoteScript("my-script.R")
      ```

1. To allow the workspace and working directory to be reused later, create a session snapshot. A snapshot is a prepared environment image of an R session saved to Machine Learning Server, which includes the session's R packages, R objects and data files. This snapshot can be loaded into any subsequent remote R session for the user who created it. [Learn more about snapshots.](../r/how-to-execute-code-remotely.md)

   ```R
   REMOTE>pause()
   >create_snapshot>snapshotId<-createSnapshot("my-snapshot-name")
   $snapshotId
   [1] "123456789-abcdef-123456789"
   ```

   >Take note of the `snapshotId` to call this snapshot later.


**To reuse those installed packages, objects and files:**

You can reload the snapshot (installed packages, objects, files) within the context of a remote session. 

```R
> loadSnapshot("123456789-abcdef-123456789")
```

You can ask your administrator to install a package across the configuration for all users once you've sufficiently tested the package as described in this section.

>[!WARNING]
>**R Server 9.0 users!** When loading a library for the REMOTE session, set lib.loc=getwd() as such: 
`library("<packagename>", lib.loc=getwd())`