---

# required metadata
title: "Configure Machine Learning Server 9.2.1 to operationalize analytics (one-box)"
description: "Configure Operationalization for Machine Learning Server 9.2.1"
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

# Configure Machine Learning Server  9.2.1 to operationalize analytics (One-box)

**Applies to: Machine Learning Server 9.2.1** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Find [Machine Learning Server 9.3](../install/configure-machine-learning-server-one-box.md) | [R Server 9.x](../install/operationalize-r-server-one-box-config.md)

You can configure Microsoft Learning Server after installation to act as a deployment server and to host analytic web services for operationalization. Machine Learning Server offers two types of configuration for operationalizing analytics and remote execution: **One-box and Enterprise**. This article describes the one-box configuration. For more on enterprise configurations, [see here](configure-machine-learning-server-enterprise.md).

A one-box configuration, as the name suggests, involves a single [web node and compute node](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) run on a single machine along with a database. Set-up is a breeze. This configuration is useful when you want to explore what it is to operationalize R and Python analytics using Machine Learning Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but might not be appropriate for production usage. 

![One-box configuration](./media/configure-machine-learning-server-one-box/setup-onebox.png)


<a name="onebox"></a>

## How to configure

>[!Important]
>For your convenience, Azure Resource Management templates are available to quickly deploy and configure Machine Learning Server for operationalization in Azure.
>
>Get one of [these templates on GitHub](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates/enterprise-configuration). Then, learn how to use it with this [blog post](https://blogs.msdn.microsoft.com/rserver/2017/05/14/configuring-r-server-to-operationalize-analytics-using-arm-templates/). Or, read a general description of [ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#template-deployment).

**To configure on a single machine:**

1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#supported-platforms)

   + Windows instructions: [Installation steps](../install/machine-learning-server-windows-install.md) | [Offline steps](../install/machine-learning-server-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called 'H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path' with a value of the parent path to the R\_SERVER or PYTHON\_SERVER folder (for example, C:\Program Files\Microsoft SQL Server\140\).

   + Linux instructions: [Installation steps](../install/machine-learning-server-linux-install.md) | [Offline steps](../install/machine-learning-server-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges:
   
   + Windows instructions: launch the administration utility as an administrator (right-click) using the shortcut in the Start menu called Administration Utility.

   + Linux instructions:  
     ```
     cd /opt/microsoft/mlserver/9.2.1/o16n
     sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
     ```

   >[!NOTE]
   >To bypass the interactive configuration steps, specify the following argument and ['admin' password](../deployr/../operationalize/configure-authentication.md#local) when launching the utility:
   >-silentoneboxinstall myPassword.
   >If you choose this method, you can skip the next three substeps. Learn more about command-line switches for this script, [here](../operationalize/configure-use-admin-utility.md#switch).

1. Choose the option to **Configure server**.

1. Choose the option to **Configure for one box** to set up the web node and compute node onto the same machine.

   >[!IMPORTANT]
   > Do not choose the suboptions **Configure a web node** or **Configure a compute node** unless you intend to have them on separate machines. This multi-machine configuration is described as an [**Enterprise** configuration](configure-machine-learning-server-enterprise.md).

1. When prompted, provide a password for the built-in, local operationalization administrator account called 'admin'.

1. Return to the main menu of the utility when the configuration ends.

1. [Run a diagnostic test of the configuration](../operationalize/configure-run-diagnostics.md).

1. If on Linux and using the IPTABLES firewall or equivalent service, then use the `iptables` command (or the equivalent) to open port 12800 to the public IP of the web node so that remote machines can access it.

>[!Important]
>Machine Learning Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

You are now ready to begin operationalizing your R analytics with Machine Learning Server.



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

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator/root/sudo privileges. The utility checks to see if any configuration files from past releases are present under the `current` folder mentioned previously.

   + Windows instructions: launch the administration utility AS AN ADMINISTRATOR (right-click) using the shortcut in the Start menu called Administration Utility.

   + Linux instructions:  
     ```
     cd /opt/microsoft/mlserver/9.2.1/o16n
     sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
     ```

1. From the menus, choose **Configure server** and then choose **Configure for one box**. The configuration script begins.

1. When the script asks you if you'd like to upgrade, enter `y`. The nodes are automatically set up using the configuration you had for R Server 9.x. Note: You can safely ignore the Python warning during upgrade.

1. From the main menu, choose the option to **Run Diagnostic Tests** to [test the configuration](../operationalize/configure-run-diagnostics.md).

1. Exit the utility. Your web and compute nodes are now upgraded and configured as they were in version 9.0.

>[!WARNING]
>The entities created by users, specifically web services, and [snapshots](../r/how-to-execute-code-remotely.md#snapshot), are tied to their usernames. For this reason, you must be careful to prevent changes to the user identifier over time. Otherwise, pre-existing web services and snapshots cannot be mapped to the users who created them. For this reason, we strongly recommend that you DO NOT change the unique LDAP identifier in appsettings.json once users start publishing service or creating snapshots. 