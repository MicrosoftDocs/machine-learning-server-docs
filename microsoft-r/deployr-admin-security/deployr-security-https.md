---

# required metadata
title: " Security in DeployR"
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "06/27/2016"
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
ms.technology: "deployr"
ms.custom: ""

---


# DeployR Server Support for SSL/TLS (HTTPS)

Both **Transport Layer Security** (TLS) protocol version 1.2 and its predecessor **Secure Sockets Layer (SSL)** are commonly-used cryptographic protocols for managing the security of message transmissions on the Internet. 

DeployR allows for HTTPS within a connection encrypted by TLS and/or SSL. 

+ In DeployR Enterprise for Microsoft R Server 2016 (v8.0.5), the DeployR Web server as well as all APIs calls and utilities support TLS 1.2 and SSL. However, HTTPS is disabled by default.

+ In DeployR 8.0.0, only SSL is supported.  

## Enabling TLS/SSL Support

Once enabled, your client applications can make API calls that connect over HTTPS.

>[!IMPORTANT] 
>For security reasons, we strongly recommend that TLS/SSL be enabled in **all production environments.**  Since we cannot ship TLS/SSL certificates for you, TLS/SSL protocols on DeployR are disabled by default.

<br />
### Enabling for DeployR for Microsoft R Server 2016

<br />
####Securing connections between the DeployR Web server and client

1. In your firewall, be sure to open the Tomcat HTTPS port (8051) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the port.

1. If you are provisioning your server on a cloud service such as [Azure or an AWS EC2 instance](../deployr-admin-install-in-cloud.md), then you must also add endpoints for port 8051.

1. Enable HTTPS in the DeployR administrator utility:

   1. Launch the utility with administrator privileges to enable HTTPS:

      + For Linux
        ```
        cd /home/deployr-user/deployr/8.0.5/deployr/tools/ 
        sudo ./adminUtilities.sh
        ```

     + For Windows:
        ```
        cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
        adminUtilities.bat
        ```

   1. From the main menu, choose option **Web Context and Security**.

   1. From the sub-menu, choose option **Configure Server SSL/HTTPS**.

   1. When prompted, answer `Y` to the question **Enable SSL?**

   1. When prompted to provide the full file path to the trusted SSL certificate file, type the full path to the file. 
   
      >If you do not have a trusted SSL certificate from a registered authority, you'll need a temporary keystore for testing purposes. [Learn how to create a temporary keystore](#temporary-keystore).

      >We recommend that you use a trusted SSL certificate from a registered authority **as soon as possible**.

   1. When prompted whether the certificate file is self-signed, answer `N` if you are using a trusted SSL certificate from a registered authority -or- `Y` if self-signed.

   1. Return to the main menu and choose the option **Start/Stop Server**. You must restart DeployR so that the changes can take effect.

   1. When prompted whether you want to stop (S) or restart (R) the DeployR server, enter `R`. It may take some time for the Tomcat process to terminate and restart.

   1. Exit the utility.

