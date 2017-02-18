---

# required metadata
title: "Roles and Groups | Microsoft R Server Docs"
description: "Enterprise-Grade Security: Authentication Roles for Operationalization with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "02/14/2017"
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

# Permissions and roles that limit web service interactions

**Applies to:  Microsoft R Server 9.1.0**


**open questions/things to research:**


+ DESIGN: <https://microsoft.sharepoint.com/teams/immlrevo/_layouts/15/WopiFrame.aspx?sourcedoc=%7b710c6a65-528c-4fa8-8f21-0db75f80117c%7d&action=edit>

+ P2. Roles handled in the admin utility   (is this going to make it in?)

----------------------------

By default, when you configure Microsoft R Server for operationalization, authenticated users can publish, list, and get any web services. Additionally, users can also update and delete their own web services.

You can use roles to further control who can publish, update and delete web services in R Server. There are several standard roles, each of which has different permissions. How users are put assigned to roles depends on what authentication method has been configured for R Server. For more on configuring authentication for R Server, read the article, ["Authentication options for operationalization"](security-authentication.md).

## What do I need?

To assign groups of users in your Active Directory to operationalization roles, you must have:

+ An instance of Microsoft R Server that is [configured for operationalization](../operationalize/configuration-initial.md)

+ Authentication for this instance must be via [Active Directory/LDAP (AD/LADP) or Azure Active Directory (AAD)](../operationalize/security-authentication.md) 

+ The names of the groups that contain the users to whom you want to give special permissions

