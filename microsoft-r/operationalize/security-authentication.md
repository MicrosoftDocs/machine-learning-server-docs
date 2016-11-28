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

R Server's offers seamless integration with authentication solutions for operationalization. To secure connections and communications, you have several options:

|Authentication Method|When to Use|
|----------------------------------|----------------------------------|
|[Local `admin` account](#local)|Use with [one-box](configuration-initial.md) configurations|
|[Active Directory / LDAP](#ldap)|Use with [enterprise](configuration-initial.md) _on-premise_ configurations|
|[Active Directory / LDAP-S](#ldap)|Use with [enterprise](configuration-initial.md) _on-premise_ configurations with SSL/TLS enabled|
|[Azure Active Directory](#aad)|Use with [enterprise](configuration-initial.md) _cloud_ configurations|

<br>

![Security](../media/o16n/security.png)

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

**On each web node, do the following:**

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

[Azure Active Directory (AAD)](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory) can be used to securely authenticate  in the cloud when the client application and Web node have access to the internet.

**In the Azure Classic Portal, do the following:**

1. Log on to the [Azure portal](https://portal.azure.com/), and navigate to your application. Copy the URL in your browser address bar. You will use this to configure your Azure Active Directory app.

1. Sign in to the [Azure classic portal](https://manage.windowsazure.com/) and navigate to **Active Directory** in the left hand pane.

1. Select your directory. If the Azure Active Directory has not been set up yet, contact your system administrator.  

1. Select the **Applications** tab at the top. 

1. Now, create a web app that will be tied to the Azure Active Directory as follows: 

   1. In the **Applications** tab, click **ADD** at the bottom to create a new app registration. A dialog appears.
 
   1. Click **Add an application my organization is developing**. The **Add Application** wizard appears.

   1. In the wizard, enter a **Name** for your application, such as `rserver_web`.

   1. For the **Type**, click the **Web Application And/Or Web API**. 

   1. Click the arrow to continue.

   1. Enter the App properties. In the **SIGN-ON URL** box, paste the application URL you copied earlier or something like `http://localhost:12800`. Then, enter that same URL in the **App ID URI** box. 

   1. Click the checkmark to continue and add the application.

   1. Once the application has been added, click the **Configure** tab. 

   1. Copy the **Client ID** for the web app. You will configure your Native application and Microsoft R Server to use this later.

   1. Add a key by selecting a key duration.

   1. Also, take note of the application's tenant id.  The tenant ID is displayed as part of the URL such as: `https://manage.windowsazure.com/tenantname#Workspaces/ActiveDirectoryExtension/Directory/<TenantID>/...` 

   1. Click **Save**. The application is created.

1. Now, create a native app, which will link the web app to the Microsoft R Server operationalization server as follows:

   1. In the **Applications** tab, click **ADD** at the bottom to create a new app registration. A dialog appears.

   1. Click **Add an application my organization is developing**. The **Add Application** wizard appears.

   1. In the wizard, enter a **Name** for your native application, such as `rserver_native`.

   1. For the **Type**, click the **Native Client Application**. 

   1. Click the arrow to continue.

   1. In the **Application Information** dialog, enter `urn:ietf:wg:oauth:2.0:oob` for the **Redirect URI**.

   1. Click the checkmark to continue and add the application.

   1. Once the application has been added, click the **Configure** tab. 

   1. Copy the **Client ID** for the native app. You will use it when enabling AAD in Microsoft R Server in a later step.

   1. Scroll down and click **Add Application** button. A dialog opens in which you can define which apps will have access to the native app.

   1. In the **Permissions to other applications** dialog, enter the name of the web app you created above in the **Starting With** field.

   1. Click the checkmark next to this field to filter the list of apps using the string you entered.

   1. In the list, click the + symbol next to the name of the web app.

   1. Click the checkmark in the bottom right to give the native app permissions to the web application.

   1. Add **Delegated Permissions** to the web app.
     ![Delegated Permissions](../media/o16n/aad-delegated-permissions.png)

   1. Click **Save**. 

**On each Web node, enable Azure AD by doing the following:**

1. Open the configuration file, `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

    1. Search for the section starting with `"AzureActiveDirectory": {`

    1. Uncomment characters in that section and update the properties so that they match the values in the Azure Management portal.  Properties include:

       |Azure AD Properties|Definition|
       |----------------|-------------------------------|
       |`Authority`|Use `https://login.windows.net/<URL to AAD login>` where `<URL to AAD login>` is the URL to the AAD login.|
       |`Audience`|Use the `CLIENT ID` value for the web app you copied from the Azure management portal.|

1. Launch the administrator's utility and:
   1. [Restart the Web node](admin-utility.md#startstop).
   1. Run the [diagnostic tests](admin-utility.md#test).

1. Repeat these steps on each  machine hosting the Web node.


**When authenticating with the `mrsdeploy` package, do the following:**

To authenticate with Azure Active Directory from your R script using the  `mrsdeploy` package, use the `remoteLoginAAD` function.

```
remoteLoginAAD("http://localhost:12800", #SIGN-ON URL value from Web Application
                   authuri = "https://login.windows.net",
                   tenantid = "<AAD_DOMAIN>", #domain of AAD account
                   clientid = "<NATIVE_APP_CLIENT_ID>",  #clientID from AAD Native Application
                   resource = "<WEB_APP_CLIENT_ID>", #clientID from AAD Web Application
                   session = TRUE,
                   diff=TRUE,
                   commandline=TRUE,
                   prompt = "MY-PROMPT> ")  #the remote R session prompt once authenticated 
```

You'll be prompted for your AAD username (`<username>@<AAD-account-domain>`) and password. 