1. Test these changes by logging into the landing page and visiting DeployR Administration Console using the new HTTPS URL at `https://<DEPLOYR_SERVER_IP>:8051/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine.

<!--
<br />
####Securing connections between DeployR Web server and the database

If your corporate policies require that you secure the communications between the Web server and the DeployR database, then you should configure DeployR to use either a [SQL Server database](../deployr-install-on-windows.md#postgresql) or a [PostgreSQL database](../deployr-install-on-linux.md#postgresql) rather than the default H2 database.

To encrypt the connections between the web server and a **remote SQL Server database**:

1.  In the DeployR administrator utility, [stop the DeployR server](../deployr-common-administration-tasks.md#startstop).

1.  Update the database connection string properties for encryption:

    1.  Open the `$DEPLOYR_HOME\deployr\deployr.groovy` file, which is the DeployR server external configuration file.

    2.  Locate the `dataSource` property block.

    3.  Replace **the entire contents** of the `dataSource` block with these for your authentication method:

        For Windows authentication:

              dataSource {
                   dbCreate = "update"
                   driverClassName = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
                   url = "jdbc:sqlserver://localhost;instanceName=<PUT_INSTANCE_NAME_HERE>;database=deployr;integratedSecurity=true;"
              }

        For basic authentication:

              dataSource {
                   dbCreate = "update"
                   driverClassName = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
                   username = “<PUT_DB_USERNAME_HERE>”
                   password = “<PUT_DB_PASSWORD_HERE>”
                   url = "jdbc:sqlserver://localhost;instanceName=<PUT_INSTANCE_NAME_HERE>;database=deployr;"
              }

        >If you are unsure of your SQL instance name, please consult with your SQL administrator. The default instance name for:
        >    -   SQL Server Express is `SQLEXPRESS`
        >    -   SQL Server Standard or Professional is `MSSQLEXPRESS`
        >
        >If you are using a remote database, use the IP address or FQDN of the remote machine rather than `localhost`.

1. If you are connecting to a remote SQL Server database, be sure to [open the database port to the public IP of the DeployR server](#update-your-firewall).

1. In the DeployR administrator utility:
    1. Test the database connection.
        -   If there are any issues, solve them before continuing.
        -   Once the connection test passes, return the main menu.

    1. [Restart the DeployR server](../deployr-common-administration-tasks.md#startstop).

    1.  Exit the utility.

1. There are additional steps in SQL Server 2016 needed to properly secure the connections and force encryption. Read more here: 
    + https://msdn.microsoft.com/en-us/library/ms378751(v=sql.110).aspx
    + https://msdn.microsoft.com/en-us/library/ms191192.aspx
    + https://support.microsoft.com/en-us/kb/3135244

-->


<br>
### Enabling for DeployR 8.0.0

1. **Provide an SSL certificate.**
   + If you have a trusted SSL certificate from a registered authority, then copy it to the Tomcat directory so it can be deployed at startup. (If you do not have one, skip to the next bullet to define a temporary certificate.)
        
      >Be sure to specify the correct Tomcat path for the `-keystore` argument.
	
      + For Linux / OS X:

        >This example is written for user `deployr-user`. For another user, use the appropriate filepath to the `.keystore`.

        1. Go to the directory in which the keystore is stored.

        2. Copy the certificate keystore to the Tomcat directory. At the prompt, type:
           ```
           cp .keystore $DEPLOYR_HOME/tomcat/tomcat7/.keystore
           ```
           
      + For Windows:

        1. Go to the directory in which the keystore is stored.

        1. Launch a command window as administrator and type the following at the prompt:
           ```           
           copy .keystore  C:\Program Files\Microsoft\DeployR-8.0\Apache_Tomcat\bin\.keystore
           ```
           
    + If you do not yet have a trusted SSL certificate from a registered authority, then create a temporary keystore for testing purposes. This temporary keystore will contain a “self-signed” certificate for Tomcat SSL on the server machine. [Learn how to create a temporary keystore](#temporary-keystore).


2. **Next, enable SSL support for Tomcat.**

   + For Linux / OS X:
    
     >This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` and `web.xml` as well as the `keystoreFile` property on the Connector.

     1. Enable the HTTPS connector on Tomcat by **removing the comments** around the following code in the file `$DEPLOYR_HOME/tomcat/tomcat7/conf/server.xml`.

        ```
        <!-- 
        <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="<$DEPLOYR_HOME>/tomcat/tomcat7/.keystore" />
        -->
        ```

     1. Force Tomcat to upgrade all HTTP connections to HTTPS connections by **removing the comments** around the following code in the file `$DEPLOYR_HOME/tomcat/tomcat7/conf/web.xml`.

        ```
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
        ```
        
      1. Be sure to open the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the iptables command (or equivalent command/tool) to open the port.
	 
      1. If you are provisioning your server on a cloud service such as [Azure or AWS EC2](../deployr-admin-install-in-cloud.md), then you must also add endpoints for port 8001.

   + For Windows:

     1. Enable the HTTPS connector/channel on Tomcat by **removing the comments** around the following code in the file `C:\Program Files\Microsoft\DeployR-8.0\Apache_Tomcat\conf\server.xml`.

        ```
        <!-- 
        <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="C:\Program Files\Microsoft\DeployR-8.0\Apache_Tomcat\bin\.keystore" />
        -->
        ```
        
     1.  Force Tomcat to upgrade all HTTP connections to HTTPS connections by **removing the comments** around the following code in the file `C:\Program Files\Microsoft\DeployR-8.0\Apache_Tomcat\conf\web.xml`.

        ```
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
        ```

     1. Be sure to open the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the iptables command (or equivalent command/tool) to open the port.

     1. If you are provisioning your server on a cloud service such as [Azure or AWS EC2](../deployr-admin-install-in-cloud.md), then you must also add endpoints for port 8001.