+ The password for [the local `admin` account](../operationalize/security-authentication.md#local) for operationalization

## Groups versus operationalization roles

In AD/LDAP and AAD, security groups are used to collect user accounts, computer accounts, and other groups into manageable units. Working with groups instead of with individual users helps simplify network maintenance and administration. Your organization might have groups like "Admin", "Engineering", "Level3", and so on. And, users might belong to more than one group.
You can leverage the AD groups you've already defined in your organization to assign a collection of users to R Server operationalization roles. 

In R Server, the administrator can assign one or more Active Directory groups to one or more operationalization roles: "Owner", "Contributor", and "Reader". These roles give specific permissions related to publishing and interacting with web services. When a user attempts to authenticate, R Server will check to see whether you've declared roles for operationalization. If you have, then R Server checks to see to which group the user belongs. If the user belongs to one of the AD/LDAP or AAD groups that you declare in R Server, then the user is authenticated and given permissions according to the role to which their group is assigned. See the following section on **"Role declaration states"** for more information.

A user can belong to multiple groups, and therefore it is possible to be assigned multiple roles and all of their permissions.

## Standard operationalization roles

When roles are enabled, the administrator has the choices of putting groups (of users) into these roles.

|Role |Definition|Can do with<br>web services |Cannot do with<br>web services|
|-------------|------------|-----------------|---------------------|
|`Owner` |When declared, these users can publish<br> and manage any service.|● Publish any service <br>● Update any service <br>● Delete any service <br>● List all services <br>● Consume any service |N/A| 
|`Contributor` |When declared, these users can publish and <br>manage their services, but no one else's.|● Publish any service <br>● Update his/her service <br>● Delete his/her service <br>● List all services <br>● Consume any service|● Update someone else's service<br>● Delete someone else's service| 
|`Reader`|Never declared, this is a catchall role is given <br>to any authenticated user that does not assigned a role <br>when the Contributor role has be declared. <br>These users can only list and consume services.|● List all services<br>● Consume any service|● Publish any service <br>● Update any service <br>● Delete any service|

You only have to configure the roles to the groups that have elevated permissions ("Owner", "Contributor"). The Reader role, on the other hand, is implicitly assigned to any other authenticated user that isn't assigned the "Owner" or "Contributor" role.


## Role declaration states

You can choose from the following states:

1. No roles are explicitly declared (default state after install)
1. Only one role is explicitly declared
1. "Contributor" and "Owner" are explicitly declared

|When these roles are declared|Everyone else is implicitly assigned to:|
|-----|:--------------------:|
|_no roles (default state)_|"Contributor"|
|"Owner" only|"Contributor"|
|"Contributor" only|"Reader"|
|"Contributor" and "Owner"|"Reader"|





## Declaring roles for the local `admin` account

If you only have the default local administrator account, `admin`, set up for R Server's operationalization, then this is the only user and the `admin` user is implicitly assigned to the "Contributor" role.





## Declaring roles for AD/LDAP users

If you configure R Server to [use Active Directory/LDAP authentication](security-authentication.md#ldap), then you can configure it to assign roles using Active Directory groups as follows:

#### Steps

1. On each R Server web node, open the `appsettings.json` configuration file in order to declare the roles and the groups that belong them. 

   + On Windows, this file is under `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\` where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

   + On Linux, this file is under `/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/`.
   
1. Search for the following section: `"LDAP": {`

   >[!NOTE]
   > You cannot have both Azure Active Directory and Active Directory/LDAP enabled at the same time. If one is set to `"Enabled": true`, then the other must be set to `"Enabled": false`.

1. Add the roles to which you want to map AAD groups under the `"AzureActiveDirectory": {` section. You can add one or both roles ("Owner" and "Contributor"). You can map one or more AD groups to each role such as:

   ```"Owner": [ "GroupName1" ],``` 

   ```"Contributor": [ "GroupName2", "GroupName3" ]```

1. In order for R Server to verify that the groups you've declared are valid in AD/LDAP, you must provide the `QueryUserDn` and `QueryUserPassword` in the `"LDAP": {` section. See the example below. This allows R Server to verify that each declared group is, in fact, a valid, existing group in AD.

1. [Restart the web node](admin-utility.md#startstop) for the changes to take effect.

1. Repeat these changes in every web node you've configured.  The configuration must be the same across all web nodes.

#### Example of roles for AD/LDAP

Here is an example of roles declared for AD/LDAP in `appsettings.json` on the web node:

```
 "Authorization": { 
    "AzureActiveDirectory": { 
       "Enabled": false,
       ...
    }, 
    "LDAP": {
       "Enabled": true,
       "Owner": [ "Administrators" ], 
       "Contributor": [ "RProgrammers", "Quality" ] 
       "Values": [
                {
                    "Host": "<host_ip>",
                    "UseLDAPS": "True",
                    "SkipCertificateValidation": "True",
                    "BindFilter": "CN={0},CN=DeployR,DC=TEST,DC=COM",
                    "QueryUserDn": "CN=deployradmin,CN=DeployR,DC=TEST,DC=COM",
                    "QueryUserPasswordEncrypted": true,
                    "QueryUserPassword": 
                    "abcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQR",
                    "SearchBase": "CN=DeployR,DC=TEST,DC=COM",
                    "SearchFilter": "cn={0}"
                }
            ]
        }
      ...
    }  
```

## Declaring roles for AAD users

If you configure R Server to [use Azure Active Directory](security-authentication.md#aad)(AAD), then you can configure it to assign roles using AAD groups as follows:

####Steps 

On each web node, declare the roles in the external JSON configuration file, `appsettings.json` as follows:

1. In the Azure Classic Portal, update the configuration to allow R Server to match a user with his or her groups and authenticate with AAD. To do that, you must:
   1. Sign in to the [Azure classic portal](https://manage.windowsazure.com/) 

   1. In the left menu, choose **Active Directory**.
   1. Select the active directory you want to open.
   1. Once open, click the **Applications** tab at the top.
   1. Open [the web application you created when you configured R Server for AAD authentication](security-authentication.md#aad).
   1. With the application open, click the **Manage Manifest** button at the bottom of the page. A popup menu appears.

      ![Manifest](../media/o16n/security-auth-2.png)
   1. Choose **Download manifest** and save the file locally.
   1. Edit the manifest file in a text editor.  
   1. Ensure that the property `"groupMembershipClaims":` looks like this:  `"groupMembershipClaims": "SecurityGroup"`.
   1. Save the manifest file.
   1. Back in the portal, click  the **Manage Manifest** button at the bottom of the page again.
   1. Choose **Upload Manifest** and upload the edited file back into the portal.
   1. In the **Configure** tab, scroll to the **Keys** section, take note of the key as you'll need to add this to the configuration file `appsettings.json` so R Server can validate the group names at authentication time.  
   1. In the same tab, scroll to the **Permissions to other applications** section. 
   1. Click on the **Delegated Permissions** listbox and make sure that the **Read directory data** checkbox is enabled.

      ![Checkbox](../media/o16n/security-auth-1.png) 

1. On each R Server web node, update the configuration file in order to declare the roles and the groups that belong them. 

   1. Open the `appsettings.json` configuration file.

      + On Windows, this file is under `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\` where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

      + On Linux, this file is under `/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/`.
   
   1. Search for the following section: `"AzureActiveDirectory": {`

      >[!NOTE]
      > You cannot have both Azure Active Directory and Active Directory/LDAP enabled at the same time. If one is set to `"Enabled": true`, then the other must be set to `"Enabled": false`.
   
   1. Add the roles to which you want to map AAD groups under the `"AzureActiveDirectory": {` section. You can add one or both roles ("Owner" and "Contributor"). See example below. You can map one or more AD groups to each role such as:

      ```"Owner": [ "GroupName1" ],``` 

      ```"Contributor": [ "GroupName2", "GroupName3" ]```
 
   1. In order for R Server to verify that the groups you've declared are valid in AAD, you must declare the alphanumberic client key for the application you created in AAD to the `"Key": ` property in the `"AzureActiveDirectory": {` section.  See example below.

      ```"Key": "ABCD000000000000000000000000WXYZ"```
      
      >[!NOTE]
      > For more security, we recommend you [encrypt the key](admin-utility.md#encrypt) before adding the information to `appsettings.json`.

      >[!NOTE]
      > If a given user belongs to more than groups that allowed in AAD (overage limit), AAD will provide an overage claim in the token it returns. This claim along with the key you provide here allows R Server to retrieve the group memberships for the user.

1. [Restart the web node](admin-utility.md#startstop) for the changes to take effect.

1. Repeat these changes in every web node you've configured.  The configuration must be the same across all web nodes.


#### Example of roles for AAD

Here is an example of roles declared for AAD in `appsettings.json` on the web node:
```
 "Authorization": { 
    "AzureActiveDirectory": { 
       "Enabled": true,
       "Owner": [ "Administrators" ], 
       "Contributor": [ "RProgrammers", "Quality" ] 
       "Values": [ 
          { 
             "Authority": "https://login.windows.net/rserver.contoso.com", 
             "Audience": "00000000-0000-0000-0000-000000000000" 
           } 
        ]
      "Key": "ABCD000000000000000000000000WXYZ"   
    }, 
    "LDAP": {
       "Enabled": false,
      ...
    }  
```   
