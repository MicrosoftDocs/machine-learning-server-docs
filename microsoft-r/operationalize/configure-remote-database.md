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

During the configuration of DeployR, a local SQLite database is automatically installed and configured for you. After configuring DeployR, you can update the configuration to use another database. This is particularly useful when you want to use a remote database or when you have multiple front-ends. The supported databases are:

> WHICH VERSIONS DO WE SUPPORT FOR THIS RELEASE??

+ **SQL Server Professional, Standard, or Express Version 2012 or greater** (on Windows)
+ **PostgreSQL 9.1 or greater** (on Linux) 

> Any data that was saved in the default SQLite database will be lost if you configure a remote database.

<a name="sqlserver"></a>

### Using a SQL Server Database

To use a local or remote SQL Server database for DeployR instead of the default local SQLite database, do the following.

> These steps assume that you have already set up SQL Server as described for that product.
>
> The database will be created for you when you restart the server.

On each front-end machine:

1.  [Stop the DeployR server](deployr-common-administration-tasks.md#startstop).
    > HOW DO WE DO THAT?

1.  Update the database properties to point to the new database as follows:

    1.  Open the `appsettings.json` file, which is the DeployR external configuration file.

    2.  Locate the `ConnectionStrings` property block.

    3.  Replace **the entire contents** of the `ConnectionStrings` block with these for your authentication method:

        Integrated SQL authentication:

        > DO WE HANDLE INTEGRATED SECURITY????? HOW DO WE SET INTEGRATED SECURITY? HOW DO YOU SPECIFY YOUR INSTANCE NAME?  

            "ConnectionStrings": {
                "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>;integratedSecurity=true;Initial Catalog=<DB-NAME>;",
            },
            
        Non-integrated authentication:
        > PASSWORD ENCRYPTION. USER TOO. In this case, the password here will need to be encrypted (at least recommended) thorough a tool that we are writing, using a certificate key. The tool will either be available to the admin or will be integrated into the admin utility. Still considering setting remote DB as an option in the admin utility.

            "ConnectionStrings": {
                "sqlserver": "Data Source=<DB-SERVER-IP-OR-FQDN>;User ID=<DB-USERNAME>;Password=<USER-PASSWORD>;Initial Catalog=<DB-NAME>;",
            },

1. Encrypt the remote database and/or LDAP/LDAP-S login credentials (username and password) using the administration utility as follows:
    1. Make sure a credential encryption certificate with a private key is installed on the front-end.  Remote database and/or LDAP login credentials (username and password) must be encrypted using the certificate.
    1. LAUNCH UTIL ON WIN AND Linux
    1. You will select an option in the admin util to start the encryption tool. You will enter the secret string (user and password). The tool will return an encrypted string that you will need to go and add to the configuration file. (basic implementation)
    1. You will select an option of configuring LDAP/DB in the admin util. The util will prompt for the user and passwords, will call the encryption tool internally and will update the config locally for you with the encrypted strings. (more user friendly but currently a P2)

1. [Open the database port on the remote machine to the public IP of each DeployR front-end](#firewall).  @@@@@@

1.  Restart the front-end and test the connection to the database as follows:
    > HOW DO WE DO THAT?

    1.  Launch the DeployR administrator utility script with administrator privileges:

    1.  From the main menu, choose the option **Start/Stop Server** to restart DeployR-related services.

    1.  Once the DeployR server has been successfully restarted, return the main menu.

    1.  From the main menu, choose option to run the [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

    1.  Exit the utility.


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
        > PASSWORD ENCRYPTION. USER TOO. In this case, the password here will need to be encrypted (at least recommended) thorough a tool that we are writing, using a certificate key. The tool will either be available to the admin or will be integrated into the admin utility. Still considering setting remote DB as an option in the admin utility.

            "ConnectionStrings": {
                "User ID=<DB-USERNAME>;Password=<USER-PASSWORD>;Host=<DB-SERVER-IP-OR-FQDN>;Port=5432;Database=<DB-NAME>;Pooling=true;",
            },            

1.  If you are connecting to a remote PostgreSQL database, be sure to [open the database port to the public IP of the DeployR server](#firewall).  @@@@

1.  Restart the front-end and test the connection to the database as follows:
    > HOW DO WE DO THAT?

    1.  Launch the DeployR administrator utility script with administrator privileges:

    1.  From the main menu, choose the option **Start/Stop Server** to restart DeployR-related services.

    1.  Once the DeployR server has been successfully restarted, return the main menu.

    1.  From the main menu, choose option to run the [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

    1.  Exit the utility.