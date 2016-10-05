---

# required metadata
title: "Operationalization with R Server"
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

# Configure SQL Server or PostgreSQL database for DeployR

During the configuration of DeployR, a local SQLite database is automatically installed and configured for you. After configuring DeployR, **but before using it**, you can configure DeployR to use another database. This is particularly useful when you want to use a remote database or when you have multiple front-ends. The supported databases are:

> WHICH VERSIONS DO WE SUPPORT FOR THIS RELEASE??

+ **SQL Server Professional, Standard, or Express Version 2012 or greater** (on Windows)
+ **PostgreSQL 9.1 or greater** (on Linux) 

<a name="sqlserver"></a>

### Using a SQL Server Database

To use a local or remote SQL Server database for DeployR instead of the default local SQLite database, you'll need to:

> These steps assume that you have already set up SQL Server as described for that product.
>
> The database will be created for you when you restart the server.

1.  [Stop the DeployR server](deployr-common-administration-tasks.md#startstop).

1.  Update the database properties to point to the new database as follows:

    1.  Open the `appsettings.json` file, which is the DeployR external configuration file.

    2.  Locate the `ConnectionStrings` property block.

    3.  Replace **the entire contents** of the `ConnectionStrings` block with these for your authentication method:

        For Windows authentication:

        > HOW DO WE SET INTEGRATED SECURITY? HOW DO YOU SPECIFY YOUR INSTANCE NAME?  

            "ConnectionStrings": {
                "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>;integratedSecurity=true;Initial Catalog=<DB-NAME>;",
            },
            
        For basic authentication:

            "ConnectionStrings": {
                "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>;User ID=<DB-USERNAME>;Password=<USER-PASSWORD>;Initial Catalog=<DB-NAME>;",
            },

7.  If you are connecting to a remote SQL Server database, be sure to [open the database port to the public IP of each DeployR front-end](#firewall).  @@@@@@

8.  Test the connection to the database and restart the server as follows:

    1.  Launch the DeployR administrator utility script with administrator privileges:

        > HOW DO WE DO THAT?

    2.  From the main menu, choose the option **Test Database Connection**.

        -   If there are any issues, you must solve them before continuing.

        -   Once the connection test passes, return the main menu.

        > CAN WE STILL TEST THE CONNECTION STRING IN THE ADMIN UTIL WITHOUT HAVING TO RESTART THE SERVER??

    3.  From the main menu, choose the option **Start/Stop Server** to restart DeployR-related services.

    4.  Once the DeployR server has been successfully restarted, return the main menu.

    5.  From the main menu, choose option to run the [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

    6.  Exit the utility.



<a name="postgresql"></a>
### Use a PostgreSQL Database

During the configuration of DeployR, a local SQLite database is automatically installed and configured for you. After configuring DeployR, **but before using it**, you can configure DeployR to use a database in **PostgreSQL 9.1 or greater.**

> WHAT VERSION OF POSTGRESQL ARE WE SUPPORTING?

If you want to use a local or remote PostgreSQL database for DeployR instead of the default local SQLite database, you'll need to:

1.  Install and configure PostgreSQL as described for that product.

    > A DATABASE WILL BE CREATED FOR YOU

1.  [Stop the DeployR server](deployr-common-administration-tasks.md#startstop).

1.  Update the database properties to point to the new database as follows:

    1.  Open the `appsettings.json` file, which is the DeployR external configuration file.

    2.  Locate the `ConnectionStrings` property block.

    3.  Replace **the entire contents** of the `ConnectionStrings` block with these for your authentication method:

            "ConnectionStrings": {
                "User ID=<DB-USERNAME>;Password=<USER-PASSWORD>;Host=<DB-SERVER-IP-OR-FQDN>;Port=5432;Database=<DB-NAME>;Pooling=true;",
            },
            
        >If you are using a remote database, use the IP address or FQDN of the remote machine rather than `localhost`.

7.  If you are connecting to a remote PostgreSQL database, be sure to [open the database port to the public IP of the DeployR server](#firewall).  @@@@

8.  Test the connection to the database and restart the server as follows:

    1.  Launch the DeployR administrator utility script with administrator privileges:

        > HOW DO WE DO THAT?

    2.  From the main menu, choose the option **Test Database Connection**.

        -   If there are any issues, you must solve them before continuing.

        -   Once the connection test passes, return the main menu.

        > CAN WE STILL TEST THE CONNECTION STRING IN THE ADMIN UTIL WITHOUT HAVING TO RESTART THE SERVER??

    3.  From the main menu, choose the option **Start/Stop Server** to restart DeployR-related services.

    4.  Once the DeployR server has been successfully restarted, return the main menu.

    5.  From the main menu, choose option to run the [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

    6.  Exit the utility.