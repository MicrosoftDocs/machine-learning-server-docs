---

# required metadata
title: "Operationalization Administration Utility | Microsoft R Server Docs"
description: "Operationalization of R Analytics with Microsoft R Server"
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

# R Server Operationalization Administration

**Applies to:  Microsoft R Server 9.0.1**

[!include[Admin Utility Introduction](../includes/o16n/admin-utility-intro.md)]

<br>
<a name="launch"></a>

## Launch the Administrator Utility

These instructions describe how to launch the Administrator Utility.

**On Windows:**

+ You can launch the administration utility using the shortcut in the **Start** menu called **Microsoft R Server - Microsoft-R-Admin-Util**. 

+ Alternately, open a command line window with administrator privileges and enter the following commands:
  ```
  cd <MRS_home>\deployr
  dotnet Microsoft.DeployR.Utils.AdminUtil\Microsoft.DeployR.Utils.AdminUtil.dll
  ```
  where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

**On Linux:**

1. Launch the administration utility script with `root` or `sudo` privileges.

1. At the prompt, enter the following commands:
   ```
   cd /usr/lib64/microsoft-deployr/9.0.1
   dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll
   ```

<br><a name="admin-password"></a>

## Set/Update Local Administrator Password

When no other form of [authentication](security-authentication.md) is used, you must define a password for the local administrator account called `admin`.  If you do enable another form of authentication, the local administrator account is automatically disabled.

>The password for the local `admin` account must be 8-16 characters long and contain at least 1 or more uppercase character(s), 1 or more lowercase character(s), 1 or more number(s), and 1 or more special character(s).

**To set or update the local admin account password:**

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. From the main menu, choose the option to set a password for the local `admin` account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account. 
   >If your configuration has multiple web nodes, we recommend the same password be used.

1. Confirm the password.

<br><a name="startstop"></a>

## Starting and Stopping Services

To start or stop all operationalization-related services on the machine at once, use the administrator utility. 

**To stop or start a web node or compute node:**

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. From the main menu, choose the option **Stop and start services**.

1. From the sub-menu, choose which services to start or stop.

<br><a name="ports"></a>

## Update Port Numbers

You can update the ports numbers for the web node, compute node, or [`deployr-rserve`](https://github.com/Microsoft/deployr-rserve)(a forked version of RServe).

**To update the port values:**

1. Log into the machine on which your web node or compute node is installed.

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. From the main menu, choose the option **Change service ports**.

1. From the sub-menu, choose the port you want to update.

1. Enter the port number. 

>The port number will be updated the next time the [service is restarted](#startstop).

<br><a name="encrypt"></a>

## Encrypt Credentials 

For security purposes, we strongly recommend that you encrypt the connection string for the remote database and/or the password for the LDAP/LDAP-S credentials rather than store them in plain text in the `appsettings.json` configuration file. Here's how: 
       
1. On the web node, install a credential encryption certificate with a private key into the default certificate store on the local machine. 

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

      1. From the main menu, choose the option **Encrypt Credentials**.

      1. Specify where is the encryption certificate installed: 
        + Local machine (Computer account)
        + Current user (My user account)

        The list of available certificates appears.

      1. Specify the certificate to use for encryption.

      1. Enter information you want to encrypt.  The tool will return an encrypted string.

1. Open the configuration file, `appsettings.json`.

   + On Windows, this file is under `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\` where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

   + On Linux, this file is under `/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/`.

1. In that file, update the appropriate section for a [remote database connection](configure-remote-database.md#encrypt) or the [authentication password](security-authentication.md#encrypt) strings. 

<br><a name="test"></a>

## Diagnostic Testing

You can assess the state and health of your environment with the set of diagnostic tests found in this Administration Utility. 
Armed with this information, you can identify unresponsive components, execution problems, and access the log files. The set of diagnostic tests include a general health check of the configuration as well as tracing R code executions or web service executions.

[Learn how to run the diagnostics and troubleshoot.](admin-diagnostics.md)

<br><a name="capacity"></a>

## Evaluate Capacity

To evaluate the load balancing capacity, you can simulate the traffic for the configuration or for a given web service. You can test for maximum latency or maximum thread count.
+ **Maximum Latency:** Define the maximum number of milliseconds for a web node request, the initial thread count, and the thread increments for the test. The test will increase the number of threads by the defined increment until the defined time limit is reached.

+ **Maximum Thread Count:** Define the number of threads against which you want to run, such as 10, 15, or 40.  The test will increase the number of parallel requests by the specified increment until the maximum number of threads is reached. 

[Learn how to configure the test parameters, run the test, and interpret the results.](admin-evaluate-capacity.md)