3. **Then, enable SSL support for DeployR.**

   + For Linux / OS X:

     1. Enable SSL support on the Administration Console by changing `false` to `true` in the following line of the DeployR external configuration file, `$DEPLOYR_HOME/deployr/deployr.groovy`:
        ```
        grails.plugins.springsecurity.auth.forceHttps = false
        ```
        
     1. Enable HTTPS in the server policies so that any non-HTTPS connections to the server are automatically rejected.  Run the `setWebContext.sh` script and specify the value of `true` for the `https` argument:
        ```
        $DEPLOYR_HOME/deployr/tools/setWebContext.sh -https true
        ```

   + For Windows:

     1. Enable SSL support on the Administration Console by changing `false` to `true` in the following line of the DeployR external configuration file, `C:\Program Files\Microsoft\DeployR-8.0\deployr/deployr.groovy`:
        ```
        grails.plugins.springsecurity.auth.forceHttps = false
        ```
        
     1. Enable HTTPS in the server policies so that any non-HTTPS connections to the server are automatically rejected. Run the `setWebContext.bat` script and specify the value of `true` for the `https` argument:
        ```
        C:\Program Files\Microsoft\DeployR-8.0\deployr\tools\setWebContext.bat -https true
        ```
        
     Upon completion of this script with `-https true`, the following changes will have been made to the [server policies](../deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties) in the Administration Console:
     + The server web context now ressembles `https://xx.xx.xx.xx:8001/deployr` instead of `http://xx.xx.xx.xx:8000/deployr`.
     + The `Enable HTTPS` property for each of operation policies (authenticated, anonymous, and asynchronous) are all checked.

