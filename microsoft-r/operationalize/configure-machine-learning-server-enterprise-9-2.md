---

# required metadata
title: "Configure Machine Learning Server 9.2.1 to operationalize analytics (Enterprise) - Machine Learning Server "
description: "Configure Operationalization for Machine Learning Server 9.2.1, load balancer, "
keywords: "setup machine learning server for deployment; install machine learning server for deploying"
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
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

# Configure Machine Learning Server 9.2.1 to operationalize analytics (Enterprise)

**Applies to: Machine Learning Server 9.2.1** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Find [Machine Learning Server 9.3](../install/configure-machine-learning-server-enterprise.md) | [R Server 9.x](../install/operationalize-r-server-enterprise-config.md)

You can configure Microsoft Learning Server after installation to act as a deployment server and to host analytic web services for operationalization. Machine Learning Server offers two types of configuration for operationalizing analytics and remote execution: **One-box and Enterprise**. This article describes the enterprise configuration. For more on one-box configurations, [see here](configure-machine-learning-server-one-box.md).

An enterprise configuration involves multiple [web and compute nodes](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) that  are configured on multiple machines along with other enterprise features.  These nodes can be scaled independently. Scaling out web nodes enables an active-active configuration that allows you to load balance the incoming API requests.  Additionally, with multiple web nodes, you must use a [SQL Server or PostgreSQL database](../operationalize/configure-remote-database-to-operationalize.md) to share data and web services across web node services. The web nodes are stateless therefore there is no need for session stickiness if you use a load balancer.

For added security, you can [configure SSL](../operationalize/configure-https.md) and authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md) in this configuration.

![Enterprise Configuration](./media/configure-machine-learning-server-enterprise/configure-enterprise.png)

## How to configure

