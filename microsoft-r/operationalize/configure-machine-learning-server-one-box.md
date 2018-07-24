---

# required metadata
title: "Configure Machine Learning Server 9.3 to operationalize analytics (one-box)"
description: "Configure Machine Learning Server 9.3 to operationalize analytics on a single machine (One-box)"
keywords: "setup machine learning server for deployment; install machine learning server for deploying"
author: "HeidiSteen"
ms.author: "heidist"
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

# Configure Machine Learning Server 9.3 to operationalize analytics on a single machine (One-box)

**Applies to: Machine Learning Server 9.3** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For older versions: [ML Server 9.2.1](configure-machine-learning-server-one-box-9-2.md) | [R Server 9.x](../install/operationalize-r-server-one-box-config.md)

You can configure Microsoft Learning Server after installation to act as a deployment server and to host analytic web services for operationalization. Machine Learning Server offers two types of configuration for operationalizing analytics and remote execution: **One-box and Enterprise**. This article describes the one-box configuration. For more on enterprise configurations, [see here](configure-machine-learning-server-enterprise.md).

A one-box configuration, as the name suggests, involves a single [web node and compute node](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) run on a single machine along with a database. Set-up is a breeze. This configuration is useful when you want to explore what it is to operationalize R and Python analytics using Machine Learning Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but might not be appropriate for production usage. 

![Configure Machine Learning Server on one-box architecture](./media/configure-machine-learning-server-one-box/setup-onebox.png)


<a name="onebox"></a>

## How to configure

>[!Important]
> You can create a ready-to-use server with just a few clicks using Azure Marketplace VM [Machine Learning Server Operationalization](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/deploy-r.operationalization?tab=Overview). Navigate to the virtual machine listing on [Azure portal](https://portal.azure.com/#create/deploy-r.operationalizationonebox), select the **Create** button at the bottom, provide necessary inputs to create the Operationalization Server. 
>
> [Azure Resource Management templates](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates) are also provided in GitHub. This [blog post](https://blogs.msdn.microsoft.com/mlserver/2018/02/27/configuring-microsoft-machine-learning-server-9-3-to-operationalize-analytics-using-arm-templates/) provides more information. 
>

**To configure on a single machine:**

1. Install Machine Learning Server and its dependencies as follows. Learn about [supported platforms](../operationalize/configure-start-for-administrators.md#supported-platforms)  for this configuration.

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 2.0 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140\).

1. In a command line window or terminal that was launched with administrator (Windows) or root/sudo (Linux) privileges, run the following [CLI commands](configure-admin-cli-launch.md) to:
   + Set up a web node and compute node on the same machine.
   + Define a password for the default 'admin' account.  Replace <Password> with a password of your choice. The admin password must be 8-16 characters long and contain 1+ uppercase character, 1+ lowercase character, 1+ one number, and 1+ special characters:<br/> `~ ! @ # $ % ^ & ( ) - _ + = | < > \ / ; : , .`
   + Authenticate with Machine Learning Server.
   + [Test the configuration](../operationalize/configure-run-diagnostics.md).

   ```azurecli
   # With elevated privileges, run the following commands.
   # Set up both nodes on one machine
   az ml admin node setup --onebox --admin-password <Password> --confirm-password <Password>

   # Check that the nodes are now running
   az ml admin node list

   # Authenticate via CLI
   # Account name is `admin` if LDAP or AAD is not set up.
   az login --mls

   # Test configuration to validate setup
   az ml admin diagnostic run
   ``` 

   You can always configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-admin-cli-local-password.md) later.

   If you need help with CLI commands, run the command but add `--help` to the end.

1. If on Linux and using the IPTABLES firewall or equivalent service, then use the `iptables` command (or the equivalent) to open port 12800 to the public IP of the web node so that remote machines can access it.

>[!Important]
>Machine Learning Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

You are now ready to begin operationalizing your R and Python analytics with Machine Learning Server.



## How to upgrade 

Carefully review the following steps.

1. Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

2. R Server 9.x users only: If you used the default SQLite database, `deployrdb_9.0.0.db` or `deployrdb_9.1.0.db` in R Server and want to persist the data, then you must **back up the SQLite database before uninstalling Microsoft R Server**. Make a copy of the database file and put it outside of the Microsoft R Server directory structure. 

   (If you are using a SQL Server or PostgreSQL database, you can skip this step.)

   >[!Warning]
   >If you skip this SQLite database backup step, your database data will be lost.

3. Uninstall the old version. The uninstall process stashes away a copy of your configuration files for a seamlessly upgrade to Machine Learning Server 9.3.
    + For Machine Learning Server 9.2.1, read these instructions: [Windows](../install/machine-learning-server-windows-uninstall.md) | [Linux](../install/machine-learning-server-linux-uninstall.md).
    + For Microsoft R Server 9.x, read this [Uninstall Microsoft R Server to upgrade to a newer version](../install/r-server-install-uninstall-upgrade.md). 

4. If you are using a SQL Server or PostgreSQL database, you can skip this step. If you backed up a SQLite database in Step 1, manually move the .db file under this directory so it can be found during the upgrade:
   + Windows: C:\Users\Default\AppData\Local\DeployR\current\frontend

   + Linux: /etc/deployr/current/frontend

5. Install Machine Learning Server and its dependencies as follows.  Learn about [supported platforms](../operationalize/configure-start-for-administrators.md#supported-platforms)  for this configuration.

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)

     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 2.0 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140\).

6. In a command line window or terminal that was launched with administrator (Windows) or root/sudo (Linux) privileges, run [CLI commands](configure-admin-cli-launch.md) to:

   + Set up a web node and compute node on the same machine.
   + Define a password for the default 'admin' account.  Replace <Password> with a password of your choice. The admin password must be 8-16 characters long and contain 1+ uppercase character, 1+ lowercase character, 1+ one number, and 1+ special characters:<br/> `~ ! @ # $ % ^ & ( ) - _ + = | < > \ / ; : , .`
   + Authenticate with Machine Learning Server.
   + [Test the configuration](../operationalize/configure-run-diagnostics.md).
   
   ```azurecli
   # Set up both nodes on one machine
   az ml admin node setup --onebox --admin-password <Password> --confirm-password <Password>

   # Check that the nodes are now running
   az ml admin node list
   ``` 
   You can always configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-admin-cli-local-password.md) later.

   If you need help with CLI commands, run the command but add `--help` to the end.

   You can always configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-admin-cli-local-password.md) later.

7. If on Linux and using the IPTABLES firewall or equivalent service, then use the `iptables` command (or the equivalent) to open port 12800 to the public IP of the web node so that remote machines can access it.

>[!Important]
>Machine Learning Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

You are now ready to begin operationalizing your R and Python analytics with Machine Learning Server.

>[!WARNING]
>The entities created by users, specifically web services, and [snapshots](../r/how-to-execute-code-remotely.md#snapshot), are tied to their usernames. For this reason, you must be careful to prevent changes to the user identifier over time. Otherwise, pre-existing web services and snapshots cannot be mapped to the users who created them. For this reason, we strongly recommend that you DO NOT change the unique LDAP identifier in appsettings.json once users start publishing service or creating snapshots. 
