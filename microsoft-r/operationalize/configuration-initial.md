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

# Configuring R Server for Operationalization

To operationalize your R analytics, you must configure Microsoft R Server after installation to enable the operationalization feature. 

The simplest configuration for this feature involves a single front-end and back-end, called a **one-box** configuration. These components are defined as follows:

+ A **front-end** acts as a http REST endpoint with which DeployR users can interact directly to make API calls. It can also access data in the database, and send jobs to be computed on the back-end. 

+ A **back-end** is used to execute R code as a session or service.

In addition to the one-box configuration, you can also install multiple components on multiple machines, which is referred to as an  **enterprise** configuration. 

<a name="onebox"></a>
## The Basic One-Box Configuration

With one-box configurations, as the name suggests, everything runs on a single machine and set-up is a breeze. This configuration includes an operationalization front-end and back-end on the same machine. It also relies on the default local SQLite database.

This configuration is useful when you want to explore what it is to operationalize R analytics using R Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but might not be appropriate for production usage. 

![One-box configuration](../media/o16n/setup-onebox.jpeg)

**To configure on a single machine:**

  1. On each machine, install Microsoft R Server:

     + On Windows, install [R Server (Standalone)](https://msdn.microsoft.com/en-us/library/mt671127.aspx). 
     + On Linux, install [Microsoft R Server](../rserver-install-linux-server.md).  

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.

1. Choose the option to **Configure DeployR**.

1. Choose the option to **Configure for one box** to set up the front-end and back-end onto the same machine.

1. @@When prompted, provide the admin password for the built-in, local DeployR `administrator` account.  
  
1. After the script has ended, set the proper firewall rule to open the front-end port (9000) to the public IP of the DeployR server so that remote machines can access it. 

   If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

1. [Run a diagnostic test of the configuration](admin-utility.md#test). 

You are now ready to begin operationalizating your R analytics with R Server.


<a name="enterprise"></a>
## The Enterprise Configuration

With an enterprise configuration, you can work with your production-grade data within a scalable, multi-machine setup, and benefit from enterprise-grade security. 

This configuration includes one or more front-ends and back-ends on a group of machines. These front-ends and back-ends can be scaled independently.  For added security, you can authenticate against [Active Directory (LDAP) or Azure Active Directory](security-authentication.md) and [configure SSL](security-https.md) for DeployR.

Additionally, when you have multiple front-ends, you must set up a [remote SQL Server or PostgreSQL database](configure-remote-database.md) so that data can be shared across front-end services.
 
![Enterprise Configuration](../media/o16n/setup-enterprise-ready.jpeg)


**Step 1: Configure Front-end(s)**

>**Note:** It is possible to run the DeployR front-end service from within IIS.

  1. On each machine, install Microsoft R Server:

     + On Windows, install [R Server (Standalone)](https://msdn.microsoft.com/en-us/library/mt671127.aspx). 
     + On Linux, install [Microsoft R Server](../rserver-install-linux-server.md).  

  1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges and:

     1. From the main menu, choose the option to **Configure DeployR**.
     1. From the sub-menu, choose the option to **Configure a front-end**.     
     1. Follow the onscreen prompts.  
     1. Return to the main menu and choose the option to [Set a local administrator password](admin-utility.md#admin-password).
     1. Set the password for that account. 
        You can always configure DeployR to authenticate against  [Active Directory (LDAP) or Azure Active Directory](security-authentication.md) later.

  1. Choose the option to open the BLAH BLAH BLAH @@ port:

  1. In your firewall, open the front-end port (9000) to the public IP of the DeployR server so that remote machines can access it.
    
     If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

Your front-end is now configured. Repeat these steps for each front-end you want to add to the configuration.

**Step 2: Configure Back-end(s)**

>**Note:** A back-end can be configured on its own machine or on the same machine as the front-end.

1. On each machine, install the same R Server version you installed on the front-end.

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges and:

   1. From the main menu, choose the option to **Configure DeployR**.
   1. From the sub-menu, choose the option to **Configure a back-end**.
   1. Follow the onscreen prompts.

1. After the script has ended, configure the firewall.
    
   1. Set the proper firewall rule to open the back-end port (9002) to ONLY allow communication with the front-end(s) so that all back-ends can reach all front-ends. 

      If using the IPTABLES firewall or equivalent service on Linux, use the `iptables` command (or the equivalent) to open the port.

   1. For added security, restrict the list of IPs that can access the machine.
  
Your back-end is now configured. Repeat these steps for each back-end you want to add to the configuration.

**Step 3: Configure Enterprise-Grade Security**

In production environments, we strongly recommend the following approaches:

1. [Configure SSL](security-https.md) for DeployR and install the necessary certificates. 

1. Authenticate DeployR against [Active Directory (LDAP) or Azure Active Directory](security-authentication.md).  

**Step 4: Configure a Remote Database**

By default, the front-end configuration sets up a SQLite database. If you have multiple front-ends or simply want to use a remote database, follow these instructions to [configure remote database](configure-remote-database.md) (SQL Server or PostgreSQL) so that data can be shared across front-end services.

**Step 5: Provisioning DeployR on the Cloud**

If provisioning DeployR on a cloud service, then you must also [create inbound security rules for port BLAH BLAH BLAH @@ in Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console.

> @@WHAT MORE DO WE NEED TO SAY ABOUT CLOUD SETUPS?  

**Step 6: Post Configuration**

1. Once all front-ends and back-ends are configured, declare the IP addresses of each back-end with each front-end using admin util or config file `appsettings.json`. 
   > @@HOW DO YOU DO THAT? DO WE NEED THIS STEP?@@ 

1. Verify the configuration by running [diagnostic test](admin-utility.md#test) on each front-end machine. 