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

During the configuration of DeployR, a local SQLite database is automatically installed and configured for you. After configuring DeployR, you can update the configuration to use another database. This is particularly useful when you want to use a remote database or when you have multiple front-ends. 

The supported databases are:
+ **SQL Server Professional, Standard, or Express Version 2008 or greater** (on Windows)
+ **PostgreSQL 9.2 or greater** (on Linux) 

> Any data that was saved in the default SQLite database will be lost if you configure a remote database.

<a name="sqlserver"></a>

### Using a SQL Server Database

To use a local or remote SQL Server database for DeployR instead of the default local SQLite database, do the following.

> These steps assume that you have already set up SQL Server as described for that product.
>
> The database will be created for you when you restart the server.

On each front-end machine:

1.  [Stop the DeployR server](admin-utility.md#startstop).

1.  Update the database properties to point to the new database as follows:

    1.  Open the `appsettings.json` file, which is the DeployR external configuration file.

    2.  Locate the `ConnectionStrings` property block.

    3.  Replace **the entire contents** of the `ConnectionStrings` block with these for your authentication method:
        **Integrated Security**:
        ``` 
        "ConnectionStrings": {
           "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>\\<INSTANCE-NAME>;Initial Catalog=<DB-NAME>;Integrated Security=True;",
        },
        ```

        **SQL authentication**: 
        ```
        "ConnectionStrings": {
           "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>\\<INSTANCE-NAME>;Initial Catalog=<DB-NAME>; Integrated Security=False; User Id=<USER-ID>;Password=<PASSWORD>;",
         },
         ```

         >For security purposes, we strongly recommend that you [encrypt database credentials](admin-utility.md#encrypt) for the remote database before adding that information to this file as follows:
         >1. Make sure a credential encryption certificate with a private key is installed on the front-end. 
         >2. [Generate an encrypted credential string](admin-utility.md#encrypt) using the Administrator Utility.
         >3. Return to `appsettings.json` and insert this string into the database connection string.
         
> HOW DO WE OPEN THIS PORT AND WHAT PORT IS THAT? @@@@

1. [Open the database port on the remote machine to the public IP of each DeployR front-end](#firewall).  @@@@@@ 

1. Launch the administrator's utility and [restart the front-end](admin-utility.md#startstop).

1. In the same utility, run the [DeployR diagnostic tests](admin-diagnostics-troubleshooting.md).


<a name="postgresql"></a>
### Use a PostgreSQL Database

To use a local or remote PostgreSQL database for DeployR instead of the default local SQLite database, you'll need to:

> WHAT VERSION OF POSTGRESQL ARE WE SUPPORTING?


> These steps assume that you have already set up PostgreSQL as described for that product.
>
> The database will be created for you when you restart the server.

1.  [Stop the DeployR server](deployr-common-administration-tasks.md#startstop).

1.  Update the database properties to point to the new database as follows:

    1.  Open the `appsettings.json` file, which is the DeployR external configuration file.

    2.  Locate the `ConnectionStrings` property block.

    3.  Replace **the entire contents** of the `ConnectionStrings` block with these for your authentication method:

> HOW DO WE ENCRYPT SECRETS??

1. @@@@@@@ Encrypt the remote database (and/or LDAP/LDAP-S login) credentials (username and password) using the @@@@@@administration utility as follows:
         > PASSWORD ENCRYPTION. USER TOO. In this case, the password here will need to be encrypted (at least recommended) thorough a tool that we are writing, using a certificate key. The tool will either be available to the admin or will be integrated into the admin utility. Still considering setting remote DB as an option in the admin utility.

            "ConnectionStrings": {
                "User ID=<DB-USERNAME>;Password=<USER-PASSWORD>;Host=<DB-SERVER-IP-OR-FQDN>;Port=5432;Database=<DB-NAME>;Pooling=true;",
            },            

> HOW DO WE OPEN THIS PORT AND WHAT PORT IS THAT? @@@@

1. [Open the database port on the remote machine to the public IP of each DeployR front-end](#firewall).  @@@@@@ 

1. Launch the administrator's utility and [restart the front-end](admin-utility.md#startstop).

1. In the same utility, run the [DeployR diagnostic tests](admin-diagnostics-troubleshooting.md).