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

# Configure a Remote SQL Server or PostgreSQL Database

During the configuration of operationalization for R Server, a local SQLite database is automatically installed and configured for you. Later, you can update the configuration to use another database. This is particularly useful when you want to use a remote database or when you have multiple front-ends. 

The supported databases are:
+ SQL Server Professional, Standard, or Express Version 2008 or greater (Windows)
+ PostgreSQL 9.2 or greater (Linux) 

> [!Important]
> Any data that was saved in the default SQLite database will be lost if you configure a remote database.

<a name="sqlserver"></a>
<a name="postgresql"></a>

**To use a local or remote database instead of the default local SQLite database, do the following:**

> These steps assume that you have already set up SQL Server or PostgreSQL as described for that product.

1.  On **each front-end machine**, [stop the service](admin-utility.md#startstop).

1.  Update the database properties to point to the new database as follows:

    1. Open the `appsettings.json` file, which is the external configuration file. 
    
       You can find that file under `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

    1. Locate the `ConnectionStrings` property block.

    1. Replace **the entire contents** of the `ConnectionStrings` block with these for your authentication method:

       > [!Important]
       > For security purposes, we recommend you [encrypt login credentials](admin-utility.md#encrypt) for this database before adding the information to this file.

       For SQL Server Database (Integrated Security), use:
       ``` 
       "ConnectionStrings": {
          "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>\\<INSTANCE-NAME>;Initial Catalog=<DB-NAME>;Integrated Security=True;"
        },
        ```

        For SQL Server Database (SQL authentication), use: 
        ```
        "ConnectionStrings": {
           "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>\\<INSTANCE-NAME>;Initial Catalog=<DB-NAME>; Integrated Security=False; User Id=<USER-ID>;Password=<PASSWORD>;"
        },
        ```

        For PostgreSQL Database, use:
        ```
        "ConnectionStrings": {
           "postgresql": "User ID=<DB-USERNAME>;Password=<USER-PASSWORD>;Host=<DB-SERVER-IP-OR-FQDN>;Port=5432;Database=<DB-NAME>;Pooling=true;"
        },   
        ```

1. Open the database port on the remote machine to the public IP of each Dfront-end as described in these articles:
   + [For SQL Server](https://technet.microsoft.com/en-us/library/ms175043(v=sql.130).aspx)
   + [For PostgreSQL](https://www.postgresql.org/docs/current/static/auth-pg-hba-conf.html)
         
1. Launch the administrator's utility and:
   1. [Restart the front-end](admin-utility.md#startstop). The database is created for you when you restart.
   1. [Run the diagnostic tests](admin-utility.md#test).