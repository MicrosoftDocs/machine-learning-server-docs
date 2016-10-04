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

# Configuring DeployR for HTTPS

You should consider configuring DeployR for HTTPS in all production environments. 

>[!IMPORTANT] 
>For security reasons, we strongly recommend that TLS/SSL be enabled in **all production environments.**  Since we cannot ship TLS/SSL certificates for you, TLS/SSL protocols are disabled by default.

Both **Transport Layer Security** (TLS) protocol version 1.2 and its predecessor **Secure Sockets Layer (SSL)** are commonly-used cryptographic protocols for managing the security of message transmissions on the Internet. 

DeployR allows for HTTPS within a connection encrypted by TLS and/or SSL. To setup DeployR for HTTPS, you will definitely need the following things apart from some general network configurations around opening ports, updating firewall rules, and so on:

1. Three to four certificate

|Certificate|Description|Front-end|Back-end|
|------------------------------------|------------------------------------------------------------------------|--------------|---------------|
|Server-side HTTPS certificate|Used to secure coommunication between client applications and DeployR front-end|Yes, with private key|No|
|Back-end HTTPS certificate|Used to ????  Uses multi domain (SAN) certificate for all backends’ names (eg. Backend01, backend02, etc.) You can use a single Multi-Domain (SAN) certificate for all back-ends, or you can have a unique certificate for each back-end. If you use a SAN certificate, be sure that certificate has each unique back-end name specified as a subject alternative name.|No|Yes, with private key|
|Client authentication HTTPS certificate |Allows only the front-end to communicate with back-end|Yes, with private key|Yes, with public key|
|Access token signing certificate|Used when LDAP-S is enabled|Yes, with private key|No|
|Database certificate|??????|??|??|

2. SSL Port


> [!IMPORTANT]
> In production environments, we strongly recommend that you use a trusted SSL certificate from a registered authority **as soon as possible**. 
>
> If you do not have a trusted SSL certificate from a registered authority, you'll need a temporary keystore for testing purposes. [Learn how to create a temporary keystore](#temporary-keystore).




>**OPEN QUESTIONS:**
> - What versions of SSL and TLS will we support? Do the versions need to be the same on Win and Lin?
>
> - Windows Server 2012 supports TLS1.2, we should just say you can only use that or higher, I don’t know the specifics about linux
>
> - Since the front and back ends both need multiple certificates in certain scenarios, will we allow them to point to more than one certificate using the installer?
>
> - Will admins be able to determine if or what certificates have been defined after installation in an easy way?
 


## Enabling TLS/SSL Support

Once enabled, your client applications can make API calls that connect over HTTPS.

<br />

#### Secure connections between the client and DeployR

The following walks you through the steps for securing the connections between the client application and the DeployR front-end.

> The following steps assume that you have already installed a trusted SSL certificate from a registered authority to enable HTTPs and encrypt communication between client and front-end to prevent traffic from being modified or read.  This certificate must include a private key.


> WHICH PORTS TO OPEN TO THE OUTSIDE ON THE DEPLOYR FRONT END??

