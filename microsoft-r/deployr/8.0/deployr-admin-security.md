
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

** For Root Installs**

On each and every machine hosting a grid node:

1.  Log in as `root`.

2.  Before making any configuration changes to system files, stop Rserve and any other DeployR-related services:

        cd /opt/deployr/8.0.0
        ./stopAll.sh

3.  Grant `root` permission to launch the RServe process. This is required so that each DeployR grid node can enforce R session process controls.

    1.  Using your preferred editor, edit the file `/opt/deployr/8.0.0/rserve/rserve.sh` as follows:

		-   On Redhat/CentOS platforms, find the following section:
		
		        daemon --user "apache"
		
		    and, change `apache` to `root` as follows:
		
		        daemon --user "root"

		-   On SLES platforms, find the following section:
		
		        start_daemon -u r "apache"
		
		    and, change `apache` to `root` as follows:
		
		        start_daemon -u r "root"

    2.  Save this change and close the file in your editor.

4.  Set group privileges on the DeployR install directory.

        cd /opt
        chown -R apache.apache deployr
        chmod -R g+rwx deployr

5.  Execute the following command to add each user to the `apache` group.
    **Repeat for each user that will authenticate with the server.**

        usermod -a -G apache <some-username>

6.  Restart Rserve and any other DeployR-related services:

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
	        
                    $JAVA_HOME/bin/keytool -genkey -alias tomcat -keyalg RSA -keystore              /home/deployr-user/deployr/8.0.0/tomcat/tomcat7/.keystore

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

		>[!WARNING]
		>**Alert Your Users!**  
        >The following browser warning applies ONLY for self-signed certificates. When DeployR users attempt to open the DeployR landing page, Administration Console, or Repository Manager in their Web browser, they will be prompted to acknowledge and accept your self-signed certificate as a security precaution. Each browser prompts in a different way, such as requiring users to acknowledge "I Understand the Risks” (Firefox), or to click “Advanced” (Chrome) or click “Continue” (Safari). Please inform your users accordingly.  
        >We strongly recommend that you use a trusted SSL certificate from a registered authority in your production environments.


2.  **Next, enable SSL support for Tomcat.**
    + For Linux:
        >[!NOTE]
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
        >[!NOTE]
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

    [Learn more about server policies](https://deployr.revolutionanalytics.com/documents/help/admin-console//#Topics/policies-properties.htm).

4.  **Restart DeployR** by [stopping and starting all its services](https://deployr.revolutionanalytics.com/documents/admin/common/#server) so the changes can take effect. Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.  
     

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

    	[Learn more about server policies](https://deployr.revolutionanalytics.com/documents/help/admin-console//#Topics/policies-properties.htm).

3.  **Restart DeployR** by [stopping and starting all its services](https://deployr.revolutionanalytics.com/documents/admin/common/#server) so the changes can take effect. Between stopping and starting, be sure to pause long enough for the Tomcat process to terminate.  
     

4.  **Test** these changes by logging into the landing page and visiting DeployR Administration Console using the new HTTP URL at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine.

## Server Access Controls

### Working with IP Address Filters

While access to DeployR is typically controlled by the authentication mechanisms discussed in this document, DeployR also supports access controls based on IP address filters.

Under the [**Server Policies**](https://deployr.revolutionanalytics.com/documents/help/admin-console//#Topics/policies-properties.htm) tab in the DeployR Administration Console, you have a mechanism to configure your IP address filter policy. The `Operation Policies` for authenticated, asynchronous, and anonymous operations each support an [IP filter](https://deployr.revolutionanalytics.com/documents/help/admin-console//#Topics/policies-properties.htm) property. If you assign an IP filter to this property, then any attempt by a client application to connect from outside of the IP address range on that filter will be automatically rejected.

For example, you can make your DeployR server instance accessible only from IP addresses on the local LAN or VPN, such as `192.168.1.xxx` or `10.xxx.xxx.xxx`. Note that it is possible to achieve these same kinds of access controls with an appropriate configuration on your firewall and/or routers.

Refer to the [Administration Console Help](https://deployr.revolutionanalytics.com/documents/help/admin-console/) for further details on IP filters and server policies.

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
2.  Stop and restart the DeployR server using [these instructions](https://deployr.revolutionanalytics.com/documents/admin/common/#server).

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

You can change the access level on a repository-managed file using the `/r/repository/file/update` API call or using the [Repository Manager](https://deployr.revolutionanalytics.com/documents/help/repo-man//#e-file-properties.htm).

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

Repository-managed R scripts can be exposed as an executable on the API. Since repository-managed R scripts are a type of repository-managed file, all information in the [previous section](#repofileaccess) also applies to repository-managed scripts.

However, repository-managed R scripts deserve special mention since scripts can be managed through the Administration Console interface. Additionally, when you work with the R scripts in the Administration Console, you will likely also use and work with roles so as to impose restricted access to your R scripts.

>[!NOTE]
>For information on how to use roles as a means to restrict access to individual R scripts, refer to the [Administration Console Help](https://deployr.revolutionanalytics.com/documents/help/admin-console/#Topics/policies-properties.htm).

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

If the `lock.timeout` value is set to 0, then locked user accounts must be manually unlocked by the administrator in the [Users tab](https://deployr.revolutionanalytics.com/documents/help/admin-console/#Topics/user-view-edit.htm) of the Administration Console.

If the `lock.timeout` value (measured in seconds), is set to any non-zero, positive value then a locked user account will be automatically *unlocked* by DeployR once the `lock.timeout` period of time has elapsed without further activity on the account.

By default, automatic account *unlocking* is enabled and occurs 30 minutes after the last failed attempt at authentication on an account.


