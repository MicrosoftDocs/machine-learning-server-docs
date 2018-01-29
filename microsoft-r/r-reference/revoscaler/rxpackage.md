--- 
 
# required metadata 
title: "rxPackage function (revoAnalytics) | Microsoft Docs" 
description: " This article explains how to enable and disable R package management on SQL Server Machine Learning Services (in-database), as well as installation, usage, and removal of individual packages. **RevoScaleR** provides the necessary rx functions for these tasks. " 
keywords: "(revoAnalytics), rxPackage, packages, sql, install, uninstall, remove, use" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 #rxPackage: R Package Management in SQL Server 
 ##Description
 
This article explains how to enable and disable R package management on SQL Server Machine Learning Services (in-database), as well as installation, usage, and removal of individual packages.
**RevoScaleR** provides the necessary rx functions for these tasks.
 
 
 ##Details
 
SQL Server Machine Learning Services and the previous version, SQL Server R Services, support install and uninstall of R packages on SQL Server. A database administrator (in `db_owner` role) must grant permissions to allow access to package functions at both the database and instance level.

R package management functions are part of the base distribution. These functions allows packages to be installed from a local or remote repository into a local library (folder). 
R provides the following core functions for managing local libraries:



* 
 available.packages() - enumerate packages available in a repository for installation

* 
 installed.packages() - enumerate installed packages in a library

* 
 install.packages() - install packages, including dependency resolution, from a repository into a library

* 
 remove.packages() - remove installed packages from a library

* 
 library() - load the installed package and access its functions



RevoScaleR also provides package management functions, which is especially useful for managing packages in a SQL Server compute context in a client session. 
Packages in the database are reflected back on the file system, in a secure location. Access is controlled through database roles, which determine whether you can install packages from a local or remote repository into a database.

RevoScaleR provides the following core client functions for managing libraries in a database on a remote SQL server:


* 
 [rxInstalledPackages](rxInstalledPackages.md)() - enumerate installed packages in a database on SQL Server

* 
 [rxInstallPackages](rxInstallPackages.md)() - install packages, including dependency resolution, from a repository onto a library in a database. Simultaneously install the same packages on the file system using per-database and per-user profile locations.

* 
 [rxRemovePackages](rxRemovePackages.md)() - remove installed packages from a library in a database and further uninstall the packages from secured per-database, per-user location on SQL server

* 
 [rxSqlLibPaths](rxSqlLibPaths.md)() - get secured library paths for the given user to refer to the installed packages for SQL Server to then use it in .libPaths() to refer to the packages

* 
 library() - same as R equivalent; used to load the installed package and access its functions

* 
 [rxSyncPackages](rxSyncPackages.md)() - synchronize packages from a database on SQL Server to the file system used by R




Ther are two scopes for installation and usage of packages in a particular database in SQL Server:


* 
 `Shared scope` - Share the packages with other users of the same database.

* 
 `Private scope` - Isolate a packge in a per-user private location, accessible only to the user installing the package.



Both scopes can be used to design different secure deployment models of packages. For example, using a `shared` scope, data scientist department heads can be granted permissions to install packages, which can then be used by all other data scientists in the group. In another deployment model, the data scientists can be granted `private` scope permissions to install and use their private packages without affecting other data scientists using the same server.


Database roles are used to secure package installation and access:


* 
 `rpkgs-users` - allows users to use shared packages installed by users belong to `rpkgs-shared` role

* 
 `rpkgs-private` - allows all permissions as `rpkgs-users` role and also allows users to install, remove and use private packages

* 
 `rpkgs-shared` - allows all permissions as `rpkgs-private` role and also allows users to install, remove shared packages

* 
 `db_owner` - allows all permissions as `rpkgs-shared` role and also allows users to install & remove shared and private packages for other users for management




**Enable R package management on SQL Server**


By default, R package management is turned off at the instance level. To use this functionality, the administrator must do the following:


* 
 Enable package management on the SQL Server instance. 

* 
 Enable package management on the SQL database. 



**`RegisterRExt.exe`** command line utility, which ships with RevoScaleR, allows administrators to enable package management for instances and specific databases. You can find RegisterRExt.exe at `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe`.


To enable R package management at instance level, open an elevated command prompt and run the following command:


* 
 **`RegisterRExt.exe /installpkgmgmt [/instance:name] [/user:username] [/password:*|password]`**



This command creates some package-related, instance-level artifacts on the SQL Server machine.

Next, enable R package management at database level using an elevated command prompt and the following command:


* 
 **`RegisterRExt.exe /installpkgmgmt /database:databasename  [/instance:name] [/user:username] [/password:*|password]`**

This command creates database artifacts, including `rpkgs-users`, `rpkgs-private` and `rpkgs-shared` database roles to control user permissions who can install, uninstall, and use packages.



**Disable R package management on a SQL Server**

To disable R package management on a SQL server, the administrator must do the following:


* 
 Disable package management on the database. 

* 
 Disable package management on the SQL Server instance. 



Use the **`RegisterRExt.exe`** command line utility located at `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe`.

To disable R package management at the database level, open an elevated command prompt and run the following command:


* 
 **`RegisterRExt.exe /uninstallpkgmgmt /database:databasename  [/instance:name] [/user:username] [/password:*|password]`**

This command removes the package-related database artifacts from the database, as well as the packages in the secured file system location.



After disabling package management at the database level, disable package management at instance level by running the following command at an elevated command prompt:


* 
 **`RegisterRExt.exe /uninstallpkgmgmt [/instance:name] [/user:username] [/password:*|password]`**

This command removes the package-related, per-instance artifacts from the SQL Server. 


 
 
 
 ##See Also
 
[rxSqlLibPaths](rxSqlLibPaths.md),
[rxInstalledPackages](rxInstalledPackages.md),
[rxInstallPackages](rxInstallPackages.md),   
[rxRemovePackages](rxRemovePackages.md),
[rxFindPackage](rxFindPackage.md),
library
require
   
 ##Examples

 ```
   
  ## Not run:
 
#
# install and remove packages from client 
#
sqlcc <- RxInSqlServer(connectionString = "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;")

pkgs <- c("dplyr")
rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)


#
# use the installed R package from rx function like rxExec(...)
#
usePackageRxFunction <- function()
{
  library(dplyr)
  
  # returns list of functions contained in dplyr
  ls(pos="package:dplyr")
  
  #
  # more code to use dplyr functionality
  #
  # ...
  #
  
}
rxSetComputeContext(sqlcc)
rxExec(usePackageRxFunction)


#
# use the installed R packages in an R function call running on SQL server using T-SQL
#
declare @instance_name nvarchar(100) = @@SERVERNAME, @database_name nvarchar(128) = db_name();
exec sp_execute_external_script 
  @language = N'R',
  @script = N'
    #
    # setup the lib paths to private and shared libraries
    #
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    .libPaths(rxSqlLibPaths(connStr));

    #
    # use the installed R package from rx function like rxExec(...)
    #
    library("dplyr");
    
    #
    # more code to use dplyr functionality
    #
    # ...
    #
  ', 
  @input_data_1 = N'', 
  @params = N'@instance_name nvarchar(100), @database_name nvarchar(128)',
  @instance_name = @instance_name, 
  @database_name = @database_name;
 ## End(Not run) 
  
 
```
 
 
 
 
 
 
 
