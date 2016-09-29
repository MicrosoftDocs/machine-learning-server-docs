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

# Configure SQL Server or PostgreSQL database for DeployR.

By default, DeployR is configured to use a SQLite database out of the box. If you want to use a remote database or if you have multiple front-ends, you can configure DeployR to use the following databases instead:

+ SQL Server (on Windows)
+ PostgreSQL (on Linux) 

<a name="sqlserver"></a>

### Using a SQL Server Database

During the configuration of DeployR, a local SQLite database is automatically installed and configured for you. After configuring DeployR, **but before using it**, you can configure DeployR to use a database in **SQL Server Professional, Standard, or Express Version 2012 or greater**.

If you want to use a local or remote SQL Server database for DeployR instead of the default local H2 database, you'll need to:

1. Install and configure SQL Server as described for that product.

1. Log into the SQL Server Management Studio.

1. Create a database with the name `deployr` and an instance called `DEPLOYREXPRESS`. For help creating that database, visit: https://technet.microsoft.com/en-us/library/ms186312(v=sql.130).aspx

    >The JDBC drivers are installed with DeployR. 

1.  If you are using Windows Authentication for login:

    1. In SQL Server Management Studio and grant permissions to the user `NT SERVICE\Apache-Tomcat-for-DeployR-<X.Y.Z._VERSION_NUMBER>`.
    
    1.  In the Object Explorer pane, right click **Security &gt; Logins**.
    
        ![Login](../../media/deployr-install-on-windows/sqlserver-new-login.png)

    1.  In the **Login - New** dialog, enter `NT SERVICE\Apache-Tomcat-for-DeployR-<X.Y.Z._VERSION_NUMBER>`, where `<X.Y.Z._VERSION_NUMBER>` is the three digit number DeployR version number, such as `NT SERVICE\Apache-Tomcat-for-DeployR-8.0.5` into the **Login name** field.
    
        ![Login](../../media/deployr-install-on-windows/sqlserver-login-dialog.png)

    1.  Choose the **Server Roles** page on the left and select the checkboxes for `public`.
    
    6.  Choose the **User Mapping** page on the left and select the checkbox for the database for DeployR, which in our example is called `deployr` and for the Database role member, select `db_owner` and `public`.
    
    7.  Choose the **Status** page on the left and **Grant** permission to connect to database engine and choose **Enabled** login.
    
    8.  Click **OK** to create the new login.

5.  [Stop the DeployR server](deployr-common-administration-tasks.md#startstop).

6.  Update the database properties to point to the new database as follows:

    1.  Open the `$DEPLOYR_HOME\deployr\deployr.groovy` file, which is the DeployR server external configuration file.

    2.  Locate the `dataSource` property block.

    3.  Replace **the entire contents** of the `dataSource` block with these for your authentication method:

        For Windows authentication:

              dataSource {
                   dbCreate = "update"
                   driverClassName = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
                   url = "jdbc:sqlserver://localhost;instanceName=<PUT_INSTANCE_NAME_HERE>;database=deployr;integratedSecurity=true;"
              }

        For basic authentication:

              dataSource {
                   dbCreate = "update"
                   driverClassName = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
                   username = “<PUT_DB_USERNAME_HERE>”
                   password = “<PUT_DB_PASSWORD_HERE>”
                   url = "jdbc:sqlserver://localhost;instanceName=<PUT_INSTANCE_NAME_HERE>;database=deployr;"
              }

        >If you are unsure of your SQL instance name, please consult with your SQL administrator. The default instance name for:
        >    -   SQL Server Express is `SQLEXPRESS`
        >    -   SQL Server Standard or Professional is `MSSQLEXPRESS`
        >
        >If you are using a remote database, use the IP address or FQDN of the remote machine rather than `localhost`.

7.  If you are connecting to a remote SQL Server database, be sure to [open the database port to the public IP of the DeployR server](#firewall).  @@@@@@

8.  Test the connection to the database and restart the server as follows:

    1.  Launch the DeployR administrator utility script with administrator privileges:

            cd $DEPLOYR_HOME\deployr\tools\ 
            adminUtilities.bat

    2.  From the main menu, choose the option **Test Database Connection**.

        -   If there are any issues, you must solve them before continuing.

        -   Once the connection test passes, return the main menu.

    3.  From the main menu, choose the option **Start/Stop Server** to restart DeployR-related services.

    4.  Once the DeployR server has been successfully restarted, return the main menu.

    5.  From the main menu, choose option to run the [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

    6.  Exit the utility.



<a name="postgresql"></a>
### Use a PostgreSQL Database

During the configuration of DeployR, a local SQLite database is automatically installed and configured for you. After configuring DeployR, **but before using it**, you can configure DeployR to use a database in **PostgreSQL 9.1 or greater.**

If you want to use a local or remote PostgreSQL database for DeployR instead of the default local H2 database, you'll need to:

1.  Install and configure PostgreSQL as described for that product.

2.  Create a database with the name `deployr`. Use the owner of this database as the username in the `dataSource` block in a later step.

3.  Assign the proper permissions to the database user to read and write into the database.

4.  [Download the JDBC42 Postgresql Driver for JDK 1.8](https://jdbc.postgresql.org/download.html) for the version of the database you installed and copy them under **both** of the following folders:

    -   `$DEPLOYR_HOME/tomcat/tomcat7/lib`

    -   `$DEPLOYR_HOME/deployr/tools/lib`

5.  [Stop the DeployR server](deployr-common-administration-tasks.md#startstop).

6.  Update the database properties to point to the new database as follows:

    1.  Open the `$DEPLOYR_HOME\deployr\deployr.groovy` file, which is the DeployR server external configuration file.

    2.  Locate the `dataSource` property block.

    3.  Replace the contents of that block with these properties and specify the appropriate port number, username, and password:

            dataSource {
              dbCreate = "update"
              driverClassName = "org.postgresql.Driver"
              url = "jdbc:postgresql://localhost:<PUT_PORT_HERE>/deployr"
              pooled = true
              username = "<PUT_DB_USERNAME_HERE>"
              password = "<PUT_DB_PASSWORD_HERE>"
              }

	>Put the owner of the `deployr` database as the username. For more information, see <http://www.postgresql.org/docs/9.1/static/sql-alterdatabase.html>.
        >
        >If you are using a remote database, use the IP address or FQDN of the remote machine rather than `localhost`.

7.  If you are connecting to a remote PostgreSQL database, be sure to [open the database port to the public IP of the DeployR server](#firewall).  @@@@

8.  Test the connection to the database and restart the server as follows:

    1.  Launch the DeployR administrator utility script as `root` or a user with `sudo` permissions:

            cd $DEPLOYR_HOME/deployr/tools/ 
            sudo ./adminUtilities.sh

    2.  From the main menu, choose the option **Test Database Connection**.

        -   If there are any issues, you must solve them before continuing.

        -   Once the connection test passes, return the main menu.

    3.  From the main menu, choose the option **Start/Stop Server** to restart DeployR-related services.

    4.  Once the DeployR server has been successfully restarted, return the main menu.

    5.  From the main menu, choose option to run the [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

    6.  Exit the utility.
