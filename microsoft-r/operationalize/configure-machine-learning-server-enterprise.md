---

# required metadata
title: "Configure Machine Learning Server 9.3 and later to operationalize analytics (Enterprise)"
description: "Configure Machine Learning Server 9.3 and later to operationalize analytics (Enterprise setup) with load balancing"
keywords: "setup machine learning server for deployment; install machine learning server for deploying"
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "conceptual"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# Configure Machine Learning Server 9.3 and later to operationalize analytics (Enterprise)

**Applies to: Machine Learning Server 9.3 and later** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For older versions: [ML Server 9.2.1](configure-machine-learning-server-enterprise-9-2.md) | [R Server 9.x](../install/operationalize-r-server-enterprise-config.md)

You can configure Microsoft Learning Server after installation to act as a deployment server and to host analytic web services for operationalization. Machine Learning Server offers two types of configuration for operationalizing analytics and remote execution: **One-box and Enterprise**. This article describes the enterprise configuration. For more on one-box configurations, [see here](configure-machine-learning-server-one-box.md).

An enterprise configuration involves multiple [web and compute nodes](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) that  are configured on multiple machines along with other enterprise features.  These nodes can be scaled independently. Scaling out web nodes enables an active-active configuration that allows you to load balance the incoming API requests.  Additionally, with multiple web nodes, you must use a [SQL Server or PostgreSQL database](../operationalize/configure-remote-database-to-operationalize.md) to share data and web services across web node services. The web nodes are stateless therefore there is no need for session stickiness if you use a load balancer.

For added security, you can [configure SSL](../operationalize/configure-https.md) and authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md) in this configuration.

![Configure Machine Learning Server on enterprise architecture](./media/configure-machine-learning-server-enterprise/configure-enterprise.png)

## How to configure

