---

# required metadata
title: "Control web services permissions with roles - Machine Learning Server "
description: "Owner, contributor, reader authentication roles with Machine Learning Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "9/25/2017"
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

# Roles and permissions (RBAC)

**Applies to:  Machine Learning Server, Microsoft R Server 9.1**

In Machine Learning Server (and R Server), Role-Based Access Control (RBAC) enables fine-grained access management for the operationalization APIs. Using RBAC, you can grant only the level of access that users need to perform their jobs. This article helps you get up and running with RBAC. 

By default, all authenticated users can publish/deploy, list, and get any web services as well as call all APIs. Additionally, users can also update and delete the web services they have deployed. Use the roles defined in this article to restrict who can call the APIs and publish, update, and delete web services. 

How users are assigned to roles depends on the authentication method configured for Machine Learning Server. For more on configuring authentication, read the article, ["Authentication options."](configure-authentication.md)

>[!IMPORTANT]
>**These roles are not the same as RBAC in Azure Active Directory.** While the default roles described here-in bear the same names as the roles you can define in Azure, it is not possible to inherit the Azure roles. If you want role-based access control over web services and APIs, you must set up roles again in Machine learning server.

## What do I need?

To assign groups of users in your Active Directory to Machine Learning Server roles, you must have:

+ An instance of Machine Learning Server that is [configured to operationalize analytics](configure-start-for-administrators.md#configure-server-for-operationalization)

+ Authentication for this instance must be via Active Directory/LDAP (AD/LADP) or Azure Active Directory (AAD) and [already configured](configure-authentication.md)

+ The names of the groups that contain the users to whom you want to give special permissions

## Security groups versus Machine Learning Server roles

In AD/LDAP and AAD, security groups are used to collect user accounts, computer accounts, and other groups into manageable units. Working with groups instead of with individual users helps simplify network maintenance and administration. Your organization might have groups like 'Admin', 'Engineering', 'Level3', and so on. And, users might belong to more than one group.
You can use the AD groups you have already defined in your organization to assign a collection of users to roles for web services. 

>[!Warning]
>Security group names must be unique across your LDAP/AAD configuration in order to be assigned to a Machine Learning Server role. If a group in LDAP or AAD bears the same name as another group in that LDAP or AAD directory, then it cannot be assigned to a role in Machine Learning Server or R Server.

In Machine Learning Server, the administrator can assign one or more Active Directory groups to one or more of the following roles: 'Owner', 'Contributor', and 'Reader'. Roles give specific permissions related to deploying and interacting with web services and other APIs. 

|||
|-------------|------------| 
|- Owner (highest permissions) <br>-&nbsp;Contributor&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>- Reader|![Checkbox](./media/configure-roles/role-hierarchy.png)|



## Roles and their permissions

When roles are declared in the configuration file, the administrator has the choices of putting groups (of users) into these roles.

|Role |Description|Permitted|Prohibited|
|-------------|------------|-----------------|---------------------|
|Owner|These users can manage any service and call any API, including centralized configuration APIs.|Web service APIs:<br>&nbsp;&#x2714; Publish **any** service<br>&nbsp;&#x2714; Update **any** service <br>&nbsp;&#x2714; Delete **any** service <br>&nbsp;&#x2714; List **any** service <br>&nbsp;&#x2714; Consume **any** service<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>Other APIs:<br>&nbsp;&#x2714; Call **any** other API|No API restrictions<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;| 
|Contributor|These users can publish/deploy services and manage the services they publish. They can also call most other APIs. |Web service APIs:<br>&nbsp;&#x2714; Publish **any** service <br>&nbsp;&#x2714; Update their services <br>&nbsp;&#x2714; Delete their services <br>&nbsp;&#x2714; List **any** service <br>&nbsp;&#x2714; Consume **any** service<br><br>Other APIs:<br>&nbsp;&#x2714; Call almost any other API|<br>&nbsp;&#x2716; Update service published by another<br>&nbsp;&#x2716; Delete service published by another<br>&nbsp;<br>&nbsp;<br>&nbsp;&#x2716; Centralized node configuration (v9.2+)| 
|Reader|In Machine Learning Server 9.2+, these users can list and consume any service and call most other APIs.<br><br>In R Server 9.1, this catchall role is implicitly given to any authenticated user  not assigned a role. Users can list and consume services. Never explicitly declared. |Web service APIs:<br>&nbsp;&#x2714; List **any** service<br>&nbsp;&#x2714; Consume **any** service<br><br>Other APIs:<br>&nbsp;&#x2714; Call almost any other APIs|<br>&nbsp;&#x2716; Publish **any** service <br>&nbsp;&#x2716; Update **any** service <br>&nbsp;&#x2716; Delete **any** service<br>&nbsp;<br>&nbsp;&#x2716; Centralized node configuration (v9.2+)|


## How are roles assigned

When a user calls a Machine Learning Server API, the server checks to see whether any roles were declared. When roles are declared, Machine Learning Server checks to see to which group the user belongs. 

If the user belongs to an AD/LDAP or AAD group assigned to a role, then that user is  given permissions according to their role.  If a user belongs to groups that are assigned to multiple roles, then the user is assigned to the role with the highest permissions. 

Here is an example of different LDAP group configurations and the resulting roles assigned to the persona.

|Example User <br>/ Persona|User's <br>LDAP Groups||Machine&nbsp;Learning&nbsp;Server<br>RBAC Configuration|User's<br>Role|
|:-------------:|:------------:|:-:|------------|:------------:| 
|![Checkbox](./media/configure-roles/p1.png)<br>Administrator|**admins**<br>engineering<br>FTE-north|+|"Owner":&nbsp;["**admins**",&nbsp;"managers"],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=<br>"Contributor": ["stats"]|**Owner**|
|![Checkbox](./media/configure-roles/p2.png)<br>Lead data scientist|**managers**<br>**stats**<br>FTE-north|+|"Owner": ["admins", "**managers**"],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=<br>"Contributor": ["**stats**"],<br>"Reader": ["app-devs"]|**Owner**|
|![Checkbox](./media/configure-roles/p2.png)<br>R programmer|**stats**<br>FTE-north|+|"Owner": ["admins", "managers"],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=<br>"Contributor": ["**stats**"],<br>"Reader": ["app-devs"]|**Contributor**|
|![Checkbox](./media/configure-roles/da-persona.png)<br>Python developer|stats<br>FTE-north|+|"Owner": ["admins", "managers"]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=|**Contributor**|
|![Checkbox](./media/configure-roles/p3.png)<br>Application&nbsp;Developer|**app-devs**<br>FTE-north|+|"Owner": ["admins", "managers"],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=<br>"Contributor": ["stats"],<br>"Reader": ["**app-devs**"]|**Reader**|
|![Checkbox](./media/configure-roles/p3.png)<br>System Integrator|vendor2|+|"Owner": ["admins", "managers"],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=<br>"Contributor": ["stats"]|**Reader**|
|![Checkbox](./media/configure-roles/p4.png)<br>Sales|sales|+|"Owner": ["admins", "managers"],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=<br>"Contributor": ["stats"]<br>"Reader": ["app-devs"]|no role or permissions|

## Role configuration states

The permissions assigned to users are influenced not only by the roles you define, but also by the roles you **do not** define. When one or more roles is not defined, then certain permissions are implicitly assumed. By default, no roles are declared. 

The table shows which permissions are granted to those authenticated users who are not explicitly assigned to any role. 
<br>
<a name="configroles"></a>

|Possible configurations|Users without a role are implicitly assigned to|
|-----|:--------------------:|
|- No roles: RBAC not configured|Contributor|
|- Owner only|Contributor|
|- Contributor + Owner &nbsp;_-or-_<br>- Contributor only|Reader|
|- Reader + Contributor + Owner &nbsp;_-or-_<br>- Reader + Owner &nbsp;_-or-_<br>- Reader only|v9.2+:  all API access denied<br>v9.1: not applicable since Reader is never declared|

## How to declare roles

If you configure Machine Learning Server (or R Server) to [use Active Directory/LDAP or Azure Active Directory authentication](configure-authentication.md), then you can assign roles using Active Directory groups as described here.

>[!Note]
>If only the default local administrator account is defined for Machine Learning Server (or R Server), then roles are not needed. In this case, the 'admin' user is implicitly assigned to the Owner role (can call any API).

### Step 1. Define desired roles

On each web node, edit the appsettings.json configuration file in order to declare the roles and the groups that belong to them. 

1. Open the configuration file, \<web-node-install-path>/appsettings.json. (Find the [install path](../operationalize/configure-find-admin-configuration-file.md) for your version.) 

1. Search for the following section: `"Authorization": {`

1. In that section, add only the roles you need. Then, map the security groups to each role you want to define such as:
   ```
   "Authorization": {
     "Owner": [ "Administrators" ],
     "Contributor": [ "RProgrammers", "Quality" ],
     "Reader": [ "App developers" ],
     "CacheLifeTimeInMinutes": 60
   }
   ``` 

   The 'CacheLifeTimeInMinutes' attribute was added in Machine Learning Server 9.2.1. It indicates the length of time that Machine Learning Server caches the information received from LDAP or AAD regarding user group membership. After the cache lifetime elapses, the roles and users are checked again. The changes you make to the groups in your LDAP or AAD configuration are not reflected in  Machine Learning Server until the cache lifetime expires and the configuration is checked again. 
   
   >[!IMPORTANT]
   Defining a Reader role might affect web service consumption latency as roles are being validated on each call to the web service.

   >[!WARNING]
   >For AD/LDAP authentications:
   >1. Be careful not to use the 'CN=' portion of the distinguished names. For example, if the distinguished name appears as 'CN=Administrators', enter only 'Administrators' here.
   
   >2. Ensure that the username returned for the value of 'UniqueUserIdentifierAttributeName' matches the username returned by 'SearchFilter'. For example, if `"SearchFilter": "cn={0}"` and `"UniqueUserIdentifierAttributeName": "userPrincipalName"`, then the values for `cn` and `userPrincipalName` must match.

   >3. For R Server 9.1 users: If you specify LDAP Root as the SearchBase in web node's appsettings.json, a search of the roles returns [LDAP referrals](https://technet.microsoft.com/en-us/library/cc978014.aspx) and throws a 'LDAPReferralException'. A workaround is to change the LDAP port in web node's appsettings.json from 389 to Global Catalog Port 3268. Or, for LDAPS, change Port to 3269 instead of 636. Global Catalogs do not return LDAP referrals in LDAP Search Results.  


### Step 2. Validate groups against AD/LDAP or AAD

Return to [the appsetting.json file](configure-find-admin-configuration-file.md) on each web node and make these updates:

+ **For Azure Active Directory:** In appsettings.json, find the "AzureActiveDirectory" section. Make sure the alphanumeric client key you created in the portal **for the web app** is used for "Key": property. This key allows Machine Learning Server to verify that the groups you've declared are valid in AAD. See following example. Learn more about [configuring Machine Learning Server to authenticate with Azure Active Directory](configure-authentication.md#aad).

  >[!IMPORTANT]
  > For more security, we recommend you [encrypt the key](configure-use-admin-utility.mdconfigure-admin-cli-encrypt-credentials.md) before adding the information to appsettings.json.

  >[!NOTE]
  > If a user belongs to more groups than allowed in AAD, AAD provides an overage claim in the token it returns. This claim along with the key you provide here allows Machine Learning Server to retrieve the group memberships for the user.

+ **For Active Directory/LDAP:** In appsettings.json, find the "LDAP" section.  The server verifies that the groups you have declared are valid in AD/LDAP using the QueryUserDn and QueryUserPassword values in the "LDAP" section. See the following example: These settings allow Machine Learning Server to verify that each declared group is, in fact, a valid, existing group in AD. Learn more about [configuring Machine Learning Server  to authenticate with Active Directory/LDAP](configure-authentication.md#ldap).

  With AD/LDAP, you can **restrict which users can log in and call APIs** by declaring the groups with permissions in the ['SearchFilter' LDAP property](configure-authentication.mdconfigure-admin-cli-encrypt-credentials.md).  Then, users in other groups are not able to call any APIs. In this example, only members of the 'mrsreaders', 'mrsowners', and 'mrscontributors' groups can call APIs after logging in.

  ```
  "SearchFilter": "(&(sAMAccountName={0})(|(memberOf=CN=mrsreaders,OU=Readers,OU=AA,DC=pseudorandom,DC=cloud)(memberOf=CN=mrsowners,OU=Owner,OU=AA,DC=pseudorandom,DC=cloud)(memberOf=CN=mrscontributors,OU=Contributor,OU=AA,DC=pseudorandom,DC=cloud)))",         
  "UniqueUserIdentifierAttributeName": "sAMAccountName",
  ```

### Step 3. Apply the changes to Machine Learning Server / R Server

1. [Restart the web node](configure-admin-cli-stop-start.md) for the changes to take effect. Log in  using [the local 'admin' account](configure-authentication.md#local) in the administration utility.

1. Repeat these changes in every web node you have configured.  The configuration must be the same across all web nodes.

## Example

Here is an example of roles declared for AD/LDAP in appsettings.json on the web node:

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
   "Owner": ["Administrators"],
   "Contributor": ["RProgrammers", "Quality"]      
}
```


## Web service permissions after role change

Over time, a user role can change if they are no longer part of a security group in AD/LDAP or AAD, or if a security group is no longer mapped to a role in Machine Learning Server. 

Whenever a user's role changes, that user may not longer be able to perform the same tasks on their web services. If you publish a web service while assigned to the "Owner" role, you can continue to manage and interact with that web service version as long as you are assigned this role. However, if you are reassigned to "Contributor", then you can still interact with that web service version as before, but you cannot update or delete the services published by others. Or, if roles are defined and you are no longer assigned to any roles, then you are implicitly assigned to the "Reader" role if it exists ([see here](#configroles). Consequently, you can no longer manage any services, including those services that you published previously when you were assigned to a role. 

## See Also

+ [How to publish and manage web services in R](how-to-deploy-web-service-publish-manage-in-r.md)

+ [How to interact with and consume web services in R](how-to-consume-web-service-interact-in-r.md)

+ [Authentication options for Machine Learning Server when operationalizing analytics](configure-authentication.md)

+ [Blog article: Role Based Access Control With MRS 9.1.0](https://blogs.msdn.microsoft.com/rserver/2017/04/10/role-based-access-control-with-mrs-9-1-0/)
