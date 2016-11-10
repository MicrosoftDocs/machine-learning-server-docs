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

With this configuration, everything runs on a single machine. This configuration is perfect for testing, proof-of-concepts, and small-scale prototyping, but is not appropriate for production usage. [Learn more.](configuration-scenarios.md)

<a name="onebox"></a>

**To configure on a single machine:**

1. If R Server (Standalone) is not yet installed, install it now.

1. [Launch the DeployR administrator utility script](admin-utility.md#launch).

1. Choose the option to @@**Configure DeployR**.

1. Choose the option to @@**Configure onebox** to set up the front-end and back-end onto the same machine.

1. When prompted, provide the admin password for the built-in, local DeployR `administrator` account.  
  
1. After the script has ended, set the proper firewall rule to open the front-end port (9000) to the public IP of the DeployR server so that remote machines can access it. If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

1. [Run a diagnostic test of the configuration](diagnostics-troubleshooting.md). 

You are now ready to begin operationalizating your R analytics with R Server.

<a name="enterprise"></a>

## Setting up for an enterprise configuration

This type of configuration is highly recommended for production usage. It offers a scalable architecture, enterprise-grade security, and even a remote SQL or PostgreSQL database. [Learn more.](configuration-scenarios.md)

You can configure one or more front-ends as needed and scale your back-ends as well. A back-end can be configured on its own machine or on the same machine as the front-end.

>**Note**: If desired, you can run the front-end service from within IIS.

**To configure R Server for production usage:**

1. Determine the number of front-ends and back-ends you'll need in your configuration. 

1. For each front-end, configure it as follows: 

    1. On each machine, install R Server:

       + On Windows, install R Server (Standalone).  @@ADD LINKS

       + On Linux, install Microsoft R Server.  @@ADD LINKS 

    1. [Launch the DeployR administrator utility script](admin-utility.md#launch) and:

       1. Choose the option to @@ **Configure DeployR**.

       1. Choose the option to @@ **Configure Frontend only** to set up the front-end.
    
       1. Choose the option to [set the admin password](admin-utility.md#admin-password) for the built-in, local `administrator` account.   

       1. Choose the option to open the BLAH BLAH BLAH @@ port:

    1. In your firewall, open the front-end port (9000) to the public IP of the DeployR server so that remote machines can access it.
    
       If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

    Your front-end is now configured. Repeat these steps for each front-end you want to add to the configuration.

1. For each back-end, configure it as follows: 

    1. On each machine, install the same R Server you installed on the front-end.

    1. [Launch the DeployR administrator utility script](admin-utility.md#launch) and:

       1. Choose the option to @@ **Configure DeployR**.

       1. Choose the option to @@ **Configure Backend only** to set up the back-end.

    1. After the script has ended, configure the firewall.
    
        1. Set the proper firewall rule to open the back-end port (9002) to ONLY allow communication with the front-end(s) so that all back-ends can reach all front-ends. If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

        1. For added security, restrict the list of IPs that can access the machine.
  
    Your back-end is now configured. Repeat these steps for each back-end you want to add to the configuration.

 1. Consider these additional configuration options:
    
       1. [Configure DeployR for use with Active Directory/LDAP or Azure Active Directory authentication](security-authentication.md).  We highly recommend this approach to authentication. 

       1. [Configure SSL for DeployR](security-https.md) and install the necessary certificates. 

       1. If provisioning DeployR on a cloud service, then you must also [create inbound security rules for port BLAH BLAH BLAH @@ in Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console.

          > WHAT MORE DO WE NEED TO SAY ABOUT CLOUD SETUPS?  

       1. If you want to use a remote database or if you have multiple front-ends, [configure DeployR to use a SQL Server database or a PostgreSQL database](configure-remote-database.md) instead of the local SQLite database.

1. Once all front-ends and back-ends are configured, declare the IP addresses of each back-end with each front-end using admin util or config file `appsettings.json`. @@HOW DO YOU DO THAT?@@ 

1. Once you are done configuring DeployR, [run a diagnostic test of the configuration](diagnostics-troubleshooting.md) on **each front-end machine**. 