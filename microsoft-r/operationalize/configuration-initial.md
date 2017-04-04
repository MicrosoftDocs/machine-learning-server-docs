---

# required metadata
title: "Configure R Server for Operationalization | Microsoft R Server Docs"
description: "Configuration Operationalization for Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
ms.topic: "article"
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

# Configuring R Server to operationalize analytics (One-Box Configuration)

**Applies to:  Microsoft R Server 9.x**

To benefit from Microsoft R Server’s deployment and operationalization features, you can configure R Server after installation to act as a deployment server and host analytic web services.


## One-box vs enterprise configurations

All configurations have at least a single web node and single compute node. **Web nodes** act as HTTP REST endpoints with which users can interact directly to make API calls. Web nodes also access the data in the database and send requests to the compute node for processing. **Compute nodes** are used to execute R code as a session or service. Each compute node has its own pool of R shells. By default, a SQLite 3.7+ database is installed, but you can, and in some cases must, install and [use a SQL Server (Windows) or PostgreSQL (Linux)](configure-remote-database.md) database instead.

R Server offers two types of configuration for operationalization/deployment:
1. **One-box configuration**: the simplest configuration is a single web node and compute node on a single machine, which is described in this article.

1. **Enterprise configuration**: a configuration where multiple nodes are configured on multiple machines along with other enterprise features. This configuration is described in detail in the **[Enterprise configuration](configure-enterprise.md)** article.

## Supported platforms 

The web nodes and compute nodes are supported on:
- Windows Server 2012 R2, Windows Server 2016
- Ubuntu 14.04, Ubuntu 16.04,
- CentOS/RHEL 7.x


## How to upgrade from 9.0  to 9.1 

To replace an older version, you can uninstall the older distribution before installing the new version (there is no in-place upgrade). **Carefully review the steps below.** 

1. If you used the default SQLite database, `deployrdb_9.0.0.db` in R Server 9.0 and want to persist the data, then you must **back up the SQLite database before uninstalling Microsoft R Server**. Make a copy of the database file and put it outside of the Microsoft R Server directory structure. 

   If you are using SQL Server or PostgreSQL, you do not need to do this step.

   >[!Warning]
   >If you skip this SQLite database backup step and uninstall Microsoft R Server 9.0 first, you will not be able to retrieve your database data.
   
1. Uninstall Microsoft R Server 9.0 as described in the article [Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md).  The uninstall process stashes away a copy of your 9.0 configuration files under this directory so you can seamlessly upgrade to R Server 9.1 in the next step:
   + Windows: `C:\Users\Default\AppData\Local\DeployR\current`
   + Linux: `/etc/deployr/current`

1. If you backed up a SQLite database in Step 1, now you must manually move `deployrdb_9.0.0.db` under this directory so it can be found during the upgrade:
   + Windows: `C:\Users\Default\AppData\Local\DeployR\current\frontend`
   + Linux: `/etc/deployr/current/frontend`

   If you are using a SQL Server or PostgreSQL database, you can skip this step.

1. Follow the instructions below to install Microsoft R Server 9.1 and configure your web and compute nodes. When you launch the Administration utility to configure web and compute nodes, the utility checks to see if any configuration files or SQLite database files are present in the folders mentioned above. 

   If found, you will be asked if you want to upgrade. If you answer `y`, the node will be installed and the prior edits you made to the configuration in 9.0 are automatically available in 9.1. You can safely ignore the Python warning during upgrade. 

<a name="onebox"></a>

## How to perform a one-box enterprise

With one-box configurations, as the name suggests, everything runs on a single machine and set-up is a breeze. This configuration includes an operationalization web node and compute node on the same machine. It also relies on the default local SQLite database.

This configuration is useful when you want to explore what it is to operationalize R analytics using R Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but might not be appropriate for production usage.

![One-box configuration](../media/o16n/setup-onebox.jpeg)

**To configure on a single machine:**

1. On each machine, install Microsoft R Server:

     + On Windows, install [R Server for windows](../rserver-install-windows.md).

     + On Linux, install [R Server for Linux](../rserver-install-linux-server.md).  

1. If on the following Linux flavors, then add a few symlinks:  (If on Windows, skip to the next step)

   + On CentOS 7.1, CentOS 7.2:
     ```
      cd /usr/lib64
      sudo ln -s libpcre.so.1   libpcre.so.0
      sudo ln -s libicui18n.so.50   libicui18n.so.36
      sudo ln -s libicuuc.so.50 libicuuc.so.36
      sudo ln -s libicudata.so.50 libicudata.so.36
     ```

   + On Ubuntu 14.04:
     ```
      sudo apt-get install libicu-dev

      cd /lib/x86_64-linux-gnu
      ln -s libpcre.so.3 libpcre.so.0
      ln -s liblzma.so.5 liblzma.so.0

      cd /usr/lib/x86_64-linux-gnu
      ln -s libicui18n.so.52 libicui18n.so.36
      ln -s libicuuc.so.52 libicuuc.so.36
      ln -s libicudata.so.52 libicudata.so.36
     ```

   + On Ubuntu 16.04:
     ```
      cd /lib/x86_64-linux-gnu
      ln -s libpcre.so.3 libpcre.so.0
      ln -s liblzma.so.5 liblzma.so.0

      cd /usr/lib/x86_64-linux-gnu
      ln -s libicui18n.so.55 libicui18n.so.36
      ln -s libicuuc.so.55 libicuuc.so.36
      ln -s libicudata.so.55 libicudata.so.36
     ```

   >**Note:** If there are issues with starting the compute node, see [here](admin-diagnostics.md).

1. [Launch the administration utility](admin-utility.md#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. Choose the option to **Configure R Server for Operationalization**.

1. Choose the option to **Configure for one box** to set up the web node and compute node onto the same machine.

   >[!IMPORTANT]
   > Do not choose the sub-options **Configure a web node** or **Configure a compute node** unless you intend to have them on separate machines, which is described below as an **Enterprise** configuration.

1. When prompted, provide a password for the built-in, local operationalization administrator account called `admin`.

1. Return to the main menu of the utility when the configuration ends.

1. [Run a diagnostic test of the configuration](admin-diagnostics.md).

1. If on Linux and using the IPTABLES firewall or equivalent service, then use the `iptables` command (or the equivalent) to open port 12800 to the public IP of the web node so that remote machines can access it.

>[!Important]
>R Server uses Kestrel as the web server for its operationalization web nodes. Consequently, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy set up.

You are now ready to begin operationalizating your R analytics with R Server.