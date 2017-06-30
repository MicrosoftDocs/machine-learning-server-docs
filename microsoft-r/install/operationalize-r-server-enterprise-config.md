---

# required metadata
title: "Configure R Server to operationalize analytics (enterprise) - Microsoft R Server | Microsoft Docs"
description: "Configure Operationalization for Microsoft R Server, load balancer, "
keywords: "setup r server for deployment; install r server for deploying"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "6/19/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
  - deployr
  - r-server
#ms.custom: ""
---

# Configuring R Server to operationalize analytics (Enterprise Configuration)

**Applies to:  Microsoft R Server 9.x**

This article explains how to configure R Server after installation to act as a deployment server and to host analytic web services for operationalization. This article describes how to perform an enterprise configuration of these features. 

With an enterprise configuration, you can work with your production-grade data within a scalable, multi-machine setup, and benefit from enterprise-grade security.

## Enterprise architecture

This configuration includes one or more web nodes, one or more compute nodes, and a database.   

+ Web nodes act as HTTP REST endpoints with which users can interact directly to make API calls. These nodes also access the data in the database and send requests to the compute node for processing. Web nodes are stateless, and therefore, session persistence ("stickiness") is not required.

+ Compute nodes are used to execute R code as a session or service. Each compute node has its own [pool of R shells](../operationalize/configure-evaluate-capacity.md#r-shell-pool). Scaling up compute nodes enables you to have more R execution shells and benefit from load balancing across these compute nodes. 

+ The database. While an SQLite 3.7+ database is installed by default, we strongly recommend that you set up a [SQL Server (Windows) or PostgreSQL (Linux)](../operationalize/configure-remote-database-to-operationalize.md) database instead.

In an enterprise configuration, these nodes can be scaled independently. Scaling up web nodes enables an active-active configuration that allows you to load balance the incoming API requests.  Additionally, with multiple web nodes, you must use a [SQL Server or PostgreSQL database](../operationalize/configure-remote-database-to-operationalize.md) to share data and web services across web node services.   

For added security, you can [configure SSL](../operationalize/configure-https.md) and authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md).

Another configuration, referred to as "one-box", consists of a single web node and a single compute node installed on the same machine. Learn more about this configuration, [here](operationalize-r-server-one-box-config.md). 

![Enterprise Configuration](./media/operationalize-r-server-enterprise-config/configure-enterprise.png)


## Supported platforms

The web nodes and compute nodes are supported on:
- Windows Server 2012 R2, Windows Server 2016
- Ubuntu 14.04, Ubuntu 16.04,
- CentOS/RHEL 7.x

## How to upgrade 

To replace an older version, you can uninstall the older distribution before installing the new version (there is no in-place upgrade). **Carefully review the following steps:** 

### Upgrade a compute node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall Microsoft R Server 9.0 using the instructions in the article [Uninstall Microsoft R Server to upgrade to a newer version](r-server-install-uninstall-upgrade.md). The uninstall process stashes away a copy of your 9.0 configuration files under this directory so you can seamlessly upgrade to R Server 9.1 in the next step:
   + On Windows: `C:\Users\Default\AppData\Local\DeployR\current`

   + On Linux: `/etc/deployr/current`

