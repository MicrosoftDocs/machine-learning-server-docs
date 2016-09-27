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
    1. Secure client to 

2. SSL Port


## Enabling TLS/SSL Support

Once enabled, your client applications can make API calls that connect over HTTPS.



<br />
####Securing connections between the client application and the DeployR front end. 

1. Open the BLAH BLAH BLAH @@@@@@ port:
    1. In your firewall, be sure to open port BLAH BLAH BLAH @@@@@@  to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the port.

    1. If you are provisioning your server on a cloud service such as Azure or an AWS EC2 instance, then you must also add endpoints for port BLAH BLAH BLAH @@@@@@ .

1. Enable HTTPS in DeployR:

   1. Launch the utility with administrator privileges to enable HTTPS:

   1. STEPS UNKNOWN @@@@@@@@@@@@@

   1. When prompted to provide the full file path to the trusted SSL certificate file, type the full path to the file. 
   
      >If you do not have a trusted SSL certificate from a registered authority, you'll need a temporary keystore for testing purposes. [Learn how to create a temporary keystore](#temporary-keystore).

      >We recommend that you use a trusted SSL certificate from a registered authority **as soon as possible**.

   1. Return to the main menu and choose the option **Start/Stop Server**. You must restart DeployR so that the changes can take effect.

1. Test these changes by TRYING THIS @@@@@@.   If you are using an untrusted, self-signed certificate, and you or your users are have difficulty reaching DeployR in your browser, see this [Alert](#alertusers).


<br />
####Securing connections between DeployR Web server and the database

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

<a href="" id="temporary-keystore"></a>

### Generating a Temporary Keystore

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










## Disabling HTTPS Support

