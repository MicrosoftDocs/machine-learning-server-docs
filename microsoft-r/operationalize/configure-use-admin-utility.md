---

# required metadata
title: "Administration Utility - Machine Learning Server | Microsoft Docs"
description: "configure R Server for operationalization, set passwords, restart nodes, update ports, run diagnostics, and encrypt credentials."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "6/21/2017"
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

# Administration utility for operationalizing analytics

**Applies to:  Machine Learning Server, Microsoft R Server 9.x**

This article describes how to use the administration utility to configure the server for operationalization, set passwords, restart nodes, update ports, run diagnostics, and encrypt credentials.

Use the utility to:
+ [Configure server for operationalization](../install/operationalize-r-server-one-box-config.md) front-ends and back-ends
+ [Set a local admin password](#admin-password)
+ [Stop and restart](#startstop) web and compute node services
+ [Update the service ports](#ports)
+ [Run diagnostic tests](configure-run-diagnostics.md)
+ [Encrypt credentials](#encrypt)
+ [Evaluate the configuration's capacity](configure-evaluate-capacity.md)
+ [Manage compute nodes](#uris)
+ [Learn about command line switches to this utility script](#switch)

<a name="launch"></a>

## Launch the Administrator Utility

These instructions describe how to launch the Administrator Utility.

**On Windows:**

Open a command line window with administrator privileges and enter the following commands:

|Version|Commands|
|----|------------|
|9.2.1|cd \<server_home><br>dotnet Microsoft.MLServer.Utils.AdminUtil\Microsoft.MLServer.Utils.AdminUtil.dll|
|9.1.0|cd \<server_home><br>dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll|
|9.0.1|cd \<server_home><br>dotnet Microsoft.DeployR.Utils.AdminUtil\Microsoft.DeployR.Utils.AdminUtil.dll|

where '\<server_home>' is the [path to the installation directory](../operationalize/configure-find-admin-configuration-file.md).  

**On Linux:**

Launch the administration utility script with `root` or `sudo` privileges with the following commands:

|Version|Commands|
|----|------------|
|9.2.1|cd \<server_home><br>sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll|
|9.1.0|cd \<server_home><br>sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll|
|9.0.1|cd \<server_home><br>sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll|

where '\<server_home>' is the [path to the installation directory](../operationalize/configure-find-admin-configuration-file.md).

<br><a name="admin-password"></a>

## Set/Update Local Administrator Password

When no other form of [authentication](configure-authentication.md) is used, you must define a password for the local administrator account called 'admin'.  If you do enable another form of authentication, the local administrator account is automatically disabled.

This local 'admin' password must be 8-16 characters long and contain at least 1 uppercase character(s), 1+ lowercase character(s), 1+ number(s), and 1+ special character(s).

**To set or update the local admin account password:**

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose the option to set a password for the local 'admin' account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account. 
   >If your configuration has multiple web nodes, we recommend the same password be used.

1. Confirm the password.

>[!NOTE]
>You can bypass script interface using the argument '-setpassword <password>'. Learn about all command line switches for this script, [here](#switch). For example: 
>
>dotnet Microsoft.MLServer.Utils.AdminUtil\Microsoft.MLServer.Utils.AdminUtil.dll -setpassword my-password 

<br><a name="startstop"></a>

## Starting and Stopping Services

To start or stop all operationalization-related services on the machine at once, use the administrator utility. 

**To stop or start a web node or compute node:**

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose the option **Stop and start services**.

1. From the submenu, choose which services to start or stop.

<br><a name="ports"></a>

## Update Port Numbers

You can update the ports numbers for the web node, compute node, or [deployr-rserve](https://github.com/Microsoft/deployr-rserve)(a forked version of RServe).

**To update the port values:**

1. Log in to the machine on which your web node or compute node is installed.

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose the option **Change service ports**.

1. From the sub-menu, choose the port you want to update.

1. Enter the port number. 

>The port number will be updated the next time the [service is restarted](#startstop).

<br><a name="encrypt"></a>

## Encrypt Credentials 

For security purposes, we strongly recommend that you encrypt strings, such as remote database connection strings and LDAP/LDAP-S passwords, rather than store strings in plain text in the appsettings.json configuration file. 

The encryption function available in the administration utility relies on the RSA algorithm for encryption. 

**To encrypt credentials:**
       
1. On the web node, install a credential encryption certificate with a private key into the default certificate store on the local machine. That location is in the Windows certificate store or in the Linux-based PFX file. 

   The length of the encryption key depends on the certificate, however, we recommend a length of at least 1048 bits.

   Ensure that your certificate is secured properly. On Windows, for example, you can use Bitlocker to encrypt the disk.  

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

      1. From the main menu, choose the option **Encrypt Credentials**.

      1. Specify where is the encryption certificate installed: 
         + Local machine (Computer account)
         + Current user (My user account)

         The list of available certificates appears.

      1. Specify which encryption certificate to use.

      1. Enter information you want to encrypt.  The tool returns an encrypted string.

1. Open the configuration file, \<web-node-install-path>/appsettings.json. (Find the [install path](../operationalize/configure-find-admin-configuration-file.md) for your version.) 

1. In that file, update the appropriate section for a [remote database connection](configure-remote-database-to-operationalize.md#encrypt) or the [authentication password](configure-authentication.md#encrypt) strings. 

>[!NOTE]
>You can bypass script interface using the argument '-encryptsecret encryptSecret encryptSecretCertificateStoreName encryptSecretCertificateStoreLocation encryptSecretCertificateSubjectName'. See the table at the end of this topic, [here](#switch).


<br><a name="test"></a>

## Diagnostic Testing

You can assess the state and health of your environment with the set of diagnostic tests found in this Administration Utility. 
Armed with this information, you can identify unresponsive components, execution problems, and access the log files. The set of diagnostic tests include a general health check of the configuration and  the trace of R code executions or web service executions.

[Learn how to run the diagnostics and troubleshoot.](configure-run-diagnostics.md)

<br><a name="capacity"></a>

## Evaluate Capacity

To evaluate the load balancing capacity, you can simulate the traffic for the configuration or for a given web service. You can test for maximum latency or maximum thread count.
+ **Maximum Latency:** Define the maximum number of milliseconds for a web node request, the initial thread count, and the thread increments for the test. The test increases the number of threads by the defined increment until the defined time limit is reached.

+ **Maximum Thread Count:** Define the number of threads against which you want to run, such as 10, 15, or 40.  The test increases the number of parallel requests by the specified increment until the maximum number of threads is reached. 

[Learn how to configure the test parameters, run the test, and interpret the results.](configure-evaluate-capacity.md)


<br><a name="uris"></a>

## Manage Compute Nodes

Whenever a web node is started, it looks in the database and configuration file for the list of URIs for all known compute nodes with which the web node might work. You can add to and manage this list of compute nodes using the Administration Utility. You can update this list of URIs from any web node and then all other web nodes will automatically know of the updated list the next time the web node is restarted.

This utility option was introduced with Machine Learning Server 9.2.1 to facilitate the declaration and management of these URIs. Previous releases manage URIs in appsettings.json only.

>[!Important]
>1. If the ['owner' role is defined](configure-roles.md), then the administrator must belong to the 'Owner' role in order to declare compute nodes. 
>
>2. If you declared URIs in R Server and have upgraded to 9.2.1, the URIs in appsettings.json are still read at startup and stored into the database. You can remove them from the appsettings.json of each web node and manage them in one place with this feature now.

**To declare compute nodes:**

1. Log in to the machine on which one of your web nodes is installed.

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose the option **Manage compute nodes**.

1. From the sub-menu, choose **Add URIs** to declare one or more compute node URIs.
   
1. When prompted, enter the IP address of each compute node you configured in the previous step. You can specify a specific URI or  specify port ranges (or IP octets). For multiple compute nodes, separate each URI with a comma. 
   For example: `http://1.1.1.1:12805, http://1.0.1-3.1-2:12805`.

1. Return the main menu of the utility.

<br><a name="switch"></a>

## Command line switches

The following command line switches are available for the administration utility.

|Switch|Description|Introduced in version|
|----|-----|:---:|
|-silentoneboxinstall  <br><br>-silentinstall |Sets up a [one-box configuration](../install/operationalize-r-server-one-box-config.md) silently, sets an admin password, and in 9.2 allows you to [specify compute node URIs](#uris) or IP ranges. For example:<br> -silentinstall mypass123 http://1.1.1.1:12805,http://1.0.1.1-3:12805 |9.1, URIs in 9.2|
|-silentwebnodeinstall <password>|Configures a [web node](../install/operationalize-r-server-enterprise-config.md) silentlysets an admin password, and in 9.2 allows you to [specify compute node URIs](#uris) or IP ranges. For example: -silentwebnodeinstall mypass123 http://1.1.1.1:12805,http://1.0.1.1-3:12805|9.1, URIs in 9.2|
|-silentcomputenodeinstall|Configures a [compute node](../install/operationalize-r-server-enterprise-config.md) silently. For example: -silentcomputenodeinstall|9.1|
|-setpassword <password>|Sets the password. Cannot be used <br> if LDAP or AAD was configured. For example:<br> -setpassword mypass123|9.1|
|-preparedbmigration <appSettingsPath>|Migrates the data from current database to a <br>different database schema. Takes the path to<br>the web node’s appsetting.json file as an<br> argument. This is uncommonly needed as a<br>step [when upgrading](../install/operationalize-r-server-enterprise-config.md#upgradewebnode). For example:<br>`-preparedbmigration C:/Program Files/`<br>`Microsoft/mrs/o16n/Microsoft.RServer.WebNode/`<br>appsettings.json|9.1|
|-encryptsecret encryptSecret encryptSecretCertificateStoreName encryptSecretCertificateStoreLocation encryptSecretCertificateSubjectName|Silently [encrypts secrets](#encrypt). For example: <br>-encryptsecret thesecret storeName storeLocationsubjectName|9.1|