1. Install Microsoft R Server:
   + On Windows: follow these instructions [Installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)
     >[!IMPORTANT]
     >For SQL Server Machine Learning Services, you must also:
     >1. Manually install .NET Core 1.1.
     >1. Add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder (for example, `C:\Program Files\Microsoft SQL Server\140`).

   + On Linux: follow these instructions [Installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges. The utility checks to see if any 9.0 configuration files are present under the `current` folder mentioned previously.

1. From the main menu, choose the option to **Configure R Server for Operationalization**.

1. From the submenu, choose the option to **Configure a compute node**.

1. When the script asks you if you'd like to upgrade, enter `y`. The node is automatically set up using the configuration you had for R Server 9.0. 
   The compute node is now configured. 

1. Repeat these steps for each compute node.

<a name="upgradewebnode"></a>

### Upgrade a web node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall Microsoft R Server 9.0 using the instructions in the article [Uninstall Microsoft R Server to upgrade to a newer version](r-server-install-uninstall-upgrade.md). The uninstall process stashes away a copy of your 9.0 configuration files under this directory so you can seamlessly upgrade to R Server 9.1 in the next step:
   + On Windows: `C:\Users\Default\AppData\Local\DeployR\current`

   + On Linux: `/etc/deployr/current`

1. Install Microsoft R Server:
   + On Windows: follow these instructions [Installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)

     >[!IMPORTANT]
     >For SQL Server Machine Learning Services, manually install .NET Core 1.1 and add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder, such as `C:\Program Files\Microsoft SQL Server\140`.

   + On Linux: follow these instructions [Installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges. The utility checks to see if any 9.0 configuration files are present under the `current` folder mentioned previously.

1. From the main menu, choose the option to **Configure R Server for Operationalization**.

1. From the submenu, choose the option to **Configure a web node**.     

1. When the script asks you if you'd like to upgrade, enter `y`. The node is automatically set up using the configuration you had for R Server 9.0. 
   Note: You can safely ignore the Python warning during upgrade.

1. From the main menu, choose the option to **Run Diagnostic Tests** to [test the configuration](../operationalize/configure-run-diagnostics.md).

1. Exit the utility. Your web node is now configured. 

1. Repeat these steps for each web node.



## How to configure for the enterprise

### 1. Configure a database

By default, the web node configuration sets up a local SQLite database. We strongly recommend that you use a SQL Server or PostgreSQL database for this configuration to achieve higher availability. In fact, you cannot use SQLite database at all if you have multiple web nodes or need a remote database. 

To configure that database, [follow these instructions](../operationalize/configure-remote-database-to-operationalize.md).

If you intend to configure multiple web nodes, then you **must** set up a [SQL Server or PostgreSQL database](../operationalize/configure-remote-database-to-operationalize.md) so that data can be shared across web node services.

>[!WARNING] 
>Choose and configure your database now. If you configure a different database later, all data in your current database is lost.

<a name="add-compute-nodes"></a>

### 2. Configure compute nodes

>[!Note]
>Side-by-side installations of R Server web nodes and compute nodes are not supported.

In an enterprise configuration, you can set up one or more compute nodes. 

>[!IMPORTANT]
>We highly recommend that you configure each node (compute or web) on its own machine for higher availability. 

1. Install Microsoft R Server and its dependencies:
   <br>
   **On Windows**

   Follow these instructions: [R Server installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)
   
   >[!IMPORTANT]
   >For SQL Server Machine Learning Services, you must manually install .NET Core 1.1 and add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder, such as `C:\Program Files\Microsoft SQL Server\140`.

   <br>
   **On Linux**

   Follow these instructions: [R Server installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)
      
   Additional dependencies for R Server on Linux 9.0.1. If you have installed R Server 9.0.1 on Linux, you must add a few symlinks:

   |R Server 9.0.1<br>Linux|CentOS 7.x|Ubuntu 14.04|Ubuntu 16.04|
   |--|----------------------|------------|------------|
   |Symlinks|<small>cd /usr/lib64<br>sudo ln -s libpcre.so.1   libpcre.so.0<br>sudo ln -s libicui18n.so.50   libicui18n.so.36<br>sudo ln -s libicuuc.so.50 libicuuc.so.36<br>sudo ln -s libicudata.so.50 libicudata.so.36<br><br><br></small>|<small>sudo apt-get install libicu-dev<br>cd /lib/x86_64-linux-gnu<br>ln -s libpcre.so.3 libpcre.so.0<br>ln -s liblzma.so.5 liblzma.so.0<br><br>cd /usr/lib/x86_64-linux-gnu<br>ln -s libicui18n.so.52 libicui18n.so.36<br>ln -s libicuuc.so.52 libicuuc.so.36<br>ln -s libicudata.so.52 libicudata.so.36</small>|<small>cd /lib/x86_64-linux-gnu<br>ln -s libpcre.so.3 libpcre.so.0<br>ln -s liblzma.so.5 liblzma.so.0<br><br>cd /usr/lib/x86_64-linux-gnu<br>ln -s libicui18n.so.55 libicui18n.so.36<br>ln -s libicuuc.so.55 libicuuc.so.36<br>ln -s libicudata.so.55 libicudata.so.36</small>|

   >**Note:** If there are issues with starting the compute node, see [here](../operationalize/configure-run-diagnostics.md).

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges. 

    >[!NOTE]
    >You can bypass the interactive configuration steps of the node using the argument `-silentcomputenodeinstall` when launching the administration utility. If you choose this method, you can skip the next two steps. For R Server 9.1 on Windows, for example, the syntax might be: 
    `dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll -silentcomputenodeinstall`. Learn about all command line switches for this script, [here](../operationalize/configure-use-admin-utility.md#switch).
    
1. From the main menu, choose the option to **Configure R Server for Operationalization**.

1. From the submenu, choose the option to **Configure a compute node**.

1. When the configuration  utility is finished, open port 12805: 
   + On Windows: Add an exception to your firewall to open port 12805. And, for additional security, you can also restrict communication for a private network or domain using a profile.

   + On Linux: If using IPTABLES or equivalent firewall service on Linux, then open port 12805 using `iptables`  or the equivalent command.

1. From the main utility menu, choose the option **Stop and start services** and restart the compute node to define it as a service.


The compute node is now configured. Repeat these steps for each compute node you want to add.


<a name="webnode"></a>

### 3. Configure web nodes

In an enterprise configuration, you can set up one or more web nodes. Note that it is possible to run the web node service from within IIS.

>[!IMPORTANT]
>We highly recommend that you configure each node (compute or web) on its own machine for higher availability. 

1. On each machine, install the same R Server version you installed on the compute node.<br> 

   + On Windows: follow these instructions [Installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)
     >[!IMPORTANT]
     >For SQL Server Machine Learning Services, you must also:
     >1. Manually install .NET Core 1.1.
     >1. Add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder (for example, `C:\Program Files\Microsoft SQL Server\140`).

   + On Linux: follow these instructions [Installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)

1. Declare the IP addresses of every compute node with each web node.
   1. [Open the appsettings.json configuration file](../operationalize/configure-find-admin-configuration-file.md).

   1. In the file, search for the section starting with `"BackEndConfiguration": {` .

   1. Update the `"Uris": {` properties to declare each compute node. 
   
      + In R Server 9.1, you must specify the Uri for each compute node individually using the `Values` property and/or specify port ranges (or IP octets) using the `Ranges` property.

        For example, both of the following snippets result in the same specification of four compute nodes:
        ```
        "Uris": {
           "Values": [
             “http://10.1.1.1:12805”, 
             “http://10.0.0.1:12805”, 
             “http://10.0.0.2:12805”, 
             “http://10.0.0.3:12805”
           ]
        }
        ```

        ```
        "Uris": {
           "Values": [“http://10.0.0.1:12805”],
           "Ranges": [“http://10.0.0.1-3:12805”]
        }
        ```
   
      + In R Server 9.0, you must specify the Uri for each compute node individually using the `Values` property. For example, this snippet results in four compute nodes:
        ```
        "Uris": {
           "Values": [
             “http://10.1.1.1:12805”, 
             “http://10.0.0.1:12805”, 
             “http://10.0.0.2:12805”, 
             “http://10.0.0.3:12805”
           ]
        }
        ```
 
      > Do not update any other properties in this file at this point. These other properties are updated during the compute node configuration.

   1. Close and save the file.

   1. Repeat these steps on each web node to declare each compute node.

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges:

    >[!NOTE]
    >You can bypass the interactive configuration steps of the node using the argument `-silentwebnodeinstall` and by defining a password for [the local 'admin' account](../deployr/../operationalize/configure-authentication.md#local) when you launch the administration utility. If you choose this method, you can skip the next three steps. For R Server 9.1 on Windows, for example, the syntax might be: 
    `dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll -silentwebnodeinstall my-password`.  Learn about all command line switches for this script, [here](../operationalize/configure-use-admin-utility.md#switch).

   1. From the main menu, choose the option to **Configure R Server for Operationalization**.

   1. From the submenu, choose the option to **Configure a web node**.     

   1. When prompted, provide a password for the built-in, local operationalization administrator account called 'admin'.
        Later, you can configure R Server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-authentication.md#local).

   1. From the main menu, choose the option to **Run Diagnostic Tests**. Verify the configuration by running [diagnostic test](../operationalize/configure-run-diagnostics.md) on each web node.

   1. From the main utility menu, choose the option **Stop and start services** and restart the web node to define it as a service.

   1. Exit the utility.

1. If using the IPTABLES firewall or equivalent service on Linux, then allow remote machines to access the public IP of the web node using the `iptables` command (or the equivalent) to open port 12800.

Your web node is now configured. Repeat these steps for each web node you want to add.

>[!Important]
>R Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

### 4. Setup enterprise-grade security

In production environments, we strongly recommend the following approaches:

1. [Configure SSL/TLS](../operationalize/configure-https.md) and install the necessary certificates.

1. Authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md).  

1. For added security, restrict the list of IPs that can access the machine hosting the compute node.


### 5. Provision on the cloud

If you are provisioning on a cloud service, then you must also [create inbound security rule for port 12800 in Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console. This endpoint allows clients to communicate with the R Server's operationalization server.

### 6. Set up a load balancer

You can set up the load balancer of your choosing. 

Keep in mind that web nodes are stateless. Therefore, session persistence ("stickiness") is NOT required. 

For proper access token signing and verification across your configuration, ensure that the JWT certificate settings are exactly the same for every web node.  These JWT settings are defined on each web node in the configuration file, appsetting.json. [Learn more...](../operationalize/configure-authentication.md#ldap-jwt)

### 7. Post configuration steps

1. [Update service ports](../operationalize/configure-use-admin-utility.md#ports), if needed.

1. [Run diagnostic tests](../operationalize/configure-run-diagnostics.md).

1. [Evaluate](../operationalize/configure-evaluate-capacity.md) the configuration's capacity.




## See also

* [Blog article: Configuring R Server to Operationalize Analytics using ARM Templates](https://blogs.msdn.microsoft.com/rserver/2017/05/14/configuring-r-server-to-operationalize-analytics-using-arm-templates/)