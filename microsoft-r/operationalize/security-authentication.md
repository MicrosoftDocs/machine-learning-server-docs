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

# Authentication for R Server's Operationalization

To secure connections and communications, you have several authentication options:

|Authentication Method|When to Use|
|----------------------------------|----------------------------------|
|[Local `admin` account](#local)|Use with [one-box](configuration-initial.md) configurations|
|[Active Directory / LDAP](#ldap)|Use with [enterprise](configuration-initial.md) _on-premise_ configurations|
|[Active Directory / LDAP-S](#ldap)|Use with [enterprise](configuration-initial.md) _on-premise_ configurations with SSL/TLS enabled|
|[Azure Active Directory](#aad)|Use with [enterprise](configuration-initial.md) _cloud_ configurations|

<br>

<a name="local"></a>

## Local Administrator Account Authentication

During configuration, a default `admin` account is created for R Server's operationalization feature. While this might be sufficient when trying this feature out with a [one-box configuration](configuration-initial.md#onebox) when everything is running within the trust boundary, it is not recommended with [enterprise configurations](configuration-initial.md#enterprise).

To set or change the password for the local administrator account after the configuration script has been run, [follow these steps](admin-utility.md#admin-password).

<a name="ldap"></a>

## Active Directory and LDAP/LDAP-S

Active Directory (AD) and LDAP are a great authentication option for on-premise configurations to ensure that domain users have access to the APIs.  

The standard protocol for reading data from and writing data to Active Directory (AD) domain controllers (DCs) is LDAP. AD LDAP traffic is unsecured by default, which makes it possible to use network-monitoring software to view the LDAP traffic between clients and DCs.  

By default, the LDAP security provider is not configured. To enable LDAP authentication support, update the relevant properties in your configuration file. The values you assign to these properties should match the configuration of your LDAP Directory Information Tree (DIT).

You can make LDAP traffic confidential and secure using Secure Sockets Layer (SSL) / Transport Layer Security (TLS) technology. This combination is referred to as LDAP over SSL (or LDAP-S). To ensure that no one else can read the traffic, SSL/TLS establishes an encrypted tunnel between an LDAP client and a DC. [Learn more about enabling SSL/TLS.](security-https.md) Reasons for enabling LDAP-S include:

+ Organizational security policies typically require that all client/server communication is encrypted.
+ Applications use simple BIND to transport credentials and authenticate against a Domain Controller. As simple BIND exposes the users’ credentials in clear text, using SSL/TLS to encrypt the authentication session is strongly recommended.
+ Use of proxy binding or password change over LDAP, which requires LDAP-S. Bind to an AD LDS instance Through a Proxy Object
+ Applications that integrate with LDAP servers (such as Active Directory or Active Directory Domain Controllers) might require encrypted LDAP communications.

**On each Web node, do the following:**

1. Enable LDAP/LDAP-S in the external JSON configuration file, `appsettings.json`:

   1. Open the configuration file, `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.
   
   1. Search for the section starting with `"LDAP": {`

   1. Uncomment characters in that section and update the properties so that they match the values in your Active Directory Service Interfaces Editor.  Properties include:

      |LDAP Properties|Definition|
      |---------------|-------------------------------|
      |`Host`|Address of the Active Directory server|
      |`UseLDAPS`|Set `true` for LDAP-S or `false` for LDAP<br>**Note:** If LDAP-S is configured, an installed LDAP service certificate is assumed so that the tokens produced by Active Directory/LDAP can be signed and accepted by R Server. |
      |`BindFilter`|The template used to do the Bind operation. For example, `"CN={0},CN=DeployR,DC=TEST,DC=COM"`. {0} is the user's DN.|
      |`QueryUserDn`|Distinguished name of user with read-only query capabilities with which to authenticate|
      |`QueryUserPassword`|Password for that user with which to authenticate (value must be encrypted). For security purposes, you must [encrypt LDAP login credentials](admin-utility.md#encrypt) before adding the information to this file.|
      |`SearchBase`|Context name to search in, relative to the base of the configured ContextSource, e.g. `'ou=users,dc=example,dc=com'`.| 
      |`SearchFilter`|The pattern to be used for the user search. {0} is the user's DN.|

1. If using a certificate for access token signing, do the following: 

   >This is particularly useful when you have multiple Web nodes and want the tokens to be signed consistently by every Web node in your configuration. 
   >
   >In production environments, we recommend that you use a certificate with a private key to sign the user access tokens between the Web node and the LDAP server.
    
   1. On each machine hosting the Web node, install the trusted, signed **access token signing certificate** with a private key in the certificate store. Take note of the `Subject` name of the certificate as you'll need this info later.

   1. In the `appsettings.json` file, search for the section starting with `"JWTSigningCertificate": {`

   1. Uncomment characters in that section and update the properties so that they match the values for your token signing certificate:
      ```
      "JWTSigningCertificate": {
          "StoreName": "My",
          "StoreLocation": "LocalMachine",
          "SubjectName": "<subject name>"
       }
       ```

1. Launch the administrator's utility and:
   1. [Restart the Web node](admin-utility.md#startstop).
 
   1. Run the [diagnostic tests](admin-utility.md#test).

1. Repeat these steps on each  machine hosting the Web node.

<br>

<a name="aad"></a>

## Azure Active Directory 

[Azure Active Directory (AD)](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory) can be used to securely authenticate  in the cloud when the client application and Web node have access to the internet.

**On each Web node, do the following:**

1. [Log into](https://azure.microsoft.com/en-us/features/azure-portal/) portal and [register](https://azure.microsoft.com/en-us/documentation/articles/sql-database-client-id-keys/) a new web application.   

    1. Once the new application has been created, click **CONFIGURE**.

    1. Take note of the value for the  `CLIENT ID` on the page. Also, take note of the application's tenant id.  The tenant ID is displayed as part of the URL such as: `https://manage.windowsazure.com/tenantname#Workspaces/ActiveDirectoryExtension/Directory/<TenantID>/...`

1. Enable Azure AD in the external JSON configuration file:

    1. Open the configuration file, `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

    1. Search for the section starting with `"AzureActiveDirectory": {`

    1. Uncomment characters in that section and update the properties so that they match the values in the Azure Management portal.  Properties include:

       |Azure AD Properties|Definition|
       |----------------|-------------------------------|
       |`Authority`|Use `https://login.windows.net/<ID>.onmicrosoft.com` where `<ID>` is the tenant ID value you copied from the Azure management portal.|
       |`Audience`|Use the `CLIENT ID` value you copied from the Azure management portal.|

1. Launch the administrator's utility and:
   1. [Restart the Web node](admin-utility.md#startstop).
   1. Run the [diagnostic tests](admin-utility.md#test).

1. Repeat these steps on each  machine hosting the Web node.
