---

# required metadata
title: "Configure Machine Learning Server 9.3 to operationalize analytics (one-box)"
description: "Configure Machine Learning Server 9.3 to operationalize analytics on a single machine (One-box)"
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

# Configure Machine Learning Server 9.3 to operationalize analytics on a single machine (One-box)

**Applies to: Machine Learning Server 9.3** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For older versions: [ML Server 9.2.1](../install/configure-machine-learning-server-one-box-9-2.md) | [R Server 9.x](../install/operationalize-r-server-one-box-config.md)

You can configure Microsoft Learning Server after installation to act as a deployment server and to host analytic web services for operationalization. Machine Learning Server offers two types of configuration for operationalizing analytics and remote execution: **One-box and Enterprise**. This article describes the one-box configuration. For more on enterprise configurations, [see here](configure-machine-learning-server-enterprise.md).

A one-box configuration, as the name suggests, involves a single [web node and compute node](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) run on a single machine along with a database. Set-up is a breeze. This configuration is useful when you want to explore what it is to operationalize R and Python analytics using Machine Learning Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but might not be appropriate for production usage. 

![One-box configuration](./media/configure-machine-learning-server-one-box/setup-onebox.png)


<a name="onebox"></a>

## How to configure

>[!Important]
>For a speedy setup in Azure, try one of [our Azure Resource Management templates](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates) stored in GitHub. This [blog post](https://blogs.msdn.microsoft.com/rserver/2017/05/14/configuring-r-server-to-operationalize-analytics-using-arm-templates/) explains how. 

**To configure on a single machine:**

1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#supported-platforms)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140\).

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

1. In a command line window or terminal that was launched with administrator (Windows) or root/sudo (Linux) privileges, run commands to configure a web node and compute node on the same machine and [test the configuration](../operationalize/configure-run-diagnostics.md).
   ```
   # Set up both nodes on one machine
   az ml admin node setup --onebox
   # Check the nodes are now running
   az ml admin node list
   # Run configuration test to validate setup
   az ml admin diagnostic configure
   ``` 

1. If on Linux and using the IPTABLES firewall or equivalent service, then use the `iptables` command (or the equivalent) to open port 12800 to the public IP of the web node so that remote machines can access it.

>[!Important]
>Machine Learning Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

You are now ready to begin operationalizing your R and Python analytics with Machine Learning Server.



## How to upgrade 

Carefully review the following steps.

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. If you used the default SQLite database, `deployrdb_9.0.0.db` or `deployrdb_9.1.0.db` in R Server and want to persist the data, then you must **back up the SQLite database before uninstalling Microsoft R Server**. Make a copy of the database file and put it outside of the Microsoft R Server directory structure. 

   (If you are using a SQL Server or PostgreSQL database, you can skip this step.)

   >[!Warning]
   >If you skip this SQLite database backup step and uninstall Microsoft R Server 9.x first, you cannot retrieve your database data.

1. Uninstall Microsoft R Server 9.0 or 9.1 using the instructions in the article [Uninstall Microsoft R Server to upgrade to a newer version](../install/r-server-install-uninstall-upgrade.md). The uninstall process stashes away a copy of your 9.0 or 9.1 configuration files under this directory so you can seamlessly upgrade to Machine Learning Server 9.2.1 in the next step:
   
   + Windows: C:\Users\Default\AppData\Local\DeployR\current

   + Linux: /etc/deployr/current

1. If you backed up a SQLite database in Step 1, manually move the .db file under this directory so it can be found during the upgrade:
   + Windows: C:\Users\Default\AppData\Local\DeployR\current\frontend

   + Linux: /etc/deployr/current/frontend

   (If you are using a SQL Server or PostgreSQL database, you can skip this step.)

1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#supported-platforms)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140\).

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

1. In a command line window or terminal that was launched with administrator (Windows) or root/sudo (Linux) privileges, run commands to configure a web node and compute node on the same machine.
   ```
   # Set up both nodes on one machine
   az ml admin node setup --onebox
   # Check the nodes are now running
   az ml admin node list
   ```

1. When the script asks you if you'd like to upgrade, enter `y`. The nodes are automatically set up using the configuration you had for R Server 9.x. Note: You can safely ignore the Python warning during upgrade.

1. Use the CLI to test the configuration. Learn more about [diagnostic tests](../operationalize/configure-run-diagnostics.md). For the full test of the configuration, enter the following in the CLI:
   ```
   az ml admin diagnostic configure
   ```

  Your web and compute nodes are now upgraded and configured as they were in the previous version.

>[!WARNING]
>The entities created by users, specifically web services, and [snapshots](../r/how-to-execute-code-remotely.md#snapshot), are tied to their usernames. For this reason, you must be careful to prevent changes to the user identifier over time. Otherwise, pre-existing web services and snapshots cannot be mapped to the users who created them. For this reason, we strongly recommend that you DO NOT change the unique LDAP identifier in appsettings.json once users start publishing service or creating snapshots. 