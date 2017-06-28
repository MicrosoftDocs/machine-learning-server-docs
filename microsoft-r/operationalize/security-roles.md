---

# required metadata
title: "Control web services permissions with roles - Microsoft R Server | Microsoft Docs"
description: "Owner, contributor, reader authentication roles with Microsoft R Server"
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

# Roles to control web service permissions

**Applies to:  Microsoft R Server 9.1**

By default, authenticated R Server  users can publish, list, and get any web services. Additionally, users can also update and delete the web services they've published.

You can use roles to further control who can publish, update and delete web services in R Server. There are several standard roles, each of which has different permissions. How users are put assigned to roles depends on what authentication method has been configured for R Server. For more on configuring authentication for R Server, read the article, ["Authentication options"](configure-authentication.md).

>[!IMPORTANT]
>**This is not the same as RBAC in Azure Active Directory.** While the default roles described here-in bear the same names as the roles you can define in Azure, it is not possible to inherit the Azure roles. If you want role-based access control over web services, you must set up roles again.

## What do I need?

To assign groups of users in your Active Directory to R Server roles for web services, you must have:

+ An instance of Microsoft R Server that is [configured to operationalize analytics](../install/operationalize-r-server-enterprise-config.md)

+ Authentication for this instance must be via Active Directory/LDAP (AD/LADP) or Azure Active Directory (AAD) and [already configured](../operationalize/configure-authentication.md)

+ The names of the groups that contain the users to whom you want to give special permissions

## Groups versus roles for web services

In AD/LDAP and AAD, security groups are used to collect user accounts, computer accounts, and other groups into manageable units. Working with groups instead of with individual users helps simplify network maintenance and administration. Your organization might have groups like "Admin", "Engineering", "Level3", and so on. And, users might belong to more than one group.
You can leverage the AD groups you've already defined in your organization to assign a collection of users to roles for web services. 

In R Server, the administrator can assign one or more Active Directory groups to either the "Owner" or "Contributor" roles or both. Roles give specific permissions related to publishing and interacting with web services:
+ `Owner`: users assigned to this role can manage any service.
+ `Contributor`: users assigned to this role can publish and manage their services. They cannot manage the services' of others.
+ `Reader`: a catchall role implicitly given to any authenticated user that is not assigned another role. It is never explicitly declared. See next table. These users can only list and consume services.

A user can belong to multiple groups, and therefore it is possible to be assigned multiple roles and all of their permissions.

When a user attempts to authenticate, R Server will check to see whether you've declared roles for web service interactions. If you have, then R Server checks to see to which group the user belongs based on the action you are trying to perform. If the user belongs to one of the AD/LDAP or AAD groups that you declare in R Server, then the user is authenticated and given permissions according to the role to which their group is assigned. See the following section on **"Role configuration states"** for more information.


## Roles for web service interactions

When roles are declared in the configuration file, the administrator has the choices of putting groups (of users) into these roles.

|Role |Can do with<br>web services |Cannot do with<br>web services|
|-------------|------------|-----------------|---------------------|
|`Owner` |● Publish any service, including new versions<br>&nbsp;&nbsp; of web services published by someone else <br>● Update any service <br>● Delete any service <br>● List all services <br>● Consume any service |N/A| 
|`Contributor` |● Publish any service, including new versions<br>&nbsp;&nbsp; of web services published by someone else <br>● Update their services <br>● Delete their services <br>● List all services <br>● Consume any service|● Update service published by someone else<br>● Delete service published by someone else| 
|`Reader`|● List all services<br>● Consume any service|● Publish any service <br>● Update any service <br>● Delete any service|

## Role configuration states

You can choose from the following states:

1. No roles are explicitly declared (default state after install)
1. Only one role is explicitly declared
1. "Contributor" and "Owner" are explicitly declared

<br>

|When these roles are declared|Everyone else is implicitly assigned to:|
|-----|:--------------------:|
|_no roles (default state)_|"Contributor"|
|"Owner" only|"Contributor"|
|"Contributor" only|"Reader"|
|"Contributor" and "Owner"|"Reader"|


## Web service permissions after role change

A user might change roles because they no longer belong to the same security group in AD/LDAP or AAD, or perhaps that security group is no longer mapped to an R Server role in the `appsettings.json` file anymore. 

Whenever a user's role changes, that user may not longer be able to perform the same tasks on their web services. If you publis a web service while assigned to the "Owner" role, then you can continue to update, delete and interact with that web service version as long as you are assigned this role. However, if you are reassigned to the "Contributor" role, then you still be allowed to interact with that web service version as you did before, but you won't be allowed to update or delete the services published by others. Now, if roles are defined for users, but you are no longer assigned to one of those roles, you become part of the "Reader" role implicitly and can no longer manage any services, including those that you published previously when you had another role. 

## Declaring roles for the local 'admin' account

If you only have the default local administrator account, 'admin', defined for R Server, then this is the only user and the 'admin' user is implicitly assigned to the "Contributor" role.

## Declaring roles for AD/LDAP and Azure AD users

If you configure R Server to [use Active Directory/LDAP or Azure Active Directory authentication](configure-authentication.md), then you can configure it to assign roles using Active Directory groups as follows:

#### Step 1. Add the roles to R Server on each web node

On each R Server web node, edit the `appsettings.json` configuration file in order to declare the roles and the groups that belong them. 

1. Open [the `appsetting.json` file](admin-configuration-file.md).

1. Search for the following section: `"Authorization": {`

