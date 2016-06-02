---

# required metadata
title: " Security in DeployR"
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "05/06/2016"
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


# Enabling and Disabling Server SSL / HTTPS for DeployR

The **Secure Sockets Layer (SSL)** is a commonly-used protocol for managing the security of message transmissions on the Internet. Since we cannot ship SSL certificates for you, SSL on DeployR is disabled by default.

>We strongly recommended that SSL/HTTPS be enabled in **all production environments.**

## Enabling SSL Support
Once enabled your client applications can make API calls that connect over HTTPS.

### Enabling for DeployR for Microsoft R Server 2016

1. In your firewall, be sure to open the Tomcat HTTPS port (8051) to the outside on the DeployR server machine. If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the port.

1. If you are provisioning your server on a cloud service such as [Azure or an AWS EC2 instance](../deployr-admin-install-in-cloud.md), then you must also add endpoints for port 8051.

2. Launch the DeployR administrator utility script with administrator privileges to enable HTTPS:

    + For Linux
      ```
      cd /home/deployr-user/deployr/8.0.5/deployr/tools/ 
      ./adminUtility.sh
      ```

   + For Windows:
      ```
      cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
      adminUtility.bat
      ```

3.  From the main menu, choose option **Web Context and Security**.

4.  From the sub-menu, choose option **Configure Server SSL/HTTPS**.

5.  When prompted, answer `Y` to the question **Enable SSL?**

6.  When prompted to provide the full file path to the trusted SSL certificate file, type the full path to the file.

    If you do not have a trusted SSL certificate from a registered authority, you'll need a temporary keystore for testing purposes. [Learn how to create a temporary keystore](#temporary-keystore).

    >We recommend that you use a trusted SSL certificate from a registered authority **as soon as possible**.

7.  When prompted whether the certificate file is self-signed, answer `Y` if self-signed and `N` if you are using a trusted SSL certificate from a registered authority.

8.  Return to the main menu and choose the option **Start/Stop Server**. You must restart DeployR so that the changes can take effect.

9.  When prompted whether you want to stop (S) or restart (R) the DeployR server, enter `R`. It may take some time for the Tomcat process to terminate and restart.

10. Exit the utility.

11. Test these changes by logging into the landing page and visiting DeployR Administration Console using the new HTTPS URL at `https://<DEPLOYR_SERVER_IP>:8051/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine.

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

4.  **Restart DeployR** by [stopping and starting all its services](../deployr-common-administration-tasks.md#starting-and-stopping-deployr) so the changes can take effect. Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.  

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
      ./adminUtility.sh
      ```

   + For Windows:
      ```
      cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
      adminUtility.bat
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
