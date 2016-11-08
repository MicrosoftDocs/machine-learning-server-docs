---

# required metadata
title: "R Package Management"
description: "Operationalization of R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
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

# R Package Management with DeployR

Whenever you or your users are writing, testing, and deploying R scripts, it is critical that the packages (and their dependencies) needed by that R code are available at runtime or the execution will fail. 

By adopting one or both R package management approaches described below, your data scientists can avoid such issues where a script they've tested locally now fails due to missing package dependencies when executed in the remote DeployR server environment.

To simplify R package management, review the following approaches for the best practices for production and non-production environments. 

## Non-Production/Development Environments

To avoid issues where a script you've tested locally now fails due to missing package dependencies when executed in the DeployR server environment, you can install the needed R packages into the workspace of a remote R session yourself. 

This remote execution and snapshotting approach provides an excellent way to:
+ Try out new package or package versions without posing any risks to a stable production environment
+ Install packages without requiring any intervention from the administrator

The packages you install using this method do not 'contaminate' the production environment for other users since they are only available in the context of the given R session. Those packages remain installed for the lifecycle of the R session. You can prolong this lifecycle by saving the session workspace and working directory into a **snapshot** and then recalling the snapshot using its ID later whenever you want access to the workspace, the installed R packages, and the files in the working directory as they were at the time the snapshot was created. @LINK TO VIGNETTE DOC IN MSDN "LEARN MORE ABOUT SNAPSHOTS AND REMOTE EXECUTION"

**To install packages within an R session:**
1. Develop and test your code locally.
1. Load the `mrsdeploy` package.
   ```
   > library(mrsdeploy)
   ```
1. Authenticate to create the remote session. @LINK TO VIGNETTE DOC IN MSDN    For example, for Azure Active Directory:
   ```
   > remote_login_aad([AAD_properties])
   
   REMOTE> 
   ```

   A new remote session is started.

1. Run R code in the remote environment:

   1. Install new R packages and upload any needed R objects and files into the remote R session. @LINK TO VIGNETTE DOC IN MSDN

      @@@ EXAMPLE NEEDED
   
   1. Pause the remote session and execute your R script(s) to test the code and newly installed packages in the remote environment. @LINK TO VIGNETTE DOC IN MSDN
   ```
   REMOTE> pause()
   > remote_script("my-script.R")
   ```

1. To allow the workspace and working directory to be reused later, create a session snapshot.  @LINK TO VIGNETTE DOC IN MSDN
   ```
   REMOTE>pause()
   >create_snapshot("my-snapshot-name")
   $snapshotId
   [1] "123456789-abcdef-123456789"
   ```

   >Take note of the `snapshotId` to call this snapshot later.
   > Learn more about snapshots here @@@ADD LINK
    

**To reuse those installed packages, objects and files:**

You can use the snapshot (installed packages, objects, files) within the context of a web service in a remote session. When you create a web service, reference the snapshot ID. Then, when the service is executed, the snapshot will be loaded and the session information is automatically available to the service. @LINK TO VIGNETTE DOC IN MSDN

@@@CAN WE PROVIDE AN EXAMPLE??

>You can request that a package be installed across the configuration for all users by the administrator once you've sufficiently tested the package(s) as described in this section.

## Production Environments

As the DeployR administrator, one of your responsibilities is to ensure the R code that runs on the DeployR server has access to the R package dependencies declared within that code. 

This means that the right set of R package versions have been installed for the organization and are accessible to all users.

There are several ways you can achieve this goal. Two options are described here.

### Option 1: Create a Local Package Repository Using miniCRAN

You can create a local R package repository using the R package miniCRAN. This can be very useful when the server does not have Internet connectivity since the standard method of installing R packages (the R command install.packages()) might not work.

There are two options for installing packages from a local share or repository:
Use the miniCRAN package to create a local repository of the packages you need, then install from this repository. This topic describes the miniCRAN method.
Download the packages you need, and their dependencies, as zip files, and save them in a local folder, and then copy that folder to the SQL Server computer. For more information on the manual copy method, see Install Additional Packages on SQL Server.
Step 1. Install miniCRAN and download packages
Install the miniCRAN package on a computer that has Internet access.
# Install miniCRAN ---------------------------------------------------

if(!require("miniCRAN")) install.packages("miniCRAN")
if(!require("igraph")) install.packages("igraph")
library(miniCRAN)

# Define the package source: a CRAN mirror, or an MRAN snapshot
CRAN_mirror <- c(CRAN = "https://mran.microsoft.com/snapshot/2016-04-01")

# Define the local download location
local_repo <- "~/miniCRAN"

Download or install the packages you need to this computer. This will create the folder structure that you need to copy the packages to the SQL Server later.
# List the packages you need 
# Do not specify dependencies
pkgs_needed <- c("ggplot2", "ggdendro")

Copy the miniCRAN repository to the R_SERVICES library on the SQL Server instance.
Step 2. Install the packages on the SQL Server computer
On the SQL Server computer, run the R command install.packages(). You can use one of the R tools that are installed with R Services (In-database), such as Rgui.exe, or you can run the command as part of a Transact-SQL stored procedure.
At the prompt to specify a repository, select the folder containing the files you just copied; that is, the local miniCRAN repository.
pkgs_needed <- c("ggplot2", "ggdendro")
local_repo  <- "~/miniCRAN"

.libPaths()[1]
"C:/Program Files/Microsoft SQL Server/130/R_SERVER/library"

lib <- .libPaths()[1]

install.packages(pkgs_needed, 
              repos = file.path("file://", normalizePath(local_repo, winslash = "/")),
              lib = lib,
              type = "win.binary",
              dependencies = TRUE
              )

Verify that the packages were installed by running this R code.
installed.packages()

Acknowledgements
The source for this information is this article by Andre de Vries, who also developed the miniCRAN package. For details and a complete walkthrough, see How to install R packages on an off-line SQL Server 2016 instance


### Option 2: Create a Master Script with the List of Approved Packages

To achieve this end, we recommend you use an R script to install a master list of default packages across the DeployR configuration on behalf of your users. That master script ensures that the same versions of each declared package (along with all of its required package dependency) are installed each time it is run. To produce the list of packages, consider the following:
+ Which R packages (and versions) are needed and sanctioned for production.
+ Requests from users to add new R packages or update package versions.


This production-safe approach provides an excellent way to:
+ Keep a standard (and sanctioned) library of R packages for production use.
+ Manage R package dependencies as well as package versions.
+ Schedule timely updates to R packages.

**To use a master script to install packages:**

1. Create the master list of packages (and versions) in an R script format.  For example:
   ```
   install.packages("Package-A")
   install.packages("Package-B")
   install.packages("Package-C")
   install.packages("Package-...")
   ```

1. Manually run this R script on each back-end.

>You must update and manually rerun this script on each back-end whenever a new package or version is needed in the server environment.