>[!Important]
>For a speedy setup in Azure, try one of [our Azure Resource Management templates](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates) stored in GitHub. This [blog post](https://blogs.msdn.microsoft.com/rserver/2017/05/14/configuring-r-server-to-operationalize-analytics-using-arm-templates/) explains how. 

### 1. Configure a database

While the web node configuration sets up a local SQLite database by default, you must use a SQL Server or PostgreSQL database if any of the following situations apply:
+ You intend to set up multiple web nodes (so data can be shared across web nodes)  
+ You want to achieve higher availability
+ You need a remote database for your web node

To configure that database, [follow these instructions](../operationalize/configure-remote-database-to-operationalize.md).

>[!WARNING] 
>Configure your database now before moving on. If you configure a different database later, all data in the current database is lost.

<a name="add-compute-nodes"></a>

### 2. Configure compute nodes

Configure one or more compute nodes as needed. We highly recommend that you configure each node (compute or web) on its own machine for higher availability.

In the Enterprise configuration, side-by-side installations of a web and compute node are not supported.
 
1. Install Machine Learning Server and its dependencies as follows. Learn about [supported platforms](../operationalize/configure-start-for-administrators.md#supported-platforms)  for this configuration.

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 2.0 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

1. In a command-line window or terminal launched with administrator (Windows) or root/sudo (Linux) privileges, run [CLI commands](configure-admin-cli-launch.md) to configure a compute node.
   ```azurecli
   # With elevated privileges, run the following commands.
   # Set up a compute node

   az ml admin node setup --computenode
   
   # Check the node is now running
   az ml admin node list
   ``` 

   You can always configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-admin-cli-local-password.md) later.

   If you need help with CLI commands, run the command but add `--help` to the end.

1. If you plan to configure SSL/TLS and [install the necessary certificates](../operationalize/configure-https.md) on the compute node, do so now.

1. Open the port 12805 _on every compute node_. If you plan to configure SSL/TLS, do so BEFORE opening this port. 

   + On Windows: Add a firewall exception to open the port number. 
  
   + On Linux: Use 'iptables' or the equivalent command to open the port number.


You can now **repeat these steps** for each compute node you want to add.


<a name="webnode"></a>

### 3. Configure web nodes

In an enterprise configuration, you can set up one or more web nodes. It is possible to run the web node service from within IIS. 
 
1. Install Machine Learning Server and its dependencies as follows. Learn about [supported platforms](../operationalize/configure-start-for-administrators.md#supported-platforms)  for this configuration.

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 2.0 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

1. In a command-line window or terminal launched with administrator (Windows) or root/sudo (Linux) privileges, run [CLI commands](configure-admin-cli-launch.md) to:
   + Set up the web node.
   + Define a password for the default 'admin' account.  Replace <Password> with a password of your choice. The admin password must be 8-16 characters long and contain 1+ uppercase character, 1+ lowercase character, 1+ one number, and 1+ special characters:<br/> `~ ! @ # $ % ^ & ( ) - _ + = | < > \ / ; : , .`
   + Declare the IP address of each compute node. Separate each URI with a comma. For multiple compute nodes, separate each URI with a comma. The following example shows a single URI and a range of IPs (1.0.1.1, 1.0.1.2, 1.0.2.1, 1.0.2.2, 1.0.3.1, 1.0.3.2): <br/>
   `--uri http://1.1.1.1:12805,http://1.0.1-3.1-2:12805`
   
   ```azurecli
   # Configure a web node
   az ml admin node setup --webnode --admin-password <Password> --confirm-password <Password> --uri <URI1>,<URI2>
   ``` 

1. In the same CLI, test the configuration. Learn more about [diagnostic tests](../operationalize/configure-run-diagnostics.md). For the full test of the configuration, enter the following in the CLI:
   ```azurecli
   # Check the node is now running
   az ml admin node list

   # Run the diagnostic test
   az ml admin diagnostic run
   ```

1. If you plan on configuring SSL/TLS and [install the necessary certificates](../operationalize/configure-https.md) on the compute node, do so now.

1. Open the port 12800 _on every web node_. If you plan to configure SSL/TLS, you must do so BEFORE opening this port. 

   + On Windows: Add a firewall exception to open the port number. 
  
   + On Linux: Use 'iptables' or the equivalent command to open the port number.

You can now **repeat these steps** for each web node you want to add.

>[!Important]
>Machine Learning Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

### 4. Setup enterprise-grade security

In production environments, we strongly recommend the following approaches:

1. [Configure SSL/TLS](../operationalize/configure-https.md) and install the necessary certificates.

1. Authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md).  

1. For added security, restrict the list of IPs that can access the machine hosting the compute node.
  
**Important!** For proper access token signing and verification across your configuration, ensure that the JWT certificate settings are exactly the same for every web node.  These JWT settings are defined on each web node in the configuration file, appsetting.json. [Learn more...](../operationalize/configure-authentication.md#ldap-jwt)


### 5. Provision on the cloud

If you are provisioning on a cloud service, then you must also [create inbound security rule for port 12800 in Azure](https://azure.microsoft.com/ntation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console. This endpoint allows clients to communicate with the  Machine Learning Server's operationalization server.



### 6. Set up a load balancer

You can set up the load balancer of your choosing. Keep in mind that web nodes are stateless. Therefore, session persistence ("stickiness") is NOT required. 


### 7. Post configuration steps

1. [Update service ports](../operationalize/configure-admin-cli-ports.md), if needed.

1. Using the CLI, you can test the configuration. Learn more about [diagnostic tests](../operationalize/configure-run-diagnostics.md). For the full test of the configuration, enter the following in the CLI:
   ```azurecli
   # You must be authenticated to run diagnostic tests.
   az login --mls

   # Run test
   az ml admin diagnostic run
   ```

1. [Evaluate](../operationalize/configure-evaluate-capacity.md) the configuration's capacity.


## How to upgrade 

To replace an older version, you can uninstall the older distribution before installing the new version (there is no in-place upgrade). 

Carefully review the steps in the following sections.

<a name="upgradecomputenode"></a>

### Upgrade a compute node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall the old version. The uninstall process stashes away a copy of your configuration files for a seamlessly upgrade to Machine Learning Server 9.3 and later.
    + For Machine Learning Server 9.2.1, read these instructions: [Windows](../install/machine-learning-server-windows-uninstall.md) | [Linux](../install/machine-learning-server-linux-uninstall.md).
    + For Microsoft R Server 9.x, read this [Uninstall Microsoft R Server to upgrade to a newer version](../install/r-server-install-uninstall-upgrade.md). 

1. Install Machine Learning Server and its dependencies as follows.  Learn about [supported platforms](../operationalize/configure-start-for-administrators.md#supported-platforms)  for this configuration.

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md) 

     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 2.0 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

1. In a command-line window or terminal launched with administrator (Windows) or root/sudo (Linux) privileges, run [CLI commands](configure-admin-cli-launch.md) to configure a compute node.
   ```azurecli
   # Set up a compute node
   az ml admin node setup --computenode

   # Check the node is now running
   az ml admin node list
   ``` 

   You can always configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-admin-cli-local-password.md) later.

   If you need help with CLI commands, run the command but add `--help` to the end.

    
You can now **repeat these steps** for each compute node.

<a name="upgradewebnode"></a>

### Upgrade a web node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall the old version. The uninstall process stashes away a copy of your configuration files for a seamlessly upgrade to Machine Learning Server 9.3 and later.
    + For Machine Learning Server 9.2.1, read these instructions: [Windows](../install/machine-learning-server-windows-uninstall.md) | [Linux](../install/machine-learning-server-linux-uninstall.md).
    + For Microsoft R Server 9.x, read this [Uninstall Microsoft R Server to upgrade to a newer version](../install/r-server-install-uninstall-upgrade.md). 
    
1. Install Machine Learning Server and its dependencies as follows.  Learn about [supported platforms](../operationalize/configure-start-for-administrators.md#supported-platforms)  for this configuration.

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md) 

     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 2.0 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140).

1. In a command-line window or terminal launched with administrator (Windows) or root/sudo (Linux) privileges, run [CLI commands](configure-admin-cli-launch.md) to:
   + Set up the web node.
   + Define a password for the default 'admin' account.  Replace <Password> with a password of your choice. The admin password must be 8-16 characters long and contain 1+ uppercase character, 1+ lowercase character, 1+ one number, and 1+ special characters:<br/> `~ ! @ # $ % ^ & ( ) - _ + = | < > \ / ; : , .`
   + Declare the IP address of each compute node. Separate each URI with a comma. For multiple compute nodes, separate each URI with a comma. The following example shows a single URI and a range of IPs (1.0.1.1, 1.0.1.2, 1.0.2.1, 1.0.2.2, 1.0.3.1, 1.0.3.2): <br/>
   `--uri http://1.1.1.1:12805, http://1.0.1-3.1-2:12805`
   
   ```azurecli
   az ml admin node setup --webnode --admin-password <Password> --confirm-password <Password> --uri <URI1>,<URI2>
   ```

1. Verify the configuration was successful with these [CLI commands](configure-admin-cli-launch.md):

   ```azurecli   
   # Check the node is now running
   az ml admin node list

   # Authenticate with Machine Learning Server
   az login --mls

   # Run diagnostics to make all is configured properly
   az ml admin diagnostic run
   ``` 

   You can always configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-admin-cli-local-password.md) later.

   If you need help with CLI commands, run the command but add `--help` to the end.

You can now **repeat these steps** for each node.
