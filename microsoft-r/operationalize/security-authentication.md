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
|Local `administrator` [account](#local)|Use with [one-box](configurations.md) configurations|
|[Active Directory / LDAP](#ldap)|Use with [enterprise-ready](configurations.md) configurations|
|[Active Directory / LDAP-S](#ldap)|Use with [enterprise-ready](configurations.md)  configurations with SSL/TLS enabled|
|[Azure Active Directory](#aad)|Use in the cloud|


[ADD DANIEL's AUTHENTICATION DIAGRAM HERE]


<a name="local"></a>

## Local Administrator Account Authentication

When R Server is installed and DeployR is configured, a default `administrator` account is created. After configuration, you can set a password for this local administrator. While this might be sufficient when trying out DeployR with a [one-box configuration](configurations.md) when everything is running within the trust boundary, it is not recommended with [enterprise-ready configurations](configurations.md).

**Set the password for the local administrator account**:

1. Here's how...


<a name="ldap"></a>

## Active Directory / LDAP(S)

Active Directory (AD) and LDAP are a great authentication option for on-premise configurations of DeployR.

By default, the LDAP security provider is not configured. To enable LDAP authentication support, you must update the relevant properties in your DeployR configuration file. The values you assign to these properties should match the configuration of your LDAP Directory Information Tree (DIT).

The standard protocol for reading data from and writing data to Active Directory (AD) domain controllers (DCs) is LDAP. AD LDAP traffic is unsecured by default, which makes it possible to use network-monitoring software to view the LDAP traffic between clients and DCs. 

> The password to the local `administator` account is disabled when LDAP is enabled.

You can make LDAP traffic confidential and secure using Secure Sockets Layer (SSL) / Transport Layer Security (TLS) technology. This combination is referred to as LDAP over SSL (or LDAP-S). To ensure that no one else can read the traffic, SSL/TLS establishes an encrypted tunnel between an LDAP client and a DC. [Learn more about enabling SSL/TLS for DeployR.]() @@@@ADD LINK HERE

Reasons for enabling LDAP-S include:

+ Organizational security policies typically require that all client/server communication is encrypted.
+ Applications use simple BIND to transport credentials and authenticate against a DC. As simple BIND exposes the users’ credentials in clear text, using SSL/TLS to encrypt the authentication session is strongly recommended.
+ Use of proxy binding or password change over LDAP, which requires LDAP-S. Bind to an AD LDS instance Through a Proxy Object
+ Applications that integrate with LDAP servers (such as Active Directory or Active Directory Domain Controllers) might require encrypted LDAP communications.


### AD / LDAP

To enable Active Directory (AD) and LDAP for on-premise configurations of DeployR, the administrator should do the following:

1. Take note of the Active Directory domain controller name and the "BaseContainer", for example:
   ```
   "Host": "redmond.corp.microsoft.com",
   "BaseContainer": "dc=redmond,dc=corp,dc=microsoft,dc=com"
   ```

   @@@ How do you find this info exactly?

2. Ensure that the DeployR front-ends have access to this domain controller in the firewall.
   
   @@@ How do you do this for Windows?

   
   @@@ How do you do this for Linux?

3. Insert the `Host` and `BaseContainer` into the DeployR JSON configuration file.

   @@@ What's the name of this file?

   @@@ Where can i find this file on Windows

   @@@ Where can i find this file on Linux

   @@@ Where do I insert this information exactly?

4. A certificate is needed so that the tokens produced by Active Directory/LDAP can be signed and accepted by DeployR.

   @@@ Which certificate is this?  1 ,2 ,3 4, or something else?

   @@@ What do I do on Windows

   @@@ On Linux??


Add a note: point your application developers to this tutorial so they can learn how to integrate with DeployR. Application Developer: Read new tutorial: “how to build an application using DeployR as a back-end  that authenticates using AD/LDAP” (better title to come) which includes C# Sharp and Java examples. It should explain that developers must make a REST call to /login to get the token needed by DeployR.

@@@ Daniel, do we have this tutorial yet??



### AD / LDAPS

For LDAPS, do the following:

@@@ HOW DO YOU ALL THE ABOVE BUT FOR LDAP-S

<a name="aad"></a>

## Azure Active Directory 

Azure Active Directory can be used to secure DeployR in the cloud when the client and the DeployR Service have access to the internet.

To enable Azure Active Directory (AD), the administrator should do the following:

1. Log into [Microsoft Azure management portal](https://azure.microsoft.com/en-us/features/azure-portal/).   

1. [Register a new web application](https://azure.microsoft.com/en-us/documentation/articles/sql-database-client-id-keys/)  to get a client ID.

1. Finish creating the app, click **CONFIGURE**, and copy the `CLIENT ID`. Also take note of the `TENANT ID`. You will need these values in your code.

1. Insert the `CLIENT ID` and `TENANT ID` into the DeployR JSON configuration file.

   @@@ What's the name of this file?

   @@@ Where can i find this file on Windows

   @@@ Where can i find this file on Linux

   @@@ Where do I insert this information exactly?


Add a note: point your application developers to this tutorial so they can learn how to integrate with DeployR.
Application Developer: Read new tutorial: “how to build an application using DeployR as a back-end  that authenticates using Azure AD” (better title to come) which includes C# Sharp and Java examples. It should explain that developers must download the libraries for the type of application they are developing from https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-libraries/ With this library, they can get a token from Azure Active Directory. 