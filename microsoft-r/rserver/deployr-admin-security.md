---

# required metadata
title: " Security in DeployR"
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "03/17/2016"
ms.topic: "article"
ms.prod: "deployr"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# Security for DeployR

## Overview

DeployR is a server framework that exposes the R platform as a service to allow the integration of R statistics, analytics, and visualizations inside Web, desktop, and mobile applications. In addition to providing a simple yet powerful Web services API, the framework also supports a highly flexible, enterprise security model.

By default, DeployR supports basic authentication. Users simply provide plain text username and password credentials, which are then matched against user account data stored in the DeployR database. User accounts are created and managed by an administrator using the DeployR Administration Console. Given these credentials are passed from client to server as plain text, we strongly recommend, in your production environments, that you enable and use HTTPS connections every time your application attempts to authenticate with DeployR. For more information, see [Working with HTTPS](#enable-server-ssl-https).

While basic authentication provides a simple and reliable authentication solution, the ability to deliver a seamless integration with existing enterprise security solutions is often paramount. The DeployR enterprise security model can easily be configured to "plug" into a number of widely adopted enterprise security solutions.

>**Get More DeployR Power:** Basic Authentication is available for all DeployR configurations and editions.  
>[Get DeployR Enterprise today](http://go.microsoft.com/fwlink/?LinkID=698525) to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

The DeployR security model is sufficiently flexible that it can work with multiple enterprise security solutions at the same time. As such, DeployR Enterprise ships with a number of security providers that together represent a provider-chain upon which user credentials are evaluated. For more information, see [Authentication and Authorization](#authentication-and-authorization). Every aspect of the DeployR security model is controlled by the configuration properties found in the DeployR external configuration file. This file can be found at `$DEPLOYR_HOME/deployr/deployr.groovy`.

The following sections in this document detail how to work with these configuration properties to achieve your preferred security implementation.

## Authentication and Authorization

DeployR ships with security providers for the following enterprise security solutions:

-   [Basic Authentication](#basic-authentication)
-   [CA Single Sign-On](#ca-single-sign-on-siteminder-pre-authentication)
-   [PAM Authentication Services](#pam-authentication)
-   [LDAP Authentication](#ldap-authentication)
-   [Active Directory Services](#active-directory-authentication)
-   [R Session Process Controls](#r-session-process-controls)

>**Get More DeployR Power:** Basic Authentication is available for all DeployR configurations and editions.    
>[Get DeployR Enterprise today](http://go.microsoft.com/fwlink/?LinkID=698525) to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

The DeployR security model is sufficiently flexible that it can work with multiple enterprise security solutions at the same time. If two or more enterprise security solutions are active, then user credentials are evaluated by each of the DeployR security providers in the order indicated in preceding list. If a security provider, at any depth in the provider-chain, establishes that the credentials are valid, then the login call succeeds. If the user credentials are not validated by any of the security providers in the provider-chain, then the login call fails.

When DeployR processes a user login, there are two key steps involved:

1.  Credentials must be authenticated
2.  Access privileges must be determined

DeployR access privileges are determined by the roles assigned to a user. In the case of basic authentication, an administrator simply assigns roles to a user within the DeployR Administration Console.

>**Learn More!** For information on how to manage user accounts as well as how to use roles as a means to assign access privileges to a user or to restrict access to individual R scripts, refer to the [Administration Console Help](deployr-admin-console/deployr-admin-console-about.md).

When you integrate with an external enterprise security solution, you want access privileges to be inherited from the external system. This is achieved with simple mappings in the DeployR configuration properties, which link external groups to internal roles.

### Basic Authentication

By default, the Basic Authentication security provider is enabled. The Basic Authentication provider is always enabled and there are no additional security configuration properties for this provider.

>**Get More DeployR Power:** Basic Authentication is available for all Deployr configurations and editions.  
>[Get DeployR Enterprise today](http://go.microsoft.com/fwlink/?LinkID=698525) to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

    /*
     * DeployR Basic Authentication Policy Properties
     */

### CA Single Sign-On (SiteMinder) Pre-Authentication

By default, the **CA Single Sign-On** (formerly known as SiteMinder) security provider is disabled. To enable CA Single Sign-On support, you must first update CA Single Sign-On Policy Server configuration. Then, you must update the relevant properties in your DeployR external configuration file.

>**Get More DeployR Power:** This form of security is available for [DeployR Enterprise](https://deployr.revolutionanalytics.com/download/) only.

**To enable CA Single Sign-On support:**

1.  Define or update your CA Single Sign-On Policy Server configuration. For details on how to do this, [read here](deployr-admin-configure-ca-sso.md).

2.  Update the relevant properties in your DeployR external configuration file.
    This step assumes that:

    -   Your CA Single Sign-On Policy Server is properly configured and running
    -   You understand which header files are being used by your policy server

    Relevant snippet from `deployr.groovy` file shown here:

         /*
          * Siteminder Single Sign-On (Pre-Authentication) Policy Properties
          */
       
         deployr.security.siteminder.preauth.enabled = false

         // deployr.security.preauth.username.header
         // Identify Siteminder username header, defaults to HTTP_SM_USER as used by Siteminder Tomcat 7 Agent.
         deployr.security.preauth.username.header = 'HTTP_SM_USER'

         // deployr.security.preauth.group.header
         // Identify Siteminder groups header.
         deployr.security.preauth.group.header = 'SM_USER_GROUP'

         // deployr.security.preauth.group.separator
         // Identify Siteminder groups delimiter header.
         deployr.security.preauth.group.separator = '^'

         // deployr.security.preauth.groups.map
         // Allows you to map Siteminder group names to DeployR role names.
         // NOTE: Siteminder group names must be defined using the distinguished
         // name for the group. Group distinguished names are case sensitive.
         // For example, your Siteminder distinguished group name
         // "CN=finance,OU=company,DC=acme,DC=com" must appear in the map as
         // "CN=finance,OU=company,DC=acme,DC=com". DeployR role names must
         // begin with ROLE_ and must always be upper case.
         deployr.security.preauth.groups.map = [ 'CN=finance,OU=company,DC=acme,DC=com' : 'ROLE_BASIC_USER',
                                          'CN=engineering,OU=company,DC=acme,DC=com' : 'ROLE_POWER_USER' ]

         // deployr.security.preauth.default.role
         // Optional, grant default DeployR Role to all Siteminder authenticated users:
         deployr.security.preauth.default.role = 'ROLE_BASIC_USER'

### PAM Authentication

By default, the **PAM** security provider is disabled. To enable PAM authentication support, you must:

1.  Update the relevant properties in your DeployR external configuration file, deployr.groovy.
2.  Follow the DeployR server system files configuration changes outlined below.

PAM is the Linux Pluggable Authentication Modules provided to support dynamic authorization for applications and services in a Linux system. If DeployR is installed on a Linux system, then the PAM security provider allows users to authenticate with DeployR using their existing Linux system username and password.

>**Get More DeployR Power:** This form of security is available for [DeployR Enterprise](https://deployr.revolutionanalytics.com/download/) only.

1.  Update the following properties in your DeployR external configuration file, `deployr.groovy`:

    -   deployr.security.pam.authentication.enabled
    -   deployr.security.pam.groups.map
    -   deployr.security.pam.default.role

    Relevant snippet from `deployr.groovy` file shown here:

         /*
          * DeployR PAM Authentication Policy Properties
          */

         deployr.security.pam.authentication.enabled = false

         // deployr.security.pam.groups.map
         // Allows you to map PAM user group names to DeployR role names.
         // NOTE: PAM group names are case sensitive. For example, your
         // PAM group named "finance" must appear in the map as "finance".
         // DeployR role names must begin with ROLE_ and must always be
         // upper case.
         deployr.security.pam.groups.map = [ 'finance' : 'ROLE_BASIC_USER',
                                           'engineering' : 'ROLE_POWER_USER' ]

         // deployr.security.pam.default.role
         // Optional, grant default DeployR Role to all PAM authenticated users:
         deployr.security.pam.default.role = 'ROLE_BASIC_USER'

2.  Apply the following configuration changes to the DeployR server system files:

    #### Non-Root Installs
    
    **Preparing**

    1.  Before making any configuration changes to the server system files, stop the DeployR server:

            cd /home/deployr-user/deployr/8.0.0
            ./stopAll.sh

    2.  Log in as `root` on your DeployR server.

    **Grant Permissions**

    The following steps grant `deployr-user` permission to execute just one command as a `sudo` user, which launches the Tomcat server. This is required so the DeployR server can avail of PAM authentication services.

    1.  Using your preferred editor, edit the file:

            /etc/sudoers

    2.  Find the following section:

            ## Command Aliases

    3.  Add the following line to this section:

            Cmnd_Alias DEPLOYRTOMCAT = /home/deployr-user/deployr/8.0.0/tomcat/tomcat7.sh

    4.  Find the following section:

            ## Allow root to run any commands anywhere

    5.  Add the following line to this section:

            %deployr-user      ALL = DEPLOYRTOMCAT

        Your file should now look like this, where the order is important:

            root    ALL=(ALL)       ALL
            %deployr-user   ALL = DEPLOYRTOMCAT

    6.  Save these changes and close the file in your editor.

    7.  Log out `root` from your DeployR server.

    8.  Log in as `deployr-user` to your DeployR server.

    **Update the `startAll` and `stopAll` Scripts**

    1.  Update the DeployR `startAll.sh` shell script to take advantage of the `sudo` command configured above.

        1.  Using your preferred editor, edit the file:

            	/home/deployr-user/deployr/8.0.0/startAll.sh

        2.  Find the following line:

            	/home/deployr-user/deployr/8.0.0/tomcat/tomcat7.sh start

        2.  Change it to the following:

            	sudo /home/deployr-user/deployr/8.0.0/tomcat/tomcat7.sh start

        3.  Restart the DeployR server:

	            cd /home/deployr-user/deployr/8.0.0
	            ./startAll.sh

        4.  Save this change and close the file in your editor.

    2.  Update the DeployR `stopAll.sh` shell script to take advantage of the `sudo` command configured above.

        1.  Using your preferred editor, edit the file:

            	/home/deployr-user/deployr/8.0.0/stopAll.sh

        2.  Find the following line:

            	/home/deployr-user/deployr/8.0.0/tomcat/tomcat7.sh start

        3.  Change it to the following:

            	sudo /home/deployr-user/deployr/8.0.0/tomcat/tomcat7.sh start

        4.  Restart the DeployR server:

            	cd /home/deployr-user/deployr/8.0.0
            	./stopAll.sh

        5.	 Save this change and close the file in your editor.

	#### Root Installs

    **Preparing**

    1.  Before making any configuration changes to the server system files, stop the DeployR server:

            cd /opt/deployr/8.0.0
            ./stopAll.sh

    2.  Log in as `root` on your DeployR server.

    **Grant Permissions**

    The following steps grant `root` permission to launch the Tomcat server. This is required so the DeployR server can avail of PAM authentication services.

    1.  Using your preferred editor, edit the file:

            /opt/deployr/8.0.0/tomcat/tomcat7.sh

    2.  Find the following section:

        -   On Redhat/CentOS platforms:

                daemon --user "apache" ${START_TOMCAT}

        -   On SLES platforms:

                start_daemon -u r "apache" ${START_TOMCAT}

    3.  Change `"apache"` to `"root"` as follows:

        -   On Redhat/CentOS platforms:

                daemon --user "root" ${START_TOMCAT}

        -   On SLES platforms:

                start_daemon -u r "root" ${START_TOMCAT}

    4.  Save this change and close the file in your editor.

    5.  Restart the DeployR server:

            cd /opt/deployr/8.0.0
            ./startAll.sh

>[!IMPORTANT]
>If you have enabled PAM authentication as part of the required steps for enabling R Session Process Controls, then please continue with your configuration using [these steps](#r-session-process-controls).

### LDAP Authentication

By default, the **LDAP** security provider is disabled. To enable LDAP authentication support, you must update the relevant properties in your DeployR external configuration file. The values you assign to these properties should match the configuration of your LDAP Directory Information Tree (DIT).

>**Get More DeployR Power:** This form of security is available for [DeployR Enterprise](https://deployr.revolutionanalytics.com/download/) only.

&nbsp;

>The LDAP and Active Directory security providers are, in fact, one and the same, and only their [configuration properties](#ldap-active-directory-configuration-properties) differ. As such, you may enable the LDAP provider or the Active Directory provider, but not both at the same time.

    /*
     * DeployR LDAP Authentication Configuration Properties
     */
    grails.plugin.springsecurity.ldap.context.managerDn = 'dc=example,dc=com'
    grails.plugin.springsecurity.ldap.context.managerPassword = 'secret'
    grails.plugin.springsecurity.ldap.context.server = 'ldap://localhost:10389/'
    grails.plugin.springsecurity.ldap.context.anonymousReadOnly = true
    grails.plugin.springsecurity.ldap.search.base = 'ou=people,dc=example,dc=com'
    grails.plugin.springsecurity.ldap.search.searchSubtree = true
    grails.plugin.springsecurity.ldap.authorities.retrieveGroupRoles = true
    grails.plugin.springsecurity.ldap.authorities.groupSearchBase = 'ou=people,dc=example,dc=com'
    grails.plugin.springsecurity.ldap.authorities.defaultRole = "ROLE_BASIC_USER"
    grails.plugin.springsecurity.ldap.authorities.groupSearchFilter = 'member={0}'

    // Optionally, specify LDAP password encryption algorithm: MD5, SHA-256
    // grails.plugin.springsecurity.password.algorithm = 'xxx'

    // deployr.security.ldap.user.properties.map
    // Allows you to map between LDAP user property names to DeployR user property names:
    deployr.security.ldap.user.properties.map = ['displayName':'cn',
                                                 'email':'mail',
                                                 'uid' : 'uidNumber',
                                                 'gid' : 'gidNumber']

    // deployr.security.ldap.roles.map property
    // Allows you to map between LDAP group names to DeployR role names.
    // NOTE, while LDAP group names can be defined on the LDAP server using
    // any mix of upper and lower case, such as finance, Finance or FINANCE,
    // the LDAP group names that appear in the map must have ROLE_ appended
    // and be capitialized. For example, an LDAP group named "finance" should
    // appear in the map as ROLE_FINANCE. DeployR role names must
    // begin with ROLE_ and must always be upper case.
    deployr.security.ldap.roles.map = ['ROLE_FINANCE':'ROLE_BASIC_USER',
                                       'ROLE_ENGINEERING':'ROLE_POWER_USER']

For more information, see the complete list of LDAP [configuration properties](#ldap-active-directory-configuration-properties).

>[!IMPORTANT]
>If you have enabled PAM authentication as part of the required steps for enabling R Session Process Controls, then please continue with your configuration using [these steps](#r-session-process-controls).

### Active Directory Authentication

By default, the Active Directory security provider is disabled. To enable Active Directory authentication support you must update the relevant properties in your DeployR external configuration file. The values you assign to these properties should match the configuration of your Active Directory Directory Information Tree (DIT).

>**Get More DeployR Power:** This form of security is available for [DeployR Enterprise](https://deployr.revolutionanalytics.com/download/) only.

&nbsp;

>The LDAP and Active Directory security providers are, in fact, one and the same, and only their [configuration properties](#ldap-active-directory-configuration-properties) differ. As such, you may enable the LDAP provider or the Active Directory provider, but not both at the same time.


    /*
     * DeployR Active Directory Configuration Properties
     */

    grails.plugin.springsecurity.ldap.context.managerDn = 'dc=example,dc=com'
    grails.plugin.springsecurity.ldap.context.managerPassword = 'secret'
    grails.plugin.springsecurity.ldap.context.server = 'ldap://locahost:10389/'
    grails.plugin.springsecurity.ldap.authorities.ignorePartialResultException = true
    grails.plugin.springsecurity.ldap.search.base = 'ou=people,dc=example,dc=com'
    grails.plugin.springsecurity.ldap.search.searchSubtree = true
    grails.plugin.springsecurity.ldap.search.attributesToReturn = ['mail', 'displayName'] 
    grails.plugin.springsecurity.ldap.search.filter="sAMAccountName={0}"
    grails.plugin.springsecurity.ldap.auth.hideUserNotFoundExceptions = false
    grails.plugin.springsecurity.ldap.authorities.retrieveGroupRoles = true
    grails.plugin.springsecurity.ldap.authorities.groupSearchFilter = 'member={0}'
    grails.plugin.springsecurity.ldap.authorities.groupSearchBase = 'ou=group,dc=example,dc=com'

    // Optionally, specify LDAP password encryption algorithm: MD5, SHA-256
    // grails.plugin.springsecurity.password.algorithm = 'xxx'

    // deployr.security.ldap.user.properties.map
    // Allows you to map between LDAP user property names to DeployR user property names:
    deployr.security.ldap.user.properties.map = ['displayName':'cn',
                                                 'email':'mail',
                                                 'uid' : 'uidNumber',
                                                 'gid' : 'gidNumber']

    // deployr.security.ldap.roles.map property
    // Allows you to map between LDAP group names to DeployR role names.
    // NOTE, while LDAP group names can be defined on the LDAP server using
    // any mix of upper and lower case, such as finance, Finance or FINANCE,
    // the LDAP group names that appear in the map must have ROLE_ appended
    // and be capitialized. For example, an LDAP group named "finance" should
    // appear in the map as ROLE_FINANCE. DeployR role names must
    // begin with ROLE_ and must always be upper case.
    deployr.security.ldap.roles.map = ['ROLE_FINANCE':'ROLE_BASIC_USER',
                                       'ROLE_ENGINEERING':'ROLE_POWER_USER']

For more information, see the complete list of [configuration properties](#ldap-active-directory-configuration-properties).

>[!IMPORTANT]
>If you have enabled PAM authentication as part of the required steps for enabling R Session Process Controls then please continue with your configuration using [these steps](#r-session-process-controls).

### LDAP & Active Directory Configuration Properties

The following table presents the complete list of LDAP and Active Directory configuration properties.

>[!IMPORTANT]
>To use one of these configuration properties in the `deployr.groovy` external configuration file, you must prefix the property name with `grails.plugin.springsecurity`. For example, to use the `ldap.context.server='ldap://localhost:389'` property in `deploy.groovy`, you must write the property as such: `grails.plugin.springsecurity.ldap.context.server='ldap://localhost:389'`

### Context Properties

| Property                                | Default Value                  | Description                                                                                                   |
|-----------------------------------------|--------------------------------|---------------------------------------------------------------------------------------------------------------|
| ldap.context.server                     | 'ldap://localhost:389'         | Address of the LDAP server.                                                                                   |
| ldap.context.managerDn                  | "'cn=admin,dc=example,dc=com'" | DN to authenticate with.                                                                                      |
| ldap.context.managerPassword            | secret'                        | Manager password to authenticate with.                                                                        |
| ldap.context.baseEnvironmentProperties  | None                           | Extra context properties.                                                                                     |
| ldap.context.cacheEnvironmentProperties | TRUE                           | Whether environment properties should be cached between requests.                                             |
| ldap.context.anonymousReadOnly          | FALSE                          | Whether an anonymous environment should be used for read-only operations.                                     |
| ldap.context.referral                   | null ('ignore')                | The method to handle referrals. Can be 'ignore' or 'follow' to enable referrals to be automatically followed. |

### Search Properties

| Property                              | Default Value                  | Description                                                                                                                                         |
|---------------------------------------|--------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| ldap.search.base                      | "'ou=users,dc=example,dc=com'" | "Context name to search in, relative to the base of the configured ContextSource, e.g. 'ou=users,dc=example,dc=com'."                               |
| ldap.search.searchSubtree             | TRUE                           | "If true then searches the entire subtree as identified by context, if false (the default) then only searches the level identified by the context." |
| ldap.search.filter                    | '(uid={0})'                    | The filter expression used in the user search.                                                                                                      |
| ldap.search.derefLink                 | FALSE                          | Enables/disables link dereferencing during the search.                                                                                              |
| ldap.search.timeLimit                 | 0 (unlimited)                  | The time to wait before the search fails.                                                                                                           |
| ldap.search.attributesToReturn        | null (all)                     | The attributes to return as part of the search.                                                                                                     |
| ldap.authenticator.dnPatterns         | null (none)                    | "Optional pattern(s) used to create DN search patterns, e.g. \[""cn={0},ou=people""\]."                                                             |
| ldap.authenticator.attributesToReturn | null (all)                     | Names of attribute ids to return; use null to return all and an empty list to return none.                                                          |

### Authorities Properties

| Property                                      | Default Value                   | Description                                                                                                                                                  |
|-----------------------------------------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ldap.authorities.retrieveGroupRoles           | TRUE                            | Whether to infer roles based on group membership.                                                                                                            |
| ldap.authorities.retrieveDatabaseRoles        | FALSE                           | Whether to retrieve additional roles from the database using the User/Role many-to-many.                                                                     |
| ldap.authorities.groupRoleAttribute           | 'cn'                            | The ID of the attribute which contains the role name for a group.                                                                                            |
| ldap.authorities.groupSearchFilter            | 'uniquemember={0}'              | The pattern to be used for the user search. {0} is the user's DN.                                                                                            |
| ldap.authorities.searchSubtree                | TRUE                            | "If true a subtree scope search will be performed, otherwise a single-level search is used."                                                                 |
| ldap.authorities.groupSearchBase              | "'ou=groups,dc=example,dc=com'" | The base DN from which the search for group membership should be performed.                                                                                  |
| ldap.authorities.ignorePartialResultException | FALSE                           | " Whether PartialResultExceptions should be ignored in searches, typically used with Active Directory since AD servers often have a problem with referrals." |
| ldap.authorities.defaultRole                  | None                            | An optional default role to be assigned to all users.                                                                                                        |
| ldap.mapper.roleAttributes                    | Null                            | Optional names of role attributes.                                                                                                                           |
| ldap.mapper.convertToUpperCase                | TRUE                            | "Whether to uppercase retrieved role names (will also be prefixed with ""ROLE\_"")"                                                                          |


### R Session Process Controls

>**Get More DeployR Power:** This form of security is available for [DeployR Enterprise](https://deployr.revolutionanalytics.com/download/) on **Linux platforms** only.

By default, R sessions executing on the DeployR grid are not authorized to access files or directories outside of the R working directory. To enable broader file system access for a given R session to files or directories based on specific authenticated user ID and group ID credentials, you must first do ONE of the following:

-   Enable [PAM authentication](#pam-authentication), or
-   Enable [LDAP authentication](#ldap-authentication), or
-   Enable [Active Directory authentication](#active-directory-authentication)

Once you have enabled PAM, LDAP, or AD authentication, you must (Step 1) update the relevant process controls properties on the server **and then** (Step 2) apply system-level configuration changes to every single node on your DeployR grid, as follows:

##### Step 1: Update Process Control Properties

After you've enabled either PAM, LDAP, or Active Directory authentication, you can update the relevant process control properties in your DeployR server external configuration file, `deployr.groovy`, on the main DeployR server:

-   deployr.security.r.session.process.controls.enabled
-   deployr.security.r.session.process.default.uid
-   deployr.security.r.session.process.default.gid

Relevant snippet from deployr.groovy file shown here:

        /*
        * DeployR R Session Process Controls Policy Configuration
        *
        * By default, each R session on the DeployR grid executes
        * with permissions inherited from the master RServe process
        * that was responsible for launching it.
        *
        * Enable deployr.security.r.session.process.controls to force
        * each individual R session process to execute with the
        * permissions of the end-user requesting the R session,
        * using their authenticated user ID and group ID.
        *
        * If this property is enabled but the uid/gid associated with
        * the end-user requesting the R session is not available,
        * then the R session process will execute using the default[uid,gid]
        * indicated on the following properties.
        *
        * The default[uid,gid] values indicated on these policy
        * configuration properties must correspond to a valid user and
        * group ID configured on each node on the DeployR grid.
        *
        * For example, if you installed DeployR as deployr-user then
        * we recommend using the uid/gid for deployr-user for these
        * default property values.
        */
        deployr.security.r.session.process.controls.enabled=false
        deployr.security.r.session.process.default.uid=9999
        deployr.security.r.session.process.default.gid=9999

##### Step 2: Make System-Level Configuration Changes to Every Node

>[!WARNING]
>**Before You Begin!** Make sure you've enabled the appropriate process control properties before beginning this step.

+ **For Non-Root Installs:**  Apply the following configuration changes on **each and every node** on your DeployR grid, including the default grid node:

	1.  Before making any configuration changes to system files, you must stop Rserve and any other DeployR-related services:
	
			cd /home/deployr-user/deployr/8.0.0
			./stopAll.sh

	2.  Grant `deployr-user` permission to execute a command as a `sudo` user so that the RServe process can be launched. This is required so that the DeployR server can enforce R session process controls.
	    1.  Log in as `root`.
	   
	    1.  Using your preferred editor, edit the file `/etc/sudoers`.
	    
	    1.  Find the following section:

                 ## Command Aliases

	    1.  Add the following line to this section:

                 Cmnd_Alias DEPLOYRRSERVE = /home/deployr-user/deployr/8.0.0/rserve/rserve.sh

	    1.  Find the following section:

                 ## Allow root to run any commands anywhere

	    1.  Add or append `DEPLOYRRSERVE` for `%deployr-user` to this section:

                 ## If an entry for %deployr-user is not found, add this line:
                 %deployr-user      ALL = DEPLOYRRSERVE
                 ## Otherwise append as shown:
                 %deployr-user      ALL = DEPLOYRTOMCAT,DEPLOYRRSERVE

	    1.  Save these changes and close the file in your editor.

	    1.  Log out `root`.

	3.  Update the DeployR `startAll.sh` shell script to take advantage of the `sudo` command configured above.

	    1.  Log in as `deployr-user`.

	    1. Using your preferred editor, edit the file `/home/deployr-user/deployr/8.0.0/startAll.sh`.

	    1. Find the following line:

                 /home/deployr-user/deployr/8.0.0/rserve/rserve.sh start

	    1. Change it to the following:

                 sudo /home/deployr-user/deployr/8.0.0/rserve/rserve.sh start

	    1. Save this change and close the file in your editor.

	4.  Update the DeployR `stopAll.sh` shell script to take advantage of the `sudo` command configured above.

	    1. Using your preferred editor, edit the file `/home/deployr-user/deployr/8.0.0/stopAll.sh`.

	    1. Find the following line:

                 /home/deployr-user/deployr/8.0.0/rserve/rserve.sh stop

	    1. Change it to the following:

                 sudo /home/deployr-user/deployr/8.0.0/rserve/rserve.sh stop

	    1. Save this change and close the file in your editor.

	5.  Set group privileges on the user directory containing the DeployR grid node install directory.

	    1. Log in as `root`.

	    1. Set group privileges.

                 cd /home
                 chmod -R g+rwx deployr-user

	6.  Add each user that will authenticate with the server to the `deployr-user` group.

	    1. Log in as `root`.

	    1. Execute the following command to add each user to the `deployr-user` group.

                 usermod -a -G deployr-user <some-username>

	    1. Repeat step **B.** for each user that will authenticate with the server.

	    1. Log out `root`.

	7.  Restart Rserve and any other DeployR-related services on the machine hosting the DeployR grid node:

	    1. Log in as `deployr-user`.

	    1. Start Rserve and any other DeployR-related services:

                 cd /home/deployr-user/deployr/8.0.0
                 ./startAll.sh


+ **For Root Installs**:  Apply the following configuration changes on **each and every node** on your DeployR grid, including the default grid node:

	1. Log in as `root`.

	1. Before making any configuration changes to system files, stop Rserve and any other DeployR-related services:

			cd /opt/deployr/8.0.0
			./stopAll.sh

	1. Grant `root` permission to launch the RServe process. This is required so that each DeployR grid node can enforce R session process controls.

	    + On Redhat/CentOS platforms:
	    
	       1. Open the file `/opt/deployr/8.0.0/rserve/rserve.sh`.

	       1. Find the following section `daemon --user "apache"`.

	       1. Change `apache` to `root` as follows `daemon --user "root"`.

	       1. Save this change and close the file in your editor.	       

	    + On SLES platforms:
	    
	       1. Open the file `/opt/deployr/8.0.0/rserve/rserve.sh`.

	       1. Find the following section `start_daemon -u r "apache"`.

	       1. Change `apache` to `root` as follows `start_daemon -u r "root"`.

	       1. Save this change and close the file in your editor.

	1. Set group privileges on the DeployR install directory.

			cd /opt
			chown -R apache.apache deployr
			chmod -R g+rwx deployr

	1. Execute the following command to add each user to the `apache` group. **Repeat for each user that will authenticate with the server.**

			usermod -a -G apache <some-username>

	1. Restart Rserve and any other DeployR-related services:

			cd /home/deployr-user/deployr/8.0.0
			./startAll.sh

## Enable Server SSL / HTTPS

The **Secure Sockets Layer (SSL)** is a commonly-used protocol for managing the security of message transmissions on the Internet. Since we cannot ship SSL certificates for you, SSL on DeployR is disabled by default.

>[!IMPORTANT]
>We strongly recommended that SSL/HTTPS be enabled in **all production environments.**

Once enabled your client applications can make API calls that connect over HTTPS.

### To enable SSL support on the DeployR server:

1.  **Provide an SSL certificate.**
    + If you have a trusted SSL certificate from a registered authority, then copy it to the Tomcat directory so it can be deployed at startup. (If you do not have one, skip to the next bullet to define a temporary certificate.)
        
        >[!NOTE]
                >Be sure to specify the correct Tomcat path for the `-keystore` argument.
		>This example is written for user `deployr-user`. For another user, use the appropriate filepath to the `.keystore`.

         + For Linux:
	        1.  Go to the directory in which the keystore is stored.
	        2.  Copy the certificate keystore to the Tomcat directory. At the prompt, type:
	        
                    cp .keystore /home/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore
      
	 + For OS X:
	        1.  Go to the directory in which the keystore is stored.
	        2.  Copy the certificate keystore to the Tomcat directory. At the prompt, type:

                    cp .keystore /Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore
       
	 + For Windows:

	        1.  Go to the directory in which the keystore is stored.
        	2.  Launch a command window as administrator and type the following at the prompt:

                    copy .keystore  C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\bin\.keystore
       
    + If you do not yet have a trusted SSL certificate from a registered authority, then create a temporary keystore for testing purposes. This temporary keystore will contain a “self-signed” certificate for Tomcat SSL on the server machine.
        
        >[!NOTE]
                >Be sure to specify the correct Tomcat path for the `-keystore` argument.
		>This example is written for user `deployr-user`. For another user, use the appropriate filepath to the `.keystore`.

        + For Linux:
	        1.  Run the `keytool` to generate a temporary keystore file. At a terminal prompt, type:
	        
                    $JAVA_HOME/bin/keytool -genkey -alias tomcat -keyalg RSA -keystore /home/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore

        	2.  Provide the following information when prompted by the script:

        + OS X:
	        1.  Run the `keytool` to generate a temporary keystore file. At a terminal prompt, type:
	        
                    $JAVA_HOME/bin/keytool -genkey -alias tomcat -keyalg RSA -keystore /Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore

        	2.  Provide the following information when prompted by the script:

        + Windows:	

	        1.  Launch a command window **as administrator**.
	
	        2.  Run the `keytool` to generate a temporary keystore file. At the prompt, type:

                    "%JAVA_HOME%\bin\keytool" -genkey -alias tomcat -keyalg RSA -keystore C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\bin\.keystore

	        3.  Provide the following information when prompted by the script:
        	    + For the keystore password, enter `changeit` and confirm this password.
        	    + For your name, organization, and location, either provide the information or press the Return key to skip to the next question.
        	    + When presented with the summary of your responses, enter `yes` to accept these entries.
        	    + For a key password for Tomcat, press the Return key to use `changeit`.


	**The temporary keystore has now been is created. We recommend that you use a trusted SSL certificate from a registered authority AS SOON as possible**.
		
	<a id="alertusers"></a>
	>[!WARNING]
	>**Alert Your Users!**  
	>The following browser warning applies ONLY for self-signed certificates. When DeployR users attempt to open the DeployR landing page, Administration Console, or Repository Manager in their Web browser, they will be prompted to acknowledge and accept your self-signed certificate as a security precaution. Each browser prompts in a different way, such as requiring users to acknowledge "I Understand the Risks” (Firefox), or to click “Advanced” (Chrome) or click “Continue” (Safari). Please inform your users accordingly.  
	>We strongly recommend that you use a trusted SSL certificate from a registered authority in your production environments.


2.  **Next, enable SSL support for Tomcat.**
    + For Linux:
    
		>This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` and `web.xml` as well as the `keystoreFile` property on the Connector.

	 1.  Enable the HTTPS connector on Tomcat by **removing the comments** around the following code in the file `/home/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/server.xml`.

                 <!-- 
                 <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="/home/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore" />
                 -->

	 2.  Force Tomcat to upgrade all HTTP connections to HTTPS connections by **removing the comments** around the following code in the file `/home/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/web.xml`.

                 <!-- 
                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOnly</web-resource-name>
                       <url-pattern>/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>CONFIDENTIAL</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>
                 <security-constraint>
                       <web-resource-collection>
                       <web-resource-name>HTTPSOrHTTP</web-resource-name>
                       <url-pattern>*.ico</url-pattern>
                       <url-pattern>/img/*</url-pattern>
                       <url-pattern>/css/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>NONE</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>
                 -->

	 3.  Be sure to open the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the iptables command (or equivalent command/tool) to open the port.
	 
        >If you are provisioning your server on a cloud service such as Azure or AWS, then you must also add endpoints for port 8001.

    + For OS X:

		>This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` and `web.xml` as well as the `keystoreFile` property on the Connector.

	 1. Enable the HTTPS connector on Tomcat by **removing the comments** around the following code in the file `/Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/server.xml`.

                 <!-- 
                 <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="/Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore" />
                 -->

	 2.  Force Tomcat to upgrade all HTTP connections to HTTPS connections by **removing the comments** around the following code in the file `/Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/web.xml`.

                 <!-- 
                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOnly</web-resource-name>
                       <url-pattern>/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>CONFIDENTIAL</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>
                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOrHTTP</web-resource-name>
                       <url-pattern>*.ico</url-pattern>
                       <url-pattern>/img/*</url-pattern>
                       <url-pattern>/css/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>NONE</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>
                 -->

	 3.  Be sure to open the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the iptables command (or equivalent command/tool) to open the port.

		>If you are provisioning your server on a cloud service such as Azure or AWS, then you must also add endpoints for port 8001.

    + For Windows:

	 1.  Enable the HTTPS connector/channel on Tomcat by **removing the comments** around the following code in the file `C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\conf\server.xml`.

                 <!-- 
                 <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\bin\.keystore" />
                 -->

	 2.  Force Tomcat to upgrade all HTTP connections to HTTPS connections by **removing the comments** around the following code in the file `C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\conf\web.xml`.

                 <!-- 
                 <security-constraint>
                  <web-resource-collection>
                      <web-resource-name>HTTPSOnly</web-resource-name>
                      <url-pattern>/*</url-pattern>
                  </web-resource-collection>
                  <user-data-constraint>
                      <transport-guarantee>CONFIDENTIAL</transport-guarantee>
                  </user-data-constraint>
                 </security-constraint>
                 <security-constraint>
                  <web-resource-collection>
                      <web-resource-name>HTTPSOrHTTP</web-resource-name>
                      <url-pattern>*.ico</url-pattern>
                      <url-pattern>/img/*</url-pattern>
                      <url-pattern>/css/*</url-pattern>
                  </web-resource-collection>
                  <user-data-constraint>
                      <transport-guarantee>NONE</transport-guarantee>
                  </user-data-constraint>
                 </security-constraint>
                 -->

	 3.  Be sure to open the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the iptables command (or equivalent command/tool) to open the port.

		>If you are provisioning your server on a cloud service such as Azure or AWS, then you must also add endpoints for port 8001.

3.  **Then, enable SSL support for DeployR.**

    + For Linux:

	 1. Enable SSL support on the Administration Console by changing `false` to `true` in the following line of the DeployR external configuration file, `/home/deployr-user/deployr/8.0.0/deployr/deployr.groovy`:

                 grails.plugins.springsecurity.auth.forceHttps = false

	 1. Enable HTTPS in the server policies so that any non-HTTPS connections to the server are automatically rejected.  Run the `setWebContext.sh` script and specify the value of `true` for the `https` argument:

                 /home/deployr-user/deployr/8.0.0/deployr/tools/setWebContext.sh -https true

    + For OS X:

	 1. Enable SSL support on the Administration Console by changing `false` to `true` in the following line of the DeployR external configuration file, `/Users/deployr-user/deployr/8.0.0/deployr/deployr.groovy`:

                 grails.plugins.springsecurity.auth.forceHttps = false

	 1. Enable HTTPS in the server policies so that any non-HTTPS connections to the server are automatically rejected. Run the `setWebContext.sh` script and specify the value of `true` for the `https` argument:

                 /Users/deployr-user/deployr/8.0.0/deployr/tools/setWebContext.sh -https true

    + For Windows:

	 1. Enable SSL support on the Administration Console by changing `false` to `true` in the following line of the DeployR external configuration file, `C:\Program Files\Microsoft\DeployR\8.0\deployr/deployr.groovy`:

                 grails.plugins.springsecurity.auth.forceHttps = false

	 1. Enable HTTPS in the server policies so that any non-HTTPS connections to the server are automatically rejected. Run the `setWebContext.bat` script and specify the value of `true` for the `https` argument:

                 C:\Program Files\Microsoft\DeployR\8.0\deployr\tools\setWebContext.bat -https true

	Upon completion of this script with `-https true`, the following changes will have been made to the server policies in the Administration Console:

    -   The server web context now ressembles `https://xx.xx.xx.xx:8001/deployr` instead of `http://xx.xx.xx.xx:8000/deployr`.
    -   The `Enable HTTPS` property for each of operation policies (authenticated, anonymous, and asynchronous) are all checked.

    [Learn more about server policies](deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties).

4.  **Restart DeployR** by [stopping and starting all its services](deployr-common-administration-tasks.md#starting-and-stopping-deployr) so the changes can take effect. Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.  
     

5.  **Test** these changes by logging into the landing page and visiting DeployR Administration Console using the new HTTPS URL at `https://<DEPLOYR_SERVER_IP>:8001/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine. If you are using an untrusted, self-signed certificate, and you or your users are have difficulty reaching DeployR in your browser, see the [Alert](#alertusers) at the end of step 1.


## Disable Server SSL / HTTPS

The **Secure Sockets Layer (SSL)** is a commonly-used protocol for managing the security of message transmissions on the Internet. By default, SSL on DeployR is disabled. If you have enabled SSL at some time and you now wish to disable SSL, follow the steps in this section.

>[!IMPORTANT]
>We strongly recommended that SSL/HTTPS be enabled in **all production environments.**

### To disable SSL support on the DeployR server:

1.  **Disable SSL support for Tomcat.**

    + For Linux:
		>[!NOTE]
		>This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` as well as the `keystoreFile` property on the Connector. For another user,also use the appropriate filepath to `web.xml`.

    	1.  Disable the HTTPS connector on Tomcat by **commenting out** the following code in the file `/home/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/server.xml`.

                 <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="/home/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore" />

    	1.  Be sure to close the Tomcat HTTPS port (7401) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to close the port.
		>If you are provisioning your server on a cloud service such as Azure or AWS, then you must also remove endpoints for port 8001.

    	1.  Disable the upgrade of all HTTP connections to HTTPS connections by **commenting out** the following code in the file `/home/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/web.xml`.

                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOnly</web-resource-name>
                       <url-pattern>/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>CONFIDENTIAL</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>
                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOrHTTP</web-resource-name>
                       <url-pattern>*.ico</url-pattern>
                       <url-pattern>/img/*</url-pattern>
                       <url-pattern>/css/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>NONE</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>

    + For OS X:
		>[!NOTE]
		>This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` as well as the `keystoreFile` property on the Connector. For another user,also use the appropriate filepath to `web.xml`.

    	1. Disable the HTTPS connector on Tomcat by **commenting out** the following code in the file `/Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/server.xml`.

                 <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="/Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore" />

    	1. Be sure to close the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to close the port.
		>If you are provisioning your server on a cloud service such as Azure or AWS, then you must also remove endpoints for port 8001.

    	1. Disable the upgrade of all HTTP connections to HTTPS connections by **commenting out** the following code in the file `/Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/conf/web.xml`.

                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOnly</web-resource-name>
                       <url-pattern>/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>CONFIDENTIAL</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>
                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOrHTTP</web-resource-name>
                       <url-pattern>*.ico</url-pattern>
                       <url-pattern>/img/*</url-pattern>
                       <url-pattern>/css/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>NONE</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>

    + For Windows:
		>[!NOTE]
		>This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` as well as the `keystoreFile` property on the Connector. For another user,also use the appropriate filepath to `web.xml`.

    	1. Disable the HTTPS connector/channel on Tomcat by **commenting out** the following code in the file `C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\conf\server.xml`.

                 <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\bin\.keystore" />

    	1. Be sure to close the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to close the port.
		>If you are provisioning your server on a cloud service such as Azure or AWS, then you must also remove endpoints for port 8001.

    	1. Disable the upgrade of all HTTP connections to HTTPS connections by **commenting out** the following code in the file `C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\conf\web.xml`.

                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOnly</web-resource-name>
                       <url-pattern>/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>CONFIDENTIAL</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>
                 <security-constraint>
                   <web-resource-collection>
                       <web-resource-name>HTTPSOrHTTP</web-resource-name>
                       <url-pattern>*.ico</url-pattern>
                       <url-pattern>/img/*</url-pattern>
                       <url-pattern>/css/*</url-pattern>
                   </web-resource-collection>
                   <user-data-constraint>
                       <transport-guarantee>NONE</transport-guarantee>
                   </user-data-constraint>
                 </security-constraint>


2.  **Next, disable SSL support for DeployR.**

    + For Linux:

    	1.  Disable SSL support on the Administration Console by changing `true` to `false` in the following line of the DeployR external configuration file, `/home/deployr-user/deployr/8.0.0/deployr/deployr.groovy`:

                 grails.plugins.springsecurity.auth.forceHttps = true

    	2.  Disable HTTPS in the server policies. Run the `setWebContext.sh` script and specify the value of `false` for the `https` argument:

                 /home/deployr-user/deployr/8.0.0/deployr/tools/setWebContext.sh -https false

    + For OS X:

    	1.   Disable SSL support on the Administration Console by changing `true` to `false` in the following line of the DeployR external configuration file, `/Users/deployr-user/deployr/8.0.0/deployr/deployr.groovy`:

                 grails.plugins.springsecurity.auth.forceHttps = true

        2.  Disable HTTPS in the server policies. Run the `setWebContext.sh` script and specify the value of `false` for the `https` argument:

                 /Users/deployr-user/deployr/8.0.0/deployr/tools/setWebContext.sh -https false

    + For Windows:

    	1.  Enable SSL support on the Administration Console by changing `false` to `true` in the following line of the DeployR external configuration file, `C:\Program Files\Microsoft\DeployR\8.0\deployr/deployr.groovy`:

                 grails.plugins.springsecurity.auth.forceHttps = true

    	2.  Run the `setWebContext.bat` script and specify the value of `false` for the `https` argument:

                 C:\Program Files\Microsoft\DeployR\8.0\deployr\tools\setWebContext.bat -https false


    	Upon completion of the `setWebContext` script with `-https false`, the following changes will have been made to the server policies in the Administration Console:

    	+ The server web context now ressembles `http://xx.xx.xx.xx:8000/deployr`.

    	+ The `Enable HTTPS` property for each of operation policies (authenticated, anonymous, and asynchronous) are disabled.

    	[Learn more about server policies](deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties).

3.  **Restart DeployR** by [stopping and starting all its services](deployr-common-administration-tasks.md#starting-and-stopping-deployr) so the changes can take effect. Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.  
     

4.  **Test** these changes by logging into the landing page and visiting DeployR Administration Console using the new HTTP URL at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine.

## Server Access Controls

### Working with IP Address Filters

While access to DeployR is typically controlled by the authentication mechanisms discussed in this document, DeployR also supports access controls based on IP address filters.

Under the [**Server Policies**](deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties) tab in the DeployR Administration Console, you have a mechanism to configure your IP address filter policy. The `Operation Policies` for authenticated, asynchronous, and anonymous operations each support an [IP filter](deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties) property. If you assign an IP filter to this property, then any attempt by a client application to connect from outside of the IP address range on that filter will be automatically rejected.

For example, you can make your DeployR server instance accessible only from IP addresses on the local LAN or VPN, such as `192.168.1.xxx` or `10.xxx.xxx.xxx`. Note that it is possible to achieve these same kinds of access controls with an appropriate configuration on your firewall and/or routers.

Refer to the [Administration Console Help](deployr-admin-console/deployr-admin-console-about.md) for further details on IP filters and server policies.

### Cross-Origin Resource Sharing

Cross-Origin Resource Sharing (CORS) enables your client application to freely communicate and make cross-site HTTP requests for resources from a domain other than where the DeployR is hosted.

CORS can be enabled or disabled in the DeployR external configuration file, `deployr.groovy`:

-   In **DeployR Enterprise**, support for CORS is disabled by default.
-   In **DeployR Open**, support for CORS is enabled by default.

<!-- -->

    /*
     * DeployR CORS Policy Configuration
     *
     * cors.headers = [ 'Access-Control-Allow-Origin': 'http://app.example.com']
     */
    cors.enabled = false

**To enable CORS support:**

1.  Update the relevant properties in `deployr.groovy` by setting `cors.enabled = true`.
2.  Stop and restart the DeployR server using [these instructions](deployr-common-administration-tasks.md#starting-and-stopping-deployr).

Optionally, to restrict cross-site HTTP requests to only those requests coming from a specific domain, specify a value for `Access-Control-Allow-Origin` on the `cors.headers` property.

## Project and Repository File Access Controls

DeployR enforces a consistent security model across projects and repository-managed files. This model is based on two simple precepts: ownership and access levels.

### Project Access Controls

When a project is created, it is privately owned by default, meaning it is visible only to its owner. Such user-based privacy is a central aspect of the DeployR security model.

The owner of a temporary or persistent project has, by default, full read-write access to that project and use of the full set of project-related APIs.

DeployR introduced a type of secure, temporary project called a *blackbox project*. Blackbox projects restrict access to the underlying R session. In this case, the project owner can use only a small subset of the project-related APIs, collectively known as the “Blackbox API Controls”.

If the owner of a project wants to grant read-only access to that project to other authenticated users, then the owner can set the access level for the project to `Shared`. You can change the access level on a project using the `/r/project/about/update` API call.

>Anonymous users are not permitted access to projects. For more information, refer to the [API Reference Help](https://deployr.revolutionanalytics.com/documents/dev/api-doc/).

### Repository File Access Controls

When a repository-managed file is created, it is privately owned by default, meaning it is visible only to its owner. Such user-based privacy is a central aspect of the DeployR security model.

The owner of a repository-managed file has full read-write access to that file and use of the full set of repository-related APIs.

If the owner of a repository-managed file wants to grant read-only access to that file to other users, then the owner can set the file’s access level to one of the following values:

-   `Private` - the default access level, the file is visible to its author(s) only.

-   `Restricted` - the file is visible to authenticated users that have been granted one or more of the roles indicated on the restricted property of the file.

-   `Shared` - the file is visible to all authenticated users when the shared property is true.

-   `Public` - the file is visible to all authenticated and all anonymous users when the published property is true.

You can change the access level on a repository-managed file using the `/r/repository/file/update` API call or using the [Repository Manager](deployr-repository-manager/deployr-repository-manager-files.md#about-file-properties).

For more information, refer to the section Introducing the Repository on the API in the [API Reference Help](https://deployr.revolutionanalytics.com/documents/dev/api-doc/).

### Repository File Download Controls

The repository file download controls provide fine-grain control over who can download repository file data. It is important to tailor the configuration of these controls in your DeployR external configuration file in order to enforce your preferred download policy for repository-managed files.

    /*
     * DeployR Repository File Download Controls
     *
     * The repository file download controls provide fine
     * grain control over user access to repository file data
     * returned on the following API call:
     *
     * /r/repository/file/download
     *
     * The default repository file download policy is shown
     * for each of the supported repository file access levels:
     *
     * [Private, Restricted, Shared, Public]
     *
     * - Files with private access can be downloaded by authors only.
     * - Files with restriced access can be downloaded by authors
     *   and by authenticated users with sufficient privileges.
     * - Files with shared access can be downloaded by authors
     *   and by authenticated users.
     * - Files with public access can be downloaded by authors
     *   and by authenticated users.
     * - Regardless of access level, by default anonymous users
     *   can not download repository files.
     *
     * Enable file.author.only.download to ensure only authors
     * can download a repository-managed file. When this
     * property is enabled the file.anonymous.download option
     * is ignored.
     *
     * Enable file.anonymous.download to allow anonymous
     * users to download files with public access.
     *
     * Note: The repository file download controls apply to
     * all repository-managed files excluding R scripts.
     *
     */
    deployr.security.repository.file.author.only.download=false
    deployr.security.repository.file.anonymous.download=false

### Repository Scripts Access Controls

Repository-managed R scripts can be exposed as an executable on the API. Since repository-managed R scripts are a type of repository-managed file, all information in the [previous section](#repository-file-access-controls) also applies to repository-managed scripts.

However, repository-managed R scripts deserve special mention since scripts can be managed through the Administration Console interface. Additionally, when you work with the R scripts in the Administration Console, you will likely also use and work with roles so as to impose restricted access to your R scripts.

>[!NOTE]
>For information on how to use roles as a means to restrict access to individual R scripts, refer to the [Administration Console Help](deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties).

### Repository Script Download Controls

The repository script download controls provide fine-grain control over who can download repository script data. It is important to tailor the configuration of these controls in your DeployR external configuration file in order to enforce your preferred download policy for repository-managed scripts.

    /*
     * DeployR Repository Script Download Controls
     *
     * The repository script download controls apply to
     * repository-managed scripts [*.R/r] and markdown
     * files [*.Rmd/rmd] only.
     *
     * The repository script download controls provide fine
     * grain control over user access to repository script data
     * returned on the following API call:
     *
     * /r/repository/file/download
     *
     * Note: Script download controls do not intefere with
     * permissions to execute a script. Execution permissions are
     * entirely determined by the file access level assigned to
     * the script by it's author.
     *
     * The default repository script download policy is shown
     * for each of the supported repository script access levels:
     *
     * [Private, Restricted, Shared, Public]
     *
     * - Scripts with private access can be downloaded by authors only.
     * - Scripts with restriced access can be downloaded by authors
     *   and by authenticated users with sufficient privileges.
     * - Scripts with shared access can be downloaded by authors
     *   and by authenticated users.
     * - Scripts with public access can be downloaded by authors
     *   and by authenticated users.
     * - Regardless of access level, by default anonymous users
     *   can not download repository scripts.
     *
     * Enable script.author.only.download to ensure only authors
     * can download a repository-managed script. When this
     * property is enabled the script.anonymous.download option
     * is ignored.
     *
     * Enable script.anonymous.download to allow anonymous
     * users to download scripts with public access.
     *
     * Enabled script.list.authenticate to prevent anonymous
     * users from calling the /r/repository/script/list API.
     *
     */
    deployr.security.repository.script.author.only.download=false
    deployr.security.repository.script.anonymous.download=false
    deployr.security.repository.script.list.authenticate=false

### File Type Black List Controls

The file type black-list controls provide fine-grain control over the types of files that can be:

-   Uploaded, transferred and written into the repository or
-   Uploaded, transferred, written and loaded into the working directory of a project (R session).

These controls are particularly useful if an administrator wants to ensure malicious executable files or shell scripts are not uploaded and executed on the DeployR server.

    /*
     * DeployR File Type Black List Policy Configuration
     *
     * Files with the following extensions are forbidden
     * on upload, transfer, and write calls on:
     * 1. Project (R sesesion) directories
     * 2. The Repository
     * These files are also forbidden from participating on
     * prelaodfile* parameters on all calls that adhere to
     * the DeployR standard execution model.
     *
     * ADMINISTRATORs are not subject to these restrictions.
     */
    deployr.file.type.black.list = [ "exe", "sh", "bat", "bash", "csh", "tcsh" ]

## Password Policies

To customize password constraints on user account passwords adjust the password policy configuration properties.

The `deployr.security.password.min.characters` property enforces a minimum length for any basic authentication password. The `deployr.security.password.upper.case` property, when enabled, enforces the requirement for user passwords to contain at least a single uppercase character. The `deployr.security.password.alphanumeric`property, when enabled, enforces the requirement for user passwords to contain both alphabetic and numeric characters.

    /*
     * DeployR Password Policy Configuration
     *
     * Note: enable password.upper.case if basic-auth users
     * are required to have at least one upper case character
     * in their password.
     */
    deployr.security.password.min.characters = 8
    deployr.security.password.upper.case = true
    deployr.security.password.alphanumeric = true

These policies affect basic authentication only and have no impact on CA Single Sign On, LDAP/AD or PAM authentication where passwords are maintained and managed outside of the DeployR database.

## Account Locking Policies

To protect against brute force techniques that can be used to try and "guess" passwords on *basic authentication* and *PAM authenticated* user accounts, DeployR enforces automatic account locking when repeated authentication attempts fail within a given period of time for a given user account:

    /*
     * DeployR Authentication Failure Lockout Policy Configuration
     */
    deployr.security.tally.login.failures.enabled = true
    deployr.security.tally.login.failures.limit = 10
    deployr.security.tally.login.lock.timeout = 1800

By default, automatic account locking is enabled and activates following 10 failed attempts at authentication on a given user account. To disable this feature, set the following configuration property to `false`:

    deployr.security.tally.login.failures.enabled = false

When enabled, a count of failed authentication attempts is maintained by DeployR for each user account. The count is compared to the value specified on the following configuration property:

    deployr.security.tally.login.failures.limit = 10

If the count exceeds the `failures.limit` value, then the user account is locked and any further attempts to authentication on that account will be rejected with an error indicating that the account has been temporarily locked.

To manage locked user accounts, the administrator has two choices. The chosen behavior is determined by the value assigned to the following configuration property:

    deployr.security.tally.login.lock.timeout = 1800

If the `lock.timeout` value is set to 0, then locked user accounts must be manually unlocked by the administrator in the [Users tab](deployr-admin-console/deployr-admin-console-user-accounts.md#viewing-and-editing-user-accounts) of the Administration Console.

If the `lock.timeout` value (measured in seconds), is set to any non-zero, positive value then a locked user account will be automatically *unlocked* by DeployR once the `lock.timeout` period of time has elapsed without further activity on the account.

By default, automatic account *unlocking* is enabled and occurs 30 minutes after the last failed attempt at authentication on an account.


