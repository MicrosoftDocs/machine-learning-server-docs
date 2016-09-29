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

# Configuring R Server for Operationalization on Windows

After installing Microsoft R Server, you can configure the product for the operationalization of R analytics. 

## Setting up for a one-box configuration

With this configuration, everything runs on a single machine. This configuration is perfect for testing, proof-of-concepts, and small-scale prototyping, but is not appropriate for production usage. [Learn more.](configurations.md)

<a name="onebox"></a>

**To configure on a single machine:**

1. If R Server (Standalone) is not yet installed, install it now.

1. Run the configuration script called `onebox.exe` (on Windows) and `onebox.sh` (on Linux) to set up the front-end and back-end onto the same machine.

1. When prompted, provide the admin password for the built-in, local DeployR `administrator` account.  
  
1. After the script has ended, set the proper firewall rule to open the front-end port (9000) to the public IP of the DeployR server so that remote machines can access it. If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

1. Run a diagnostic test of the configuration. 
    + **On Windows**: 
        1. Launch the DeployR administrator utility script with administrator privileges:
           ```
           cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
           adminUtilities.bat
           ```       
    
        1. From the main menu, run the DeployR diagnostic tests.  If there are any issues, you must solve them before continuing. Consult the Troubleshooting section of this document for additional help or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

       >All output from the diagnostic test are stored in `C:\Program Files\Microsoft\DeployR-<VERSION>\deployr\tmp\logs\diagnostics.zip`.

    + **On Linux**:
        1. Launch the DeployR administrator utility script as `root` or a user with `sudo` permissions:
           ```
           cd $DEPLOYR_HOME/deployr/tools/ 
           ./adminUtilities.sh
           ```       
    
        1. From the main menu, run the DeployR diagnostic tests.  If there are any issues, you must solve them before continuing. Consult the Troubleshooting section of this document for additional help or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

        >All output from the diagnostic test are stored into `$DEPLOYR_HOME/deployr/tmp/logs/diagnostics.zip`.


You are now ready to begin operationalizating your R analytics with R Server.

<a name="enterpriseready"></a>

## Setting up for an enterprise-ready configuration

This type of configuration is highly recommended for production usage. It offers a scalable architecture, enterprise-grade security, and even a remote SQL or PostgreSQL database. [Learn more.](configurations.md)

You can configure one or more front-ends as needed. You can also scale your back-ends. A back-end can be configured on its own machine or on the same machine as the front-end.

>**Note**: If desired, you can run the front-end service from within IIS.

**To configure R Server for production usage:**

1. Determine the number of front-ends and back-ends you'll need in your configuration. 

1. For each front-end, configure it as follows: 

    1. On each machine, install R Server:

       + On Windows, install R Server (Standalone).

       + On Linux, install Microsoft R Server 

    1. Run the front-end configuration script:
       + On Windows, the script is called `frontend.exe`. 

       + On Linux, the script is called `frontend.sh`. 
    
    1. When prompted, provide the admin password for the built-in, local `administrator` account.   

    1. After the script has ended, set the proper firewall rule to open the front-end port (9000) to the public IP of the DeployR server so that remote machines can access it. If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

    1. To configure DeployR to use Active Directory/LDAP or Azure Active Directory authentication, follow [these steps](security-authentication.md).  We highly recommend this approach to authentication. 

    1. To enable HTTPS for DeployR, follow [these steps](security-https.md) and install the certificates needed to secure the communications between the client application and DeployR as well as between the front-ends and back-ends of DeployR. 

    >WHAT IF SSL IS ENABLED, DO WE NEED A DIFFERENT PORT LIKE WITH DID IN THE PAST?????@@@@@

    Your front-end is now configured. Repeat these steps for each front-end you want to add to the configuration.

1. For each back-end, configure it as follows: 

    1. On each machine, install R Server:
    
       + On Windows, install R Server (Standalone).

       + On Linux, install Microsoft R Server 

    1. Run the back-end configuration script:
       + On Windows, the script is called `backend.exe`. 

       + On Linux, the script is called `backend.sh`. 

    1. After the script has ended, configure the firewall.
    
        1. Set the proper firewall rule to open the back-end port (9002) to ONLY allow communication with the front-end(s) so that all back-ends can reach all front-ends. If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

        1. For added security, restrict the list of IPs that can access the machine.
  
    1. If you are enabling HTTPS, follow [these steps](security-https.md) and install the certificates needed on the back-end. 

    Your back-end is now configured. Repeat these steps for each back-end you want to add to the configuration.

1. Once all front-ends and back-ends are configured, declare the IP addresses of each back-end with each front-end using admin util or config file `appsettings.json`. @@@@HOW DO YOU DO THAT?@@ 

1. If desired, replace A [remote SQL Server or PostgreSQL database can be configured](configure-remote-database.md) in place of the default SQLite database. While it is optional for configurations with a single front-end, it is required whenever multiple front-ends are configured.

1. If you want to use a remote database or if you have multiple front-ends, configure DeployR to use one of the following databases instead:
    + On Windows, [configure SQL Server](configure-remote-database.md#sqlserver)
    + On Linux, [configure PostgreSQL](configure-remote-database.md#postgres)

1. Now that the configuration is complete, run the diagnostic tests on **every front-end machine**. 
    + **On Windows**: 
        1. Launch the DeployR administrator utility script with administrator privileges:
           ```
           cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
           adminUtilities.bat
           ```       
    
        1. From the main menu, run the DeployR diagnostic tests.  If there are any issues, you must solve them before continuing. Consult the Troubleshooting section of this document for additional help or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

       >All output from the diagnostic test are stored in `C:\Program Files\Microsoft\DeployR-<VERSION>\deployr\tmp\logs\diagnostics.zip`.

    + **On Linux**:
        1. Launch the DeployR administrator utility script as `root` or a user with `sudo` permissions:
           ```
           cd $DEPLOYR_HOME/deployr/tools/ 
           ./adminUtilities.sh
           ```       
    
        1. From the main menu, run the DeployR diagnostic tests.  If there are any issues, you must solve them before continuing. Consult the Troubleshooting section of this document for additional help or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

        >All output from the diagnostic test are stored into `$DEPLOYR_HOME/deployr/tmp/logs/diagnostics.zip`.