1. On each front-end, open the BLAH BLAH BLAH @@@@@@ port:

    1. In your firewall, open port BLAH BLAH BLAH @@@@@@  to the outside on the DeployR server machine. 
    
       If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the port.

    1. If provisioning DeployR on a cloud service such as Azure or an AWS EC2 instance, then you must also [create inbound security rules for port BLAH BLAH BLAH @@@@@@ in Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console.

   > WHAT MORE DO WE NEED TO SAY ABOUT CLOUD SETUPS?    
   >
   > Where do you enable HTTPS in DeployR?  The `appsettings.json` file again?? Or in the utility?

1. Enable HTTPS in DeployR:

   1. Launch the utility with administrator privileges to enable HTTPS:

   1. STEPS UNKNOWN @@@@@@@@@@@@@

      > DO WE TRUST SELF SIGNED IN THIS VERSION OF DEPLOYR?  Will we accept self-signed certificates at all now? Maybe there is no way to prevent them from using and a strong recommendation against is all we can do.

   1. When prompted to provide the full file path to the trusted SSL certificate file, type the full path to the file. 

1. **Start/Stop Server**. You must restart DeployR so that the changes can take effect.   @@@@ USE UTILITY

   > HOW CAN WE TEST THESE CHANGES???

1. If Active Directory/LDAP (AD/LDAP-S) is being used, also install an additional access token certificate, which includes a private key. 

1. Test these changes by TRYING THIS @@@@@@.   If you are using an untrusted, self-signed certificate, and you or your users are have difficulty reaching DeployR in your browser, see this [Alert](#alertusers).


<br />

#### Secure connections between the front-end and back-end

The following walks you through the steps for securing the connections between DeployR's front-end and each of its back-ends.

> The following steps assume that you have already installed a trusted SSL certificate from a registered authority with a private key to ensure that only recognized front-ends are accepted by and can communicate with the back-ends. This certificate must be installed on the front-end AND the back-end. 


1. On each front-end:

    1. Make sure the client authentication HTTPS certificate is installed with a private key. 
    
    1. In your firewall, open port BLAH BLAH BLAH @@@@@@  to the outside on the DeployR server machine. 
    
       If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the port.

    1. If provisioning DeployR on a cloud service such as Azure or an AWS EC2 instance, then you must also [create inbound security rules for port BLAH BLAH BLAH @@@@@@ in Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console.

   > WHAT MORE DO WE NEED TO SAY ABOUT CLOUD SETUPS?    
   >
   > WHAT ELSE DO WE NEED TO DO ON THE FRONT-END???
   >
   > WHAT DO WE NEED TO DO ON EACH BACK-END?

1. On each back-end, require a client certificate on each front-end. 

    1. Make sure the client authentication HTTPS certificate is installed with a private key. 

    1. WHAT IS THIS??? Uses multi domain (SAN) certificate for all backends’ names (eg. Backend01, backend02, etc.)



<br />

#### Secure connections between DeployR and the database

The following steps show how you can secure the connections between DeployR Web server and the remote database.

The following walks you through the steps for securing the connections between the DeployR front-end and the remote database.

> The following steps assume that you have already installed a trusted SSL certificate from a registered authority with a private key to ensure that only recognized front-ends are accepted by and can communicate with the back-ends.  


> WILL WE ENCOURAGE THEM TO SECURE BETWEEN DEPLOYR AND DATABASE????

If your corporate policies require that you secure the communications between the Web server and the DeployR database, then you should configure DeployR to use either a [SQL Server database](../deployr-install-on-windows.md#postgresql) or a [PostgreSQL database](../deployr-install-on-linux.md#postgresql) rather than the default H2 database.

After configuring DeployR to use one of those databases, you must also configure properly secure the database connections and force encryption.

+ For SQL Server 2016, you should: 

    + Read these articles for information on how to enable TLS for SQL Server: 
        + https://support.microsoft.com/en-us/kb/316898 
        + https://msdn.microsoft.com/en-us/library/ms378751(v=sql.110).aspx
        + https://support.microsoft.com/en-us/kb/3135244

    + In the `$DEPLOYR_HOME/deployr/deployr.groovy` external configuration file, add `encrypt=true;trustServerCertificate=true` to the connection string.

+ For PostgreSQL, you should:

    + Review the documentation here: https://www.postgresql.org/docs/9.1/static/ssl-tcp.html

    + In the `$DEPLOYR_HOME\deployr\deployr.groovy` external configuration file, add `ssl=true` to the `properties` section of the `dataSource` property block. For example:
      ```
      dataSource {
            dbCreate = "update"
            driverClassName = "org.postgresql.Driver"
            url = "jdbc:postgresql://localhost:5432/deployr"
            pooled = true
            username = "deployr"
            properties {
               ssl=true
               maxActive = -1
               minEvictableIdleTimeMillis=1800000
               timeBetweenEvictionRunsMillis=1800000
               numTestsPerEvictionRun=3
               testOnBorrow=true
               testWhileIdle=true
               testOnReturn=true
               validationQuery="SELECT 1"
            }
        }
      ```



<br />

#### DO WE DESCRIBE ANY OTHER CERTIFICATES???





#### Disabling HTTPS Support



<a href="" id="temporary-keystore"></a>

#### Generating a Temporary Keystore

If you do not have a trusted SSL certificate from a registered authority, you'll need a temporary keystore for testing purposes. This temporary keystore will contain a “self-signed” certificate for Tomcat SSL on the server machine. Be sure to specify the correct Tomcat path for the `-keystore` argument.

+ **For Windows:**

   1. Launch a command window **as administrator** 

   1. Run the `keytool` to generate a temporary keystore file. At the prompt, type:

      ```
      "<JAVA_HOME>\bin\keytool" -genkey -alias tomcat -keyalg RSA -keystore <PATH-TO-KEYSTORE>
      ```

      where `<JAVA_HOME>` is the path to the supported version of JAVA and `<PATH-TO-KEYSTORE>` is the full file path to the temporary keystore file.

   1. Provide the information when prompted by the script as described below in this topic.   
      
+ **For Linux:**

  1. Run the `keytool` to generate a temporary keystore file. At a terminal prompt, type:
     ```
     %JAVA_HOME%/bin/keytool -genkey -alias tomcat -keyalg RSA -keystore <PATH-TO-KEYSTORE>
     ```
     where `<PATH-TO-KEYSTORE>` is the full file path to the temporary keystore file.
	
  1. Provide the information when prompted by the script as described below in this topic.   

+ **For Mac OS X:** (DeployR 8.0.0 Open only)

  1. Run the `keytool` to generate a temporary keystore file. At a terminal prompt, type:
     ```
     $JAVA_HOME/bin/keytool -genkey -alias tomcat -keyalg RSA -keystore 
     /Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore
     ```
     where `<PATH-TO-KEYSTORE>` is the full file path to the temporary keystore file.
	
  1. Provide the information when prompted by the script as described below in this topic.   

When prompted by the script, provide the following information when prompted by the script:

+ For the keystore password, enter `changeit` and confirm this password.
+ For your name, organization, and location, either provide the information or press the Return key to skip to the next question.
+ When presented with the summary of your responses, enter `yes` to accept these entries.
+ For a key password for Tomcat, press the Return key to use `changeit`.

<a href="" id="alertusers"></a>
>**Alert Your Users!**
>
>If using a self-signed certificates, then alert your users.When they attempt to open the DeployR landing page, Administration Console, or Repository Manager in their Web browser, they will be prompted to acknowledge and accept your self-signed certificate as a security precaution. Each browser prompts in a different way, such as requiring users to acknowledge "I Understand the Risks” (Firefox), or to click “Advanced” (Chrome) or click “Continue” (Safari).
>
>We strongly recommend that you use a trusted SSL certificate from a registered authority in your production environments.








