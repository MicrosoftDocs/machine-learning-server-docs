--- 
 
# required metadata 
title: "SQL R Services Package Management" 
description: " **NOTE: This feature and the rx APIs listed below are in pre-release mode and subject to change before final release.**  This section describes how to enable & disable SQL R Services package management on SQL server, install R packages on SQL server database, use installed packages on SQL server database and remove R packages on SQL server. **RevoScaleR** provides the necessary rx functions to install and uninstall packages. " 
keywords: "RevoScaleR, rxPackage, packages, sql, install, uninstall, remove, use" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #`rxPackage`: SQL R Services Package Management

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
**NOTE: This feature and the rx APIs listed below are in pre-release mode and subject to change before final release.**

This section describes how to enable & disable SQL R Services package management on SQL server, install R packages on SQL server database, use installed packages on SQL server database and remove R packages on SQL server.
**RevoScaleR** provides the necessary rx functions to install and uninstall packages.
 
 
 ##Details
 
With SQL R Services 9.0 version, included in SQL vNext CTP 1.0, we are introducing a feature to allow data scientists to install and uninstall packages on the SQL server. The database administrator (in `db_owner` role) needs to grant permissions to data scientist users to use their packages for advanced analytics computation on the SQL server at scale instead of pulling the data out to the client box forcing them work with a smaller scale data.

R provides package management functionality as part of the base distribution. This functionality allows packages to be installed from a local or remote repository on to a local library (folder). R provides the following core functions for managing local libraries:



* 
 available.packages() - to enumerate packages available in a repository for installation

* 
 installed.packages() - to enumerate installed packages in a library

* 
 install.packages() - to install packages, including dependency resolution, from a repository to onto a library

* 
 remove.packages() - to remove installed packages from a library

* 
 library() - to load the installed package and use the functionality provided by the package



SQL R services package management provides similar functionality to manage packages on a SQL server from a client R session using RevoScaleR package. It uses the SQL server compute context and a set of functions to allow remote package management functionality. 

In SQL R services package management, the packages are installed from a local or remote repository onto to a database. These packages in database are then reflected back on the file system on a secured location on the SQL R services. The access to these packages are controlled through database roles defined on the database. 

SQL R services provides the following core client functions for managing libraries in a database on a remote SQL server:


* 
 [rxInstalledPackages](rxInstalledPackages.md)() - to enumerate installed packages in a database on SQL server

* 
 [rxInstallPackages](rxInstallPackages.md)() - to install packages, including dependency resolution, from a repository to onto a library in a database and further install the same packages on a secured per database, per user location on file system on SQL server

* 
 [rxRemovePackages](rxRemovePackages.md)() - to remove installed packages from a library in a database and further uninstall the packages from secured per database, per user location on SQL server

* 
 [rxSqlLibPaths](rxSqlLibPaths.md)() - to get secured library paths for the given user to refer to the installed packages for SQL server to then use it in .libPaths() to refer to the packages

* 
 library() - same as R functionality to load the installed package and user the functionality provided by the package

* 
 [rxSyncPackages](rxSyncPackages.md)() - to synchronize packages from database on SQL server to the file system used by R




SQL R services package management provides two scopes for installation & usage of packages in a particular database in SQL server:


* 
 `Shared scope` - this scope allows allowed users to install & uninstall packages in per database shared location which in turn can be used by other users of the database on SQL server.

* 
 `Private scope` - this scope allows allowed users to install & uninstall packages in per database per user private location which can only be used by the single user of the database on SQL server.



Both the scopes can be used to come up with different secure deployment models of packages on SQL server. For e.g. using `shared` scope data scientist department heads can be granted permissions to install packages which can then be used by all other data scientists in the group on SQL server. In another deployment model the data scientists can be granted `private` scope permissions to install & use their private packages without affecting other data scientists using the SQL server.


SQL R services package management uses the following per database roles to secure installation and usage for the installed packages on a per database level on a SQL server:


* 
 `rpkgs-users` - allows users to use shared packages installed by users belong to `rpkgs-shared` role

* 
 `rpkgs-private` - allows all permissions as `rpkgs-users` role and also allows users to install, remove and use private packages

* 
 `rpkgs-shared` - allows all permissions as `rpkgs-private` role and also allows users to install, remove shared packages

* 
 `db_owner` - allows all permissions as `rpkgs-shared` role and also allows users to install & remove shared and private packages for other users for management




**Enabling SQL R service package management on a SQL server**


By default, SQL R services package management is disabled on a SQL server instance and on a particular SQL database. To use this functionality, the administrator needs to do the following:


* 
 Enable package management on the SQL server instance. This only needs to be done once per SQL server instance.

* 
 Enable package management on the SQL database. This also only needs to be done once per SQL server database.



**`RegisterRExt.exe`** command line utility which ships with RevoScaleR package for SQL R Services allows administrators to enable package management feature at SQL server instance level and per database level. You can find RegisterRExt.exe at `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe`.


To enable SQL R services package management at instance level, open an elevated command prompt and use the following command:


* 
 **`RegisterRExt.exe /installpkgmgmt [/instance:name] [/user:username] [/password:*|password]`**



This command will create some package management related instance level artifacts on the SQL server machine.

After enabling it at SQL server instance level, to enable SQL R services package management at database level, open an elevated command prompt and use the following command:


* 
 **`RegisterRExt.exe /installpkgmgmt /database:databasename  [/instance:name] [/user:username] [/password:*|password]`**

This command will create some database artifacts, including `rpkgs-users`, `rpkgs-private` and `rpkgs-shared` database roles to control user permissions who can install, uninstall and use packages on the SQL database.







**Disabling SQL R service package management on a SQL server**

To disable SQL R service package management on a SQL server, the administrator needs to do the following:


* 
 Disable package management on the SQL database. This also only needs to be done once per SQL server database.

* 
 Disable package management on the SQL server instance. This only needs to be done once per SQL server instance.



**`RegisterRExt.exe`** command line utility which ships with RevoScaleR package for SQL R Services allows administrators to disable package management feature at SQL server instance level and per database level. You can find RegisterRExt.exe at `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe`.

To disable SQL R services package management at database level, open an elevated command prompt and use the following command:


* 
 **`RegisterRExt.exe /uninstallpkgmgmt /database:databasename  [/instance:name] [/user:username] [/password:*|password]`**

This command will remove the package management related database artifacts from the SQL database. It will also remove all the packages installed from database onto the secured file system location on SQL server.



After disabling package management at SQL database level, to disable SQL R services package management at instance level, open an elevated command prompt and use the following command:


* 
 **`RegisterRExt.exe /uninstallpkgmgmt [/instance:name] [/user:username] [/password:*|password]`**

This command will remove the package management related per instance artifacts from the SQL server machine. 



**Known Issues**:
The SQL R services package management is still in active development and not fully done. Here are some known issues:


* 
 When you **restore a database** on another SQL instance the packages in the database don't get automatically reinstalled  on secured file system location on SQL server automatically. This issue will be fixed in next release.

* 
 When you **drop a database** the packages installed on the secured file system location don't get removed automatically. This issue will be fixed in next release, till them please use `RegisterRExt.exe /uninstallpkgmgmt /database:databasename` command to remove the package management feature before dropping the database.


 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
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
 
 
 
 
 
 
 