1. In that section, add one or both roles ("Owner" and "Contributor"). Then, map one or more security groups to each R Server role such as:

   ```"Authorization": {```<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;```"Owner": [ "Administrators" ],```<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;```"Contributor": [ "RProgrammers", "Quality" ]```<br>```}``` 

   >[!WARNING]
   >For AD/LDAP authentications:
   >1. Be careful not to use the `CN=` portion of the distinguished names. For example, if the distinguished name appears as `CN=Administrators`, enter only `Administrators` here.
   
   >2. Ensure that the username returned for the value of `UniqueUserIdentifierAttributeName` matches the username returned by `SearchFilter`. For example, if `"SearchFilter": "cn={0}"` and `"UniqueUserIdentifierAttributeName": "userPrincipalName"`, then the values for `cn` and `userPrincipalName` must match.
   
<!--#### Step 2. Allow R Server to check groups in Azure Active Directory

R Server must be given the ability to verify the groups you declare against those in AAD or and AD/LDAP.  For AAD, you have an extra step to make that possible. For AD/LDAP, the default settings when you [set up R Server for AD/LDAP](configure-authentication.md#ldap) as sufficient.

1. Sign in to the [Azure classic portal](https://manage.windowsazure.com/) and update the configuration to allow R Server to match a user with his or her groups and authenticate with AAD as follows:

1. In the left menu, choose **Active Directory**.

1. Select the active directory you want to open.

1. After open, click the **Applications** tab at the top.

1. Open the web application you created when you [configured R Server for AAD authentication](configure-authentication.md#aad).

1. With the application open, go to the bottom of the page and click the **Manage Manifest > Download Manifest**. A popup menu appears.
   ![Manifest](../media/o16n/security-auth-2.png)

1. Open the manifest file in a text editor and ensure that the property `"groupMembershipClaims"` looks like this:

   ```"groupMembershipClaims": "SecurityGroup"```

1. Save the manifest file.

1. Back in the portal, click the **Manage Manifest > Upload Manifest** on the toolbar at the bottom of the window. Upload the edited file back into the portal.

1. In the **Configure** tab, scroll to the **Keys** section. Take note of the key as you must add this to the `"AzureActiveDirectory"` section of the `appsettings.json` configuration file. This will enable R Server to validate the group names at authentication time.  

1. In the same tab, scroll to the **Permissions to other applications** section and click the **Delegated Permissions** listbox. and make sure that the **Read directory data** checkbox is enabled.

   ![Checkbox](../media/o16n/security-auth-1.png) -->

#### Step 2. Validate the groups against AD/LDAP or AAD.

Return to [the `appsetting.json` file](admin-configuration-file.md) and do the following:

+ **For Azure Active Directory:** In `appsettings.json`, find the `"AzureActiveDirectory"` section. Make sure the alphanumberic client key you created in the portal **for the web app** is used for `"Key": ` property. This key allows R Server to verify that the groups you've declared are valid in AAD. See example below. Learn more about [configuring R Server user to authenticate with Azure Active Directory](configure-authentication.md#aad).

  >[!IMPORTANT]
  > For more security, we recommend you [encrypt the key](admin-utility.md#encrypt) before adding the information to `appsettings.json`.

  >[!NOTE]
  > If a given user belongs to more than groups that allowed in AAD (overage limit), AAD will provide an overage claim in the token it returns. This claim along with the key you provide here allows R Server to retrieve the group memberships for the user.

+ **For Active Directory/LDAP:** In `appsettings.json`, find the `"LDAP"` section.  In order for R Server to verify that the groups you've declared are valid in AD/LDAP, you must provide the `QueryUserDn` and `QueryUserPassword` in the `"LDAP"` section. See the example below. This allows R Server to verify that each declared group is, in fact, a valid, existing group in AD. Learn more about [configuring R Server user to authenticate with Active Directory/LDAP](configure-authentication.md#ldap).


#### Step 3. Apply the changes to R Server

1. [Restart the web node](admin-utility.md#startstop) for the changes to take effect. You'll need to log in  using [the local 'admin' account](../operationalize/configure-authentication.md#local) in the administration utility.

1. Repeat these changes in every web node you've configured.  The configuration must be the same across all web nodes.

### Example

Here is an example of roles declared for AD/LDAP in `appsettings.json` on the web node:

```
Authentication: {
       "AzureActiveDirectory": {
              "Enabled": false,
              "Authority": "https://login.windows.net/rserver.contoso.com",
              "Audience": "00000000-0000-0000-0000-000000000000",
              "Key": "ABCD000000000000000000000000WXYZ"  
       },
       "LDAP": {
              "Enabled": true,
              "Host": "<host_ip>",
              "UseLDAPS": "True",
              "BindFilter": "CN={0},CN=DeployR,DC=TEST,DC=COM",
              "QueryUserDn": "CN=deployradmin,CN=DeployR,DC=TEST,DC=COM",
              "QueryUserPasswordEncrypted": true,
              "QueryUserPassword":
"abcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQR",
              "SearchBase": "CN=DeployR,DC=TEST,DC=COM",
              "SearchFilter": "cn={0}"       
       }
}
 
"Authorization": {
   "Owner": [ "Administrators" ],
   "Contributor": [ "RProgrammers", "Quality" ]      
}
```

## See Also

+ [How to publish and manage web services in R](data-scientist-manage-services.md)

+ [How to interact with and consume web services in R](howto-consume-web-service-interact-in-r.md)

+ [Authentication options for R Server when operationalizing analytics](configure-authentication.md)

+ [Blog article: Role Based Access Control With MRS 9.1.0](https://blogs.msdn.microsoft.com/rserver/2017/04/10/role-based-access-control-with-mrs-9-1-0/)
