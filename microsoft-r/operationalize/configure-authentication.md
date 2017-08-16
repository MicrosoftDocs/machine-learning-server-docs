---

# required metadata
title: "LDAP AD and Azure Active Directory authentication for Machine Learning Server | Microsoft Docs"
description: "Enterprise-Grade Security: Authentication with Microsoft R Server"
keywords: "R Server LDAP-S, LDAP, AD, Azure Active Directory, AAD, Azure AD, Authentication, Microsoft R Server"
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

# Authenticate R Server users against LDAP AD or Azure Active Directory

**Applies to:  Microsoft R Server 9.x**

R Server's offers seamless integration with authentication solutions when configured to operationalize analytics.

![Security](./media/configure-authentication/security.png)

To secure connections and communications, you have several options:

|Authentication Method|When to Use|
|----------------------------------|----------------------------------|
|[Local 'admin' account](#local)|For [one-box](../install/operationalize-r-server-one-box-config.md) configurations|
|[Active Directory / LDAP](#ldap)|For [enterprise](../install/operationalize-r-server-enterprise-config.md) on-premises configurations|
|[Active Directory / LDAP-S](#ldap)|For [enterprise](../install/operationalize-r-server-enterprise-config.md) on-premises configurations with SSL/TLS enabled|
|[Azure Active Directory](#aad)|For [enterprise](../install/operationalize-r-server-enterprise-config.md) cloud configurations|

<a name="local"></a>

## Local Administrator Account Authentication

During configuration, a default administrator account, 'admin', is created to manage the web and compute nodes for R Server. This account allows you to use the [administration utility](configure-use-admin-utility.md) to configure this feature, edit ports, restart nodes, and so on. 

While this account might be sufficient when trying to operationalize with a [one-box configuration](../install/operationalize-r-server-one-box-config.md#onebox) since everything is running within the trust boundary, it is insufficient for [enterprise configurations](../install/operationalize-r-server-enterprise-config.md).

To set or change the password for the local administrator account after the configuration script has been run, [follow these steps](configure-use-admin-utility.md#admin-password).

To log in to Microsoft R Server with this user for remote execution or web service functionalities, use remoteLogin() as described in the article "[Connecting to R Server with mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)."


>[!WARNING]
> If you enable Azure Active Directory or Active Directory/LDAP authentication, this 'admin' account can no longer be used to authenticate with R Server.


<a name="ldap"></a>

## Active Directory and LDAP/LDAP-S

Active Directory (AD) and LDAP are a great authentication option for on-premises configurations to ensure that domain users have access to the APIs.  

LDAP is the standard protocol for reading data from and writing data to Active Directory (AD) domain controllers. AD LDAP traffic is unsecured by default, which makes it possible to use network-monitoring software to view the LDAP traffic between clients and domain controllers.  

By default, the LDAP security provider is not configured. To enable LDAP authentication support, update the relevant properties in your configuration file. The values you assign to these properties must match the configuration of your LDAP Directory Information Tree (DIT).

You can make LDAP traffic confidential and secure using Secure Sockets Layer (SSL) / Transport Layer Security (TLS) technology. This combination is referred to as LDAP over SSL (or LDAP-S). To ensure that no one else can read the traffic, SSL/TLS establishes an encrypted tunnel between an LDAP client and a domain controller. [Learn more about enabling SSL/TLS.](configure-https.md) Reasons for enabling LDAP-S include:

+ Organizational security policies typically require that all client/server communication is encrypted.
+ Applications use simple BIND to transport credentials and authenticate against a Domain Controller. As simple BIND exposes the users’ credentials in clear text, using SSL/TLS to encrypt the authentication session is recommended.
+ Use of proxy binding or password change over LDAP, which requires LDAP-S. Bind to an AD LDS instance Through a Proxy Object
+ Applications that integrate with LDAP servers (such as Active Directory or Active Directory Domain Controllers) might require encrypted LDAP communications.

>[!WARNING]
> You cannot have both Azure Active Directory and Active Directory/LDAP enabled at the same time. If one is set to `"Enabled": true`, then the other must be set to `"Enabled": false`.

**On each web node:**

1. Enable LDAP/LDAP-S in the external JSON configuration file, appsettings.json:

   1. [Open the appsettings.json configuration file](configure-find-admin-configuration-file.md).

   1. Search for the section starting with `"LDAP": {`
   
   1. <a name="encrypt"></a>Enable this section and update the properties so that they match the values in your Active Directory Service Interfaces Editor.  

      > For better security, we recommend you [encrypt the password](configure-use-admin-utility.md#encrypt) before adding the information to appsettings.json.

   |LDAP Properties|Definition|
   |---------------|-------------------------------|
   |Host|Address of the Active Directory server|
   |Port|(version 9.1+) Used to override the default LDAP port. By default, the LDAP port is 389 and the LDAP-S port is 636.|      
   |UseLDAPS|Set 'true' for LDAP-S or 'false' for LDAP<br>**Note:** If LDAP-S is configured, an installed LDAP service certificate is assumed so that the tokens produced by Active Directory/LDAP can be signed and accepted by R Server. |
   |BindFilter|(version 9.0.1 only) The template used to do the Bind operation. For example, "CN={0},CN=DeployR,DC=TEST,DC=COM". {0} is the user's DN.|
   |QueryUserDn|Distinguished name of user with read-only query capabilities with which to authenticate|
   |QueryUserPassword|Password for that user with which to authenticate (value must be encrypted).  We highly recommend that you [encrypt LDAP login credentials](configure-use-admin-utility.md#encrypt) before adding the information to this file.|
   |QueryUserPasswordEncrypted|True/False. If 'True', it means the value of QueryUserPassword is an encrypted string.|
   |SearchBase|Context name to search in, relative to the base of the configured ContextSource, for example, 'ou=users,dc=example,dc=com'.| 
   |SearchFilter|The pattern to be used for the user search. "SearchFilter": "cn={0}" is for each user's DN. In legacy systems, some use "SearchFilter": "sAMAccountName={0}"|
   |UniqueUserIdentifierAttributeName|(Version 9.1) The attribute name that stores the unique user id for each user. If you are configuring roles, you must ensure that the username returned for this value matches the username returned by SearchFilter. For example, if "SearchFilter": "cn={0}" and "UniqueUserIdentifierAttributeName": "userPrincipalName", then the values for cn and userPrincipalName must match.|
   |DisplayNameAttributeName|(Version 9.1) The attribute name that stores the display name for each user.|
   |EmailAttributeName|(Version 9.1) The attribute name that stores the email address for each user.|

   >[!IMPORTANT]
   >The entities created by the users, specifically web services and [session snapshots](../r/how-to-execute-code-remotely.md#snapshot), are tied to their usernames. For this reason, you must be careful to prevent changes to the user identifier over time. Otherwise, pre-existing web services and snapshots cannot be mapped to the users who created them.
   >
   >For this reason, we strongly recommend that you DO NOT change the unique LDAP identifier in appsettings.json once users start publishing service or creating snapshots. 
   >
   >Similarly, if your organization changes its usernames, those users can no longer access the web services and snapshots they created unless they are [assigned to the `Owner` role](configure-roles.md).  

   <br>

   >[!WARNING]
   >For 9.0.1 Users! The unique identifier is always set to the `userPrincipalName` in version 9.0.1. Therefore, make sure that a value is defined for the `userPrincipalName` in the Active Directory Service Interfaces Editor or the authentication may fail.  In the Explorer, connect to the domain controller, find the user to authorize, and then make sure that the value for the  UserPrincipalName (UPN) property is not null.

   For example, with R Server 9.1:
   ```
   "LDAP": {
           "Enabled": true,
           "Host": "<host_ip>",
           "Port": "<port_number>"
           "UseLDAPS": "True",
           "QueryUserDn": "CN=deployradmin,CN=DeployR,DC=TEST,DC=COM",
           "QueryUserPasswordEncrypted": true,
           "QueryUserPassword": "ABCD00000123400000000000mnop00000000WXYZ",
           "SearchBase": "CN=DeployR,DC=TEST,DC=COM",
           "SearchFilter": "cn={0}"  
           "UniqueUserIdentifierAttributeName": "userPrincipalName",
           "DisplayNameAttributeName": "name",
           "EmailAttributeName": "mail"     
   }
   ```
   
   <br>
      
   >[!NOTE]
   >Need help figuring out your Active Directory/LDAP settings? Check out your LDAP settings using the `ldp.exe` tool and compare them to what you’ve declared in appsettings.json.  You can also consult with any Active Directory experts in your organization to identify the correct parameters.

1. To set different levels of permissions for users interacting with web services, [assign them roles](configure-roles.md). <a name="ldap-jwt"></a>

1. If using a certificate for access token signing, you must: 

   >[!Important]
   >You must use a certificate for access token signing whenever you have multiple web nodes so the tokens are signed consistently by every web node in your configuration. 
   >
   >In production environments, we recommend that you use a certificate with a private key to sign the user access tokens between the web node and the LDAP server.
   >
   >Tokens are useful to the application developers who use them to identify and authenticate users who are sending API calls within their application. [Learn more...](how-to-manage-access-tokens.md)
   >
   >**Every web node must have the same values**.
    
   1. On each machine hosting the Web node, install the trusted, signed **access token signing certificate** with a private key in the certificate store. Take note of the `Subject` name of the certificate as you need this information later.  Read [this blog post](https://blogs.msdn.microsoft.com/rserver/2017/05/19/using-certificates-in-r-server-operationalization-for-linux/) to learn how to use a self-signed certificate in Linux for access token signing.

   1. In the appsettings.json file, search for the section starting with `"JWTSigningCertificate": {`

   1. Enable this section and update the properties so that they match the values for your token signing certificate:
      ```
      "JWTSigningCertificate": {
          "Enabled": true,
          "StoreName": "My",
          "StoreLocation": "CurrentUser",
          "SubjectName": "CN=<subject name>"
      }
      ```
1. Save changes to appsettings.json. 

1. [Restart the web node](configure-use-admin-utility.md#startstop) using the administration utility so that the changes can take effect. 
 
1. Run the [diagnostic tests](configure-run-diagnostics.md) to ensure all tests are passing in the configuration.

   >[!IMPORTANT]
   >If you run into connection issues when configuring for Active Directory/LDAP, try the ldp.exe tool to search the LDAP settings and compare them to what you declared in appsettings.json.  To identify the correct parameters, consult with any Active Directory experts in your organization.

1. Repeat these steps on each machine hosting the web node.

1. Share the connection details with any users who authenticate with R Server either to make [API calls](concept-api.md) directly or indirectly in R [using remoteLogin() function in the mrsdeploy package](how-to-connect-log-in-with-mrsdeploy.md).


<br>

<a name="aad"></a>

## Azure Active Directory 

[Azure Active Directory (AAD)](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory) can be used to securely authenticate  in the cloud when the client application and Web node have access to the internet.

**Step 1: Log in to the Azure classic portal**

1. Sign in to the [Azure classic portal](https://manage.windowsazure.com/) and navigate to **Active Directory** in the left-hand pane. Copy the URL in your browser address bar. You use this URL to configure your Azure Active Directory app.

1. Select your directory. If the Azure Active Directory has not been set up yet, contact your system administrator.  

1. Select the **Applications** tab at the top. 

**Step 2: Create a web application**

Now, create a web app that is tied to the Azure Active Directory as follows: 

   1. In the **Applications** tab, click **ADD** at the bottom to create an app registration. A dialog appears.
 
   1. Click **Add an application my organization is developing**. The **Add Application** wizard appears.

   1. In the wizard, enter a **Name** for your application, such as `R Server Web app`.

   1. For the **Type**, click the **Web Application And/Or Web API**. 

   1. Click the arrow to continue.

   1. Enter the App properties. In the **SIGN-ON URL** box, paste the application URL you copied earlier; or if you expect the R client to be on the same machine as the server, use http://localhost:12800. Then, enter that same URL in the **App ID URI** box. 

   1. Click the checkmark to continue and add the application.

   1. After the application has been added, click the **Configure** tab. 
      ![Configure web application](./media/configure-authentication/webapp1.png)

   1. Copy the **Client ID** for the web app. Later, you configure your Native application and Microsoft R Server with this ID.

   1. Add a client key to the web app's **Keys** section by selecting a key duration and take note of the key. 
   
      >[!IMPORTANT] 
      > Take note of this key as it is needed to configure [roles to give web services permissions to certain users](configure-roles.md). See following example.

      ![Configure web application](./media/configure-authentication/webapp2.png)

   1. Also, take note of the application's tenant id.  The tenant ID is the domain of the Azure Active Directory account, for example,  `myMRServer.contoso.com`.

   1. Click **Save**. The application is created.

**Step 3: Create a native application**

Now, create a native app. This app links the web app to the Microsoft R Server web node.

   1. In the **Applications** tab, click **ADD** at the bottom to create an app registration. A dialog appears.

   1. Click **Add an application my organization is developing**. The **Add Application** wizard appears.

   1. In the wizard, enter a **Name** for your native application, such as `R Server Native app`.

   1. For the **Type**, click the **Native Client Application**. 

   1. Click the arrow to continue.

   1. In the **Application Information** dialog, enter `urn:ietf:wg:oauth:2.0:oob` for the **Redirect URI**.

   1. Click the checkmark to continue and add the application.

   1. After the application has been added, click the **Configure** tab. 

   1. Copy the **Client ID** for the native app. Later, you use this ID to enable AAD in Microsoft R Server.

   1. Scroll down and click **Add Application** button. A dialog opens in which you can define which apps can access the native app.

   1. In the **Permissions to other applications** dialog, enter the name of the web app you created before in the **Starting With** field.

   1. Click the checkmark next to this field to filter the list of apps using the string you entered.

   1. In the list, click the + symbol next to the name of the web app.

   1. Click the checkmark in the bottom right to give the native app permissions to the web application.

   1. Add **Delegated Permissions** to the web app.
     ![Delegated Permissions](./media/configure-authentication/aad-delegated-permissions.png)

   1. Click **Save**. 

**Step 4: Enable Azure AD on each web node**

1. [Open the appsettings.json configuration file](configure-find-admin-configuration-file.md).

1. Search for the section starting with:
   ```
   "AzureActiveDirectory": {
      "Enabled": false,
   ```

   >[!WARNING]
   > You cannot have both Azure Active Directory and Active Directory/LDAP enabled at the same time. If one is set to `"Enabled": true`, then the other must be set to `"Enabled": false`.

1. Enable Azure Active Directory as the authentication method:  `"Enabled": true,`

1. Update the other properties in that section so that they match the values in the Azure portal.  Properties include:

   |Azure AD Properties|Definition|
   |----------------|-------------------------------|
   |Enabled|To use AAD for authentication, set to 'true'. Else, set to 'false'.|
   |Authority|Use 'https://login.windows.net/<URL to AAD login>' where <URL to AAD login> is the URL to the AAD login. For example, if the AAD account domain is myMRServer.contoso.com, then the Authority would be 'https://login.windows.net/myMRSServer.contoso.com'|
   |Audience|Use the CLIENT ID value for the WEB app you created in the Azure portal.|
   |ClientId|Use the CLIENT ID value for the NATIVE app you created in the Azure portal.|
   |Key|This is the key for the WEB application you took note of before.  |
   |KeyEncrypted|We highly recommend that you [encrypt login credentials](configure-use-admin-utility.md#encrypt) before adding the information to this file. Set KeyEncrypted to 'true' if using encrypted information. For plain text, set to 'false'.|

   For example:
   ```
   "AzureActiveDirectory": {
      "Enabled": true,
      "Authority": "https://login.windows.net/myMRServer.contoso.com",
      "Audience": "00000000-0000-0000-0000-000000000000",
      "ClientId": "00000000-0000-0000-0000-000000000000",
      "Key": "ABCD000000000000000000000000WXYZ", 
      "KeyEncrypted": true
    }
   ```

1. To set different levels of permissions for users interacting with web services, [assign them roles](configure-roles.md).

1. Launch the administrator's utility and:
   1. [Restart the web node](configure-use-admin-utility.md#startstop) for the changes to take effect.
 
   1. Test the configuration by running the [diagnostic tests](configure-run-diagnostics.md).

1. Repeat these steps on each machine hosting a web node.

**Step 5: Share the required AAD connection details with your users**

Share the connection details, such as the Authority and Audience, with any users who need to authenticate with R Server to make [API calls](concept-api.md) directly or indirectly in R [using remoteLoginAAD() function in the mrsdeploy package](how-to-connect-log-in-with-mrsdeploy.md#aad-arguments). 

If you do not specify a username and password as arguments to the login call or R functions, you are prompted for your AAD username (<username>@<AAD-account-domain>) and password. 

>[!IMPORTANT]
>Learn how to authenticate with AAD using the remoteLoginAAD() function in the mrsdeploy R package as described in this article: "[Connecting to R Server with mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)."

## See also

* [Blog article: Step-by-step setup for LDAPS on Windows Server](https://blogs.msdn.microsoft.com/rserver/2017/04/10/step-by-step-guide-to-setup-ldaps-on-windows-server/)