>[!Important]
>For a speedy setup in Azure, try one of [our Azure Resource Management templates](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates) stored in GitHub. This [blog post](https://blogs.msdn.microsoft.com/rserver/2017/05/14/configuring-r-server-to-operationalize-analytics-using-arm-templates/) explains how. 

### 1. Configure a database

While the web node configuration sets up a local SQLite database by default, you must use a SQL Server or PostgreSQL database for this configuration for any of the following situations:
+ Have multiple web nodes (so data can be shared across web nodes)  

+ Want to achieve higher availability

+ Need a remote database for your web node

To configure that database, [follow these instructions](../operationalize/configure-remote-database-to-operationalize.md).

>[!WARNING] 
>Choose and configure your database now. If you configure a different database later, all data in your current database is lost.

<a name="add-compute-nodes"></a>

### 2. Configure compute nodes

Configure one or more compute nodes as needed. We highly recommend that you configure each node (compute or web) on its own machine for higher availability.

In the Enterprise configuration, side-by-side installations of a web and compute node are not supported.
 
1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#supported-platforms)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges. 
   + Windows instructions: launch the administration utility AS AN ADMINISTRATOR (right-click) using the shortcut in the Start menu called Administration Utility.

   + Linux instructions:  
     ```
     cd /opt/microsoft/mlserver/9.2.1/o16n
     sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
     ```

1. From the main utility menu, choose **Configure server** and then choose **Configure a compute node** from the submenu. 

   >[!NOTE]
   >To bypass this interactive node configuration step, specify the following switch when launching the utility:
   >-silentcomputenodeinstall
   >Learn more about command-line switches for this utility [here](../operationalize/configure-admin-cli-launch.md#switch).

1. If you plan on configuring SSL/TLS and [install the necessary certificates](../operationalize/configure-https.md) on the compute node, do so now.

1. Open the port 12805 _on every compute node_. If you plan to configure SSL/TLS, you must do so BEFORE opening this port. 

   + On Windows: Add a firewall exception to open the port number. 
  
   + On Linux: Use 'iptables' or the equivalent command to open the port number.


You can now **repeat these steps** for each compute node you want to add.


<a name="webnode"></a>

### 3. Configure web nodes

In an enterprise configuration, you can set up one or more web nodes. It is possible to run the web node service from within IIS. 
 
1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#supported-platforms)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges to configure a web node:

   + Windows instructions: launch the administration utility AS AN ADMINISTRATOR (right-click) using the shortcut in the Start menu called Administration Utility.

   + Linux instructions:  
     ```
     cd /opt/microsoft/mlserver/9.2.1/o16n
     sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
     ```

     >[!NOTE]
     >Bypass these interactive steps to install the node and set an admin password using these command-line switches:
     >-silentwebnodeinstall mypassword uri1,uri2
     >Learn more about command-line switches for this utility [here](../operationalize/configure-admin-cli-launch.md#switch).

   1. From the main menu, choose **Configure server**. Then, choose **Configure a web node** from the submenu. 
  
   1. When prompted, provide a password for the built-in, local operationalization administrator account called 'admin'.  Later, you can configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-authentication.md#local).   
 
   1. When prompted, enter the IP address of each compute node you configured. You can specify a specific URI or  specify IP ranges. For multiple compute nodes, separate each URI with a comma. 

      For example: http://1.1.1.1:12805, http://1.0.1-3.1-2:12805
  
      In this example, the range represents six IP values: 1.0.1.1, 1.0.1.2, 1.0.2.1, 1.0.2.2, 1.0.3.1,  1.0.3.2.

   1. Return the main menu of the utility.

   >[!Important]
   >When configuring your web node, you might see the following message:  "Web Node was not able to start because it is not configured." Typically, this is not really an issue since the web node is automatically restarted within 5 minutes by an auto-recovery mechanism.
 
1. In the same utility, test the configuration. From the main utility menu, choose **Run Diagnostic Tests** and choose a [diagnostic test](../operationalize/configure-run-diagnostics.md).

1. Exit the utility.

1. If you plan on configuring SSL/TLS and [install the necessary certificates](../operationalize/configure-https.md) on the compute node, do so now.

1. Open the port 12800 _on every web node_. If you plan to configure SSL/TLS, you must do so BEFORE opening this port. 

   + On Windows: Add a firewall exception to open the port number. 
  
   + On Linux: Use 'iptables' or the equivalent command to open the port number.

You can now **repeat these steps** for each web node you want to add.

>[!Important]
>Machine Learning Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

### 4. Setup enterprise-grade security

In production environments, we strongly recommend the following approaches:

1. [Configure SSL/TLS](../operationalize/configure-https.md) and install the necessary certificates.

1. Authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md).  

1. For added security, restrict the list of IPs that can access the machine hosting the compute node.
  
**Important!** For proper access token signing and verification across your configuration, ensure that the JWT certificate settings are exactly the same for every web node.  These JWT settings are defined on each web node in the configuration file, appsetting.json. [Learn more...](../operationalize/configure-authentication.md#ldap-jwt)


### 5. Provision on the cloud

If you are provisioning on a cloud service, then you must also [create inbound security rule for port 12800 in Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console. This endpoint allows clients to communicate with the  Machine Learning Server's operationalization server.



### 6. Set up a load balancer

You can set up the load balancer of your choosing. Keep in mind that web nodes are stateless. Therefore, session persistence ("stickiness") is NOT required. 


### 7. Post configuration steps

1. [Update service ports](../operationalize/configure-admin-cli-ports.md), if needed.

1. [Run diagnostic tests](../operationalize/configure-run-diagnostics.md).

1. [Evaluate](../operationalize/configure-evaluate-capacity.md) the configuration's capacity.


## How to upgrade 

To replace an older version, you can uninstall the older distribution before installing the new version (there is no in-place upgrade). 

Carefully review the steps in the following sections.

<a name="upgradecomputenode"></a>

### Upgrade a compute node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall Microsoft R Server 9.0 or 9.1 using the instructions in the article [Uninstall Microsoft R Server to upgrade to a newer version](../install/r-server-install-uninstall-upgrade.md). The uninstall process stashes away a copy of your 9.0 or 9.1 configuration files under this directory so you can seamlessly upgrade to Machine Learning Server 9.2.1 in the next step:
   
   + Windows: C:\Users\Default\AppData\Local\DeployR\current

   + Linux: /etc/deployr/current

1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#supported-platforms)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator/root/sudo privileges. The utility checks to see if any configuration files from past releases are present under the `current` folder mentioned previously.

   + Windows instructions: launch the administration utility AS AN ADMINISTRATOR (right-click) using the shortcut in the Start menu called Administration Utility.

   + Linux instructions:  
     ```
     cd /opt/microsoft/mlserver/9.2.1/o16n
     sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
     ```

1. Choose **Configure server** from the menu and then **Configure a compute node** from the submenu. 

1. When the script asks you if you want to upgrade, enter `y`. The node is automatically set up using the configuration you had for R Server 9.0 or 9.1. 
    
You can now **repeat these steps** for each compute node.

<a name="upgradewebnode"></a>

### Upgrade a web node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall Microsoft R Server 9.0 or 9.1 using the instructions in the article [Uninstall Microsoft R Server to upgrade to a newer version](../install/machine-learning-server-linux-uninstall.md). The uninstall process stashes away a copy of your 9.0 or 9.1 configuration files under this directory so you can seamlessly upgrade to Machine Learning Server 9.2.1 in the next step:
   
   + Windows: C:\Users\Default\AppData\Local\DeployR\current

   + Linux: /etc/deployr/current

1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#supported-platforms)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator/root/sudo privileges. The utility checks to see if any configuration files from past releases are present under the `current` folder mentioned previously.

   + Windows instructions: launch the administration utility AS AN ADMINISTRATOR (right-click) using the shortcut in the Start menu called Administration Utility.

   + Linux instructions:  
     ```
     cd /opt/microsoft/mlserver/9.2.1/o16n
     sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
     ```

1. Choose **Configure server** from the menu and then **Configure a web node** from the submenu.  

1. When the script asks you if you'd like to upgrade, enter `y`. The node is automatically set up using the configuration you had for R Server 9.0 or 9.1. 

1. From the main menu, choose the option to **Run Diagnostic Tests** to [test the configuration](../operationalize/configure-run-diagnostics.md).

You can now **repeat these steps** for each node.
