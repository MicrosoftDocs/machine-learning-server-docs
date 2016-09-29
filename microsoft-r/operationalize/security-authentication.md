---

# required metadata
title: "Security Authentication"
description: "Operationalization of R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
ms.topic: "get-started-article"
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

# Configuring Authentication for the Deployment Server

To secure DeployR, you have several authentication options:

|Authentication Method|When to Use|
|----------------------------------|----------------------------------|
|[Local `administrator` account](#local)|Use with [one-box](configurations.md) configurations|
|[Active Directory / LDAP](#ldap)|Use with [enterprise-ready](configurations.md) _on-premise_ configurations|
|[Active Directory / LDAP-S](#ldap)|Use with [enterprise-ready](configurations.md) _on-premise_ configurations with SSL/TLS enabled|
|[Azure Active Directory](#aad)|Use with [enterprise-ready](configurations.md) _cloud_ configurations|


[ADD DANIEL's AUTHENTICATION DIAGRAM HERE]


<a name="local"></a>

## Local Administrator Account Authentication

During configuration, a default `administrator` account is created for R Server's DeployR. While this might be sufficient when trying out DeployR with a [one-box configuration](configurations.md#onebox) when everything is running within the trust boundary, it is not recommended with [enterprise-ready configurations](configurations.md#enterpriseready).

**Set or change the password for the local administrator account**:

If you want set or change the password after the configuration script has been run, you can do so in the administrator utility.

1. Here's how...



<a name="ldap"></a>

## Active Directory and LDAP/LDAP-S

Active Directory (AD) and LDAP are a great authentication option for on-premise configurations of DeployR to ensure that domain users have access to the APIs.  

The standard protocol for reading data from and writing data to Active Directory (AD) domain controllers (DCs) is LDAP. AD LDAP traffic is unsecured by default, which makes it possible to use network-monitoring software to view the LDAP traffic between clients and DCs.  

By default, the LDAP security provider is not configured. To enable LDAP authentication support, update the relevant properties in your DeployR configuration file. The values you assign to these properties should match the configuration of your LDAP Directory Information Tree (DIT).

You can make LDAP traffic confidential and secure using Secure Sockets Layer (SSL) / Transport Layer Security (TLS) technology. This combination is referred to as LDAP over SSL (or LDAP-S). To ensure that no one else can read the traffic, SSL/TLS establishes an encrypted tunnel between an LDAP client and a DC. [Learn more about enabling SSL/TLS for DeployR.](security-https.md) Reasons for enabling LDAP-S include:

+ Organizational security policies typically require that all client/server communication is encrypted.
+ Applications use simple BIND to transport credentials and authenticate against a DC. As simple BIND exposes the users’ credentials in clear text, using SSL/TLS to encrypt the authentication session is strongly recommended.
+ Use of proxy binding or password change over LDAP, which requires LDAP-S. Bind to an AD LDS instance Through a Proxy Object
+ Applications that integrate with LDAP servers (such as Active Directory or Active Directory Domain Controllers) might require encrypted LDAP communications.

<br>
**How to enable AD and LDAP/LDAP-S**

On each front-end machine, do the following:

1. Update the DeployR external JSON configuration file with the LDAP settings.

  1. Open the configuration file, `appsettings.json`.

  1. Enable AD LDAP by @@@@ HOW DO YOU SIGNAL IN THE CONFIG FILE THAT YOU WANT LDAP? did we implement a way to let deployr know which one is being used in the config file) or does DeployR try each method until one works?

  1. Search for the section starting with `"LDAP": {`

  1. Update all the relevant properties in that `LDAP` section so that they match the values in your Active Directory Service Interfaces Editor.  Properties include:
     + `Host`: Address of the Active Directory server

     + `UseLDAPS`: `true` for LDAP-S or `false` for LDAP
        > If LDAP-S is configured, an installed LDAP service certificate is assumed so that the tokens produced by Active Directory/LDAP can be signed and accepted by DeployR. 

     + `BindFilter`: ?????@@@@@

     + `ManagerDn`: Distinguished Name with which to authenticate  (This value must be encrypted.)

     + `ManagerPassword`: Manager password  with which to authenticate  (This value must be encrypted.)

     + `SearchBase`: Context name to search in, relative to the base of the configured ContextSource, e.g. 'ou=users,dc=example,dc=com'. (speeds searching)
 
1. Restart the front-end so the changes can take effect. @@@POINT TO ADMIN UTILITY

1. Repeat these steps on each front-end machine.

<a name="aad"></a>

## Azure Active Directory 

[Azure Active Directory (AD)](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory) can be used to securely authenticate with DeployR in the cloud when the client application and DeployR have access to the internet.


<br>
**How to enable Azure AD**

On each front-end machine, do the following:

To enable Azure Active Directory (AD), the administrator should do the following:

1. Log into [Microsoft Azure management portal](https://azure.microsoft.com/en-us/features/azure-portal/).   

1. [Register a new web application](https://azure.microsoft.com/en-us/documentation/articles/sql-database-client-id-keys/)  to get a client ID.

1. Finish creating the app, click **CONFIGURE**.

1. Take note of the value for the  `CLIENT ID`. You will use this value in the DeployR configuration file.

1. Also take note of the tenant id. You will use this value in the DeployR configuration file. The tenant ID is displayed as part of the URL: ```https://manage.windowsazure.com/tenantname#Workspaces/ActiveDirectoryExtension/Directory/<TenantID>/...``` 

1. Update the DeployR external JSON configuration file with the LDAP settings.

  1. Open the configuration file, `appsettings.json`.

  1. Enable Azure AD by @@@@ HOW DO YOU SIGNAL IN THE CONFIG FILE THAT YOU WANT AZURE AD?

  1. Search for the section starting with `"AzureActiveDirectory": {`

  1. Update all the relevant properties in that `AzureActiveDirectory` section so that they match the values in the Azure Management portal.  Properties include:
     + `Authority`: Use `https://login.windows.net/<ID>` where `<ID>` is the tenant ID value you copied from the Azure management portal.

     + `Audience`: Use the `CLIENT ID` value you copied from the Azure management portal.
 
1. Restart the front-end so the changes can take effect. @@@POINT TO ADMIN UTILITY

1. Repeat these steps on each front-end machine.