4.  **Restart DeployR** by [stopping and starting all its services](../deployr-common-administration-tasks.md#startstop) so the changes can take effect. Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.  

5.  **Test** these changes by logging into the landing page and visiting DeployR Administration Console using the new HTTPS URL at `https://<DEPLOYR_SERVER_IP>:8001/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine. If you are using an untrusted, self-signed certificate, and you or your users are have difficulty reaching DeployR in your browser, see the [Alert](#alertusers) at the end of step 1.

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










## Disabling SSL Support

### Disabling for DeployR for Microsoft R Server 2016

The **Secure Sockets Layer (SSL)** is a commonly-used protocol for managing the security of message transmissions on the Internet. By default, SSL on DeployR is disabled. If you have enabled SSL at some time and you now wish to disable SSL, follow the steps in this section.

>We strongly recommended that SSL/HTTPS be enabled in **all production environments.**

1. In the firewall, be sure to close the Tomcat HTTPS port (8051) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to close the port.

1. If you are provisioning your server on a cloud service such as [Azure or an AWS EC2 instance](../deployr-admin-install-in-cloud.md), then you must also remove endpoints for port 8051.

1. Launch the DeployR administrator utility script with administrator privileges to disable HTTPS:

    + For Linux
      ```
      cd /home/deployr-user/deployr/8.0.5/deployr/tools/ 
      ./adminUtilities.sh
      ```

   + For Windows:
      ```
      cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
      adminUtilities.bat
      ```

1.  From the main menu, choose option **Web Context and Security**.

1.  From the sub-menu, choose option **Configure Server SSL/HTTPS**.

1.  When prompted, answer `Y` to the question **Disable SSL?**

1.  Return to the main menu and choose option **Start/Stop Server**. You must restart DeployR so that the change can take effect.

1.  Enter `R` to restart the server. It may take some time for the Tomcat process to terminate and restart.

1. Test these changes by logging into the landing page and visiting DeployR Administration Console using the new HTTP URL at `https://<DEPLOYR_SERVER_IP>:8050/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine.


### Disabling for DeployR 8.0.0

1. **Disable SSL support for Tomcat.**

   + For Linux / OS X:

     >This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` as well as the `keystoreFile` property on the Connector. For another user,also use the appropriate filepath to `web.xml`.

     1. Disable the HTTPS connector on Tomcat by **commenting out** the following code in the file `$DEPLOYR_HOME/tomcat/tomcat7/conf/server.xml`.

        <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="<$DEPLOYR_HOME>/tomcat/tomcat7/.keystore" />

     1. Be sure to close the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to close the port.

     1. If you are provisioning your server on a cloud service such as [Azure or AWS EC2](../deployr-admin-install-in-cloud.md), then you must also remove endpoints for port 8001.

     1. Disable the upgrade of all HTTP connections to HTTPS connections by **commenting out** the following code in the file `$DEPLOYR_HOME/tomcat/tomcat7/conf/web.xml`.

        ```
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
        ```

    + For Windows:

      >This example is written for `deployr-user`. For another user, use the appropriate filepath to `server.xml` as well as the `keystoreFile` property on the Connector. For another user,also use the appropriate filepath to `web.xml`.

    	1. Disable the HTTPS connector/channel on Tomcat by **commenting out** the following code in the file `C:\Program Files\Microsoft\DeployR-8.0\Apache_Tomcat\conf\server.xml`.

                 <Connector port="8001" protocol="org.apache.coyote.http11.Http11NioProtoocol" compression="1024" compressableMimeType="text/html,text/xml,text/json,text/plain,application/xml,application/json,image/svg+xml" SSLEnabled="true" maxthreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" keystoreFile="C:\Program Files\Microsoft\DeployR-8.0\Apache_Tomcat\bin\.keystore" />

    	1. Be sure to close the Tomcat HTTPS port (8001) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to close the port.

        1. If you are provisioning your server on a cloud service such as [Azure or AWS EC2](../deployr-admin-install-in-cloud.md), then you must also remove endpoints for port 8001.

    	1. Disable the upgrade of all HTTP connections to HTTPS connections by **commenting out** the following code in the file `C:\Program Files\Microsoft\DeployR-8.0\Apache_Tomcat\conf\web.xml`.

        ```
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
        ```

2. **Next, disable SSL support for DeployR.**

   + For Linux / OS X:

     1. Disable SSL support on the Administration Console by changing `true` to `false` in the following line of the DeployR external configuration file, `$DEPLOYR_HOME/deployr/deployr.groovy`:
        ```
        grails.plugins.springsecurity.auth.forceHttps = true
        ```
        
     1. Disable HTTPS in the server policies. Run the `setWebContext.sh` script and specify the value of `false` for the `https` argument:
        ```
        $DEPLOYR_HOME/deployr/tools/setWebContext.sh -https false
        ```
        
   + For Windows:

     1. Enable SSL support on the Administration Console by changing `false` to `true` in the following line of the DeployR external configuration file, `C:\Program Files\Microsoft\DeployR-8.0\deployr/deployr.groovy`:
        ```
        grails.plugins.springsecurity.auth.forceHttps = true
        ```
        
     1. Run the `setWebContext.bat` script and specify the value of `false` for the `https` argument:
        ```
        C:\Program Files\Microsoft\DeployR-8.0\deployr\tools\setWebContext.bat -https false
        ```
        
Upon completion of the `setWebContext` script with `-https false`, the following changes will have been made to the [server policies](../deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties) in the Administration Console:

+ The server web context now ressembles `http://xx.xx.xx.xx:8000/deployr`.

+ The `Enable HTTPS` property for each of operation policies (authenticated, anonymous, and asynchronous) are disabled.
