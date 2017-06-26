---

# required metadata
title: "Administration Utility - Microsoft R Server | Microsoft Docs"
description: "configure R Server for operationalization, set passwords, restart nodes, update ports, run diagnostics, and encrypt credentials."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "6/21/2017"
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

# Administration utility for operationalizing analytics with R Server

**Applies to:  Microsoft R Server 9.x**

This article describes how to use the administration utility to configure R Server for operationalization, set passwords, restart nodes, update ports, run diagnostics, and encrypt credentials.

Use the utility to:
+ [Configure R Server for operationalization](configuration-initial.md) front-ends and back-ends
+ [Set a local admin password](#admin-password)
+ [Stop and restart](#startstop) web and compute node services
+ [Update the service ports](#ports)
+ [Run diagnostic tests](admin-diagnostics.md)
+ [Encrypt credentials](#encrypt)
+ [Evaluate the configuration's capacity](admin-evaluate-capacity.md)
+ [Learn about command line switches to this utility script](#switch)

<br>
<a name="launch"></a>

## Launch the Administrator Utility

These instructions describe how to launch the Administrator Utility.

**On Windows:**

You can launch the administration utility AS AN ADMINISTRATOR (right-click) using the shortcut in the **Start** menu called **Microsoft R Server - Microsoft-R-Admin-Util**.  

>[!WARNING]
> If your organization has the default powershell execution policy of "Restricted" (common for Windows 10 and Windows Server 2012), you may have issues running the administration utility using the shortcut. In that case, either use the alternate option detailed in the next bullet or you can change the execution policy to "Unrestricted." Read this article on [powershell execution policies](https://msdn.microsoft.com/en-us/powershell/reference/5.1/microsoft.powershell.core/about/about_execution_policies) for details. 

Alternately, open a command line window with administrator privileges and enter the following commands:

|Version|Commands|
|----|------------|
|9.1|cd <MRS_home>\o16n<br>dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll|
|9.0|cd <MRS_home>\deployr<br>dotnet Microsoft.DeployR.Utils.AdminUtil\Microsoft.DeployR.Utils.AdminUtil.dll|

where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

**On Linux:**

Launch the administration utility script with `root` or `sudo` privileges with the following commands:

|Version|Commands|
|----|------------|
|9.1|cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0<br>sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll|
|9.0|cd /usr/lib64/microsoft-deployr/9.0.1<br>sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll|

<br><a name="admin-password"></a>

## Set/Update Local Administrator Password

When no other form of [authentication](security-authentication.md) is used, you must define a password for the local administrator account called 'admin'.  If you do enable another form of authentication, the local administrator account is automatically disabled.

This local 'admin' password must be 8-16 characters long and contain at least 1 uppercase character(s), 1+ lowercase character(s), 1+ number(s), and 1+ special character(s).

**To set or update the local admin account password:**

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. From the main menu, choose the option to set a password for the local 'admin' account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account. 
   >If your configuration has multiple web nodes, we recommend the same password be used.

1. Confirm the password.

>[!NOTE]
>You can bypass script interface using the argument '-setpassword <password>' such as `dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll -setpassword my-password`. Learn about all command line switches for this script, [here](#switch).

You can also 
<br><a name="startstop"></a>

## Starting and Stopping Services

To start or stop all operationalization-related services on the machine at once, use the administrator utility. 

**To stop or start a web node or compute node:**

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. From the main menu, choose the option **Stop and start services**.

1. From the submenu, choose which services to start or stop.

<br><a name="ports"></a>

## Update Port Numbers

You can update the ports numbers for the web node, compute node, or [deployr-rserve](https://github.com/Microsoft/deployr-rserve)(a forked version of RServe).

**To update the port values:**

1. Log in to the machine on which your web node or compute node is installed.

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

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

1. [Launch the administration utility](#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

      1. From the main menu, choose the option **Encrypt Credentials**.

      1. Specify where is the encryption certificate installed: 
         + Local machine (Computer account)
         + Current user (My user account)

         The list of available certificates appears.

      1. Specify which encryption certificate to use.

      1. Enter information you want to encrypt.  The tool returns an encrypted string.

1. [Open the `appsettings.json` configuration file](admin-configuration-file.md).

1. In that file, update the appropriate section for a [remote database connection](configure-remote-database.md#encrypt) or the [authentication password](security-authentication.md#encrypt) strings. 

>[!NOTE]
>You can bypass script interface using the argument '-encryptsecret encryptSecret encryptSecretCertificateStoreName encryptSecretCertificateStoreLocation encryptSecretCertificateSubjectName'. See the table at the end of this topic, [here](#switch).


<br><a name="test"></a>

## Diagnostic Testing

You can assess the state and health of your environment with the set of diagnostic tests found in this Administration Utility. 
Armed with this information, you can identify unresponsive components, execution problems, and access the log files. The set of diagnostic tests include a general health check of the configuration and  the trace of R code executions or web service executions.

[Learn how to run the diagnostics and troubleshoot.](admin-diagnostics.md)

<br><a name="capacity"></a>

## Evaluate Capacity

To evaluate the load balancing capacity, you can simulate the traffic for the configuration or for a given web service. You can test for maximum latency or maximum thread count.
+ **Maximum Latency:** Define the maximum number of milliseconds for a web node request, the initial thread count, and the thread increments for the test. The test increases the number of threads by the defined increment until the defined time limit is reached.

+ **Maximum Thread Count:** Define the number of threads against which you want to run, such as 10, 15, or 40.  The test increases the number of parallel requests by the specified increment until the maximum number of threads is reached. 

[Learn how to configure the test parameters, run the test, and interpret the results.](admin-evaluate-capacity.md)

<br><a name="switch"></a>

## Command line switches

The following command line switches are available for the administration utility.

|Switch|Description|Version|
|----|-----|:---:|
|-silentoneboxinstall <password> <br><br>-silentinstall <password>|Sets up a [one-box configuration](configuration-initial.md) silently<br>  and sets an admin  password. For example: <br>`-silentinstall mypass123`|9.1|
|-silentwebnodeinstall <password>|Configures a [web node](configure-enterprise.md) silently<br> and sets an admin password. For example: <br>`-silentwebnodeinstall mypass123`|9.1|
|-silentcomputenodeinstall|Configures a [compute node](configure-enterprise.md) silently. For example: <br>`-silentcomputenodeinstall`|9.1|
|-setpassword <password>|Sets the password. Cannot be used <br> if LDAP or AAD was configured. For example: <br>`-setpassword mypass123`|9.1|
|-preparedbmigration <appSettingsPath>|Migrates the data from current database to a <br>different database schema. Takes the path to<br>the web node’s appsetting.json file as an<br> argument. This is uncommonly needed as a<br>step [when upgrading](configure-enterprise.md#upgradewebnode). For example:<br>`-preparedbmigration C:/Program Files/`<br>`Microsoft/mrs/o16n/Microsoft.RServer.WebNode/`<br>`appsettings.json`|9.1|
|-encryptsecret encryptSecret encryptSecretCertificateStoreName encryptSecretCertificateStoreLocation encryptSecretCertificateSubjectName|Silently [encrypts secrets](#encrypt). For example: <br>-encryptsecret thesecret storeName storeLocationsubjectName|9.1|
