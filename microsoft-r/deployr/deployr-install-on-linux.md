---

# required metadata
title: "Install DeployR for Microsoft R Server 2016 on Linux | DeployR 8.x"
description: "How to install, migrate, and configure DeployR"
keywords: "install, installation, DeployR, configuration, configure"
author: "j-martens"
manager: "jhubbard"
ms.date: "08/01/2016"
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
# Installing DeployR for Microsoft R Server 2016 (8.0.5) on Linux

**Applies to: DeployR 8.x**   (See [comparison between 8.x and 9.x](../rserver-whats-new.md#8vs9))

>Looking for docs for Microsoft R Server 9? [Start here](../deployr-repository-manager/about.md).

##Before You Begin

Read and follow these points before you begin the installation process.

>[!IMPORTANT]
>-   If you already have a version of DeployR installed, follow the [migration instructions](#migrate) first.
>
>-   We highly recommend installing DeployR on a dedicated machine. Always install the DeployR main server first before any grid nodes.
>
>-   While it is technically possible to run instances of two different versions of DeployR side-by-side on a single machine, we strongly recommend that you dedicate one machine for each server instance that is *in production* so as to avoid resource contention.
>

<a name="system-requirements"></a>
## System Requirements

Verify that the computer meets the following minimum hardware and software requirements.

_Table: System Requirements_

|System&nbsp;Requirement|Value  | 
|-----------------------|--------------------------|
|Operating Systems| SUSE Linux Enterprise Server 11 (SP2)<br>CentOS / RHEL 5.8, 6.x <br><small>(64-bit processor only)</small>|
|Hardware|Intel Pentium®-class processor; 3.0 GHz recommended|
|Free disk space|250+ GB recommended|
|RAM|4+ GB recommended|
|Java JVM heap size|1+ GB in production|
|Swap space|8+ GB for larger data sets|
|Internet access|To download DeployR and any dependencies, interact with the Repository Manager, Administration Console, API Explorer Tool.|

<a name="depend"></a>
## Install Dependencies

Before you can install DeployR, you must manually install and configure the following dependencies.

| Dependency                                                                                      | DeployR Server                                                                      | DeployR Grid Node                                                                   |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Java™ Runtime Environment 8                                                                     | Yes                                                                                 | No                                                                                  |
| [Microsoft R Server 2016](../rserver-install-linux-server.md) and its dependencies | Yes                                                                                 | Yes                                                                                 |
| DeployR Rserve 8.0.5                                                                            | Yes                                                                                 | Yes                                                                                 |
| make, gcc, gcc-c++, gfortran, cairo-devel, libicu, libicu-devel                                          | Yes                                                                                 | Yes                                                                                 |
| nfs-utils and nfs-utils-lib <br />Note: On Ubuntu, install nfs-common to get nfs-utils.  | Yes, for <br />[external directories](deployr-admin-manage-big-data.md)  | Yes, for <br />[external directories](deployr-admin-manage-big-data.md)  |
.

**To install the required dependencies:**

1.  Install dependencies as `root` or a user with `sudo` permissions.

2.  **ONLY** the main DeployR server, install **Java™ Runtime Environment 8**, a dependency of DeployR, as follows. For more help installing Java or setting the path, refer to oracle.com.

    -   [Download the tar file from Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and install it **OR** install using your local package manager.

    -   Set `JAVA_HOME` to the installation path for the JDK or JRE you just installed.
        For example, if the installation path was `/usr/lib/jvm/jdk1.8.0_45/jre`, then the command would be:

            export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_45/jre

3.  Install [Microsoft R Server 2016](../rserver-install-linux-server.md), which includes ScaleR for multi-processor and big data support. Microsoft R Server can be downloaded from Volume License Service Center, MSDN and Visual Studio Dev Essentials. The file you download will also contain the DeployR installer.

1.  Make sure the system repositories are up-to-date prior to installing DeployR. The following commands *do not install anything*; however running them will ensure that the repositories contain the latest software. Run the following command:
    + For Redhat / CentOS:
      ```
      sudo yum clean all
      ```
      
    + For Ubuntu:
      ```
      sudo apt-get update
      ```
      
    + For OpenSUSE / SLES:
      ```
      sudo zypper clean --all
      ```

1.  Install the following packages (`make`, `gcc`, `gcc-c++`, `gfortran`, `cairo-devel`, `libstdc++`, and `libicu-devel`) if any of them are missing as follows:

    >Install packages as `root` or a user with `sudo` permissions.

	+ For Redhat / CentOS, check if the required packages are already installed and install any missing packages as follows:
      ```
      #VERIFY ALL PACKAGE DEPENDENCIES ARE INSTALLED
      yum list make gcc gcc-c++ gfortran cairo-devel libicu-devel libstdc++
      
      #INSTALL ALL MISSING PACKAGES
  
      #ONE LINE PER MISSING PACKAGE
      yum install <missing_package_name>
      ```
    	
	+ For Ubuntu, check if the required packages are already installed and install any missing packages as follows:
      ```
      #VERIFY ALL PACKAGE DEPENDENCIES ARE INSTALLED
      dpkg -l make gcc gcc-c++ gfortran cairo-devel libicu-devel libstdc++ licui18n
      
      #INSTALL ALL MISSING PACKAGES
  
      #ONE LINE PER MISSING PACKAGE
      sudo apt-get install <missing-package-name>
      ```

	+ For SLES, check if the required packages are already installed and install any missing packages as follows:
      ```
      #VERIFY ALL PACKAGE DEPENDENCIES ARE INSTALLED
      sudo zypper search -i <package-name>
      
      #INSTALL ALL MISSING PACKAGES
  
      #ONE LINE PER MISSING PACKAGE
      sudo zypper install <missing-package-name>
      ```
1.  Install **DeployR Rserve 8.0.5**, a dependency of DeployR, after Microsoft R Server 2016 as follows:

    A. [Download the Linux tar file for DeployR Rserve 8.0.5, `deployrRserve_8.0.5.tar.gz`](https://github.com/Microsoft/deployr-rserve/releases/).

    B. Run the following command to install the DeployR Rserve component:

        R CMD INSTALL deployrRserve_8.0.5.tar.gz


      
<a name="installserver"></a>
## Install DeployR Server

The basic installation of DeployR will install the DeployR main server and configure a local H2 database on the same machine. If you wish to use a different database, you can configure DeployR to do so later as described in the steps below.

In addition to this basic installation, DeployR Enterprise customers can also use a [remote database for DeployR](#postgresql) or install [additional grid nodes](#gridnodes) for optimized workload distribution.

The following steps are for installing DeployR Enterprise after installing [these dependencies](#depend):

1.  Log into the operating system as `root` or a user with `sudo` permissions.

    >Examples are written for user `deployr-user`. For another user, update the commands accordingly.

2.  Create the `deployrdownload` directory and go to that directory. At the prompt, type:

        mkdir /home/deployr-user/deployrdownload
        cd /home/deployr-user/deployrdownload

3.  Find the software, `DeployR-Enterprise-Linux-8.0.5.tar.gz` from within the Microsoft R Server 2016 package you downloaded in the prerequisites step.

4.  Unzip the tar file contents, go the `deployrInstall/installFiles` directory, and launch the installation script. At the prompt, type:

        tar -xzf DeployR-Enterprise-Linux-8.0.5.tar.gz
        cd deployrInstall/installFiles
        ./installDeployREnterprise.sh

5.  When the installer starts, accept the terms of the agreement to continue.

6.  From the installer menu, choose option `1`. This will install the DeployR server along with a local H2 database.

7.  Follow the remaining onscreen installer prompts. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

1.  Review and follow these critical [post-installation steps](#postinstall). You will not be able to log into the server until you set a password.  

<a name="postinstall"></a>
##Post Installation Steps

The following steps outline what you need to do after running the DeployR installer. 

1.  **Set the administrator's password** so you can log into the server and its landing page.

    1. Launch the DeployR Administrator Utility script  as `root` or a user with `sudo` permissions:
       ```
       cd /home/deployr-user/deployr/8.0.5/deployr/tools/ 
       ./adminUtilities.sh
       ```
       
    1. From the main menu, choose the option to set a password for the local DeployR `admin` account.
    
    1. Enter a password for this account. Passwords must be **8-16 characters** long and contain at least 1 or more uppercase character(s), 1 or more lowercase character(s), 1 or more number(s), **and** 1 or more special character(s).
    
    1. Confirm the password.
    
1. **Log into the DeployR landing page** as `admin` to test your newly defined password at `http://<DEPLOYR_SERVER_IP>:8050/deployr/landing`.

   >At this point, you will only be able to login locally using `localhost`. You will be able to login remotely only once you've [configure public access](#configuring-public-access) in a later step in this section.

1. If desired, **set up grid nodes**. You can install and configure any [additional grid nodes](#gridnodes).

1. If you want to [use non-default port numbers for DeployR](deployr-admin-diagnostics-troubleshooting.md#changeport), manually update them now.

   >_HortonWorks Data Platform User Alert!_ &nbsp; Both DeployR and the [HDP Resource Manager](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_Reference_Guide/content/yarn-ports.html) use the same default port of 8050. To avoid conflicts, you can [change the DeployR port](deployr-admin-diagnostics-troubleshooting.md#changeport). 

1. If you want to **use a PostgreSQL database** locally or remotely instead of the default local H2 database, configure that as [described here](#postgresql).

1. If you want to **provision DeployR on Azure or AWS** as described in [these steps](deployr-admin-install-in-cloud.md).

1. **Run diagnostic tests**. Test the install by running the full [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

1. **Review security documentation** and consider **enabling HTTPs**. Learn more by reading the [Security Introduction](../deployr-admin-security/deployr-security.md) and the [Enabling HTTPs](deployr-security-https.md) topic.

   >We strongly recommended that SSL/HTTPS be enabled in **_all production environments_**.

1. **Check the web context**. If the wrong IP was detected during installation, [update that Web context](#configure-public-access) now.

1. **Get DeployR client-side developer tools**, including the [RBroker framework and client libraries](../deployr-tools-and-samples.md).

1. **Create accounts for your users** in the [Administration Console](../deployr-admin-console/deployr-admin-console-user-accounts.md). Then, provide each user with their username and password as well as the address of the DeployR landing page.

1. **Learn more**. Read the [Administrator Getting Started](deployr-administrator-getting-started.md) guide. You can also read and share the [Data Scientist Getting Started](deployr-data-scientist-getting-started.md) and the [Application Developer Getting Started](deployr-application-developer-getting-started.md) guides.

## Configuring DeployR

For the best results, complete these configuration topics in the order in which they are presented.

<a name="firewall"></a>
### Update Your Firewall

If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the following ports:

| Machine                   | Ports                                 | Open Ports                                                                          |
|---------------------------|---------------------------------------|-------------------------------------------------------------------------------------|
| DeployR server machine    | Tomcat ports: <br />- `8050` (Tomcat default port)<br />- `8051` (Tomcat HTTPS port)           | To the outside                                                                      |
| DeployR server machine    | - `8056` (DeployR event console port) | To the public IP of DeployR server AND to the public IP of *each* grid node machine |
| Remote grid node machines | DeployR Rserve ports:<br />- `8054` (RServe connection port)<br />- `8055` (RServe cancel port)          | To the public IP of the DeployR server                                              |


If any of the following cases exist, update your firewall manually:

-   Whenever you use **non-default port numbers** for communications between DeployR and its dependencies, add those port numbers instead of those in the table above.

-   If connecting to a **remote PostgreSQL database**, be sure to open port 5432 to the public IP of the DeployR server.

-   If defining NFS ports for **external directory support**, see the Configuration section of the [Managing External Directories for Big Data](deployr-admin-manage-big-data.md#setting-up-nfs-setup) guide.

> If provisioning DeployR on a **cloud service**, configure endpoints for these ports on your [Azure or AWS EC2 instance](deployr-admin-install-in-cloud.md), or enable port-forwarding for VirtualBox.
>[You can change any DeployR ports](deployr-admin-diagnostics-troubleshooting.md#changeport).                                                                                     
<a name="configuring-public-access"></a>
### Configure Public Access

When the wrong IP is defined, you will not be able to access to the DeployR landing page or other DeployR components after installation or reboot. In some cases, the wrong IP address may be automatically assigned during installation or when the machine is rebooted, and that address may not be the only IP address for this machine or may not be publicly accessible. If this case, you must update the server Web context address.

To fix this issue, you must define the appropriate external server IP address and port number for the **Server web context** and disable the autodetection of the IP address.

**To make changes to the IP address:**

1.  Launch the DeployR administrator utility script as `root` or a user with `sudo` permissions:

        cd /home/deployr-user/deployr/8.0.5/deployr/tools/ 
        ./adminUtilities.sh

2.  From the main menu, choose option `3` to configure the Web Context and Security options for DeployR.

3.  From the sub-menu, enter `A` to specify a different IP or fully qualified domain name (FQDN).

4.  Specify the new IP or FQDN. For [Azure or AWS EC2 instances](deployr-admin-install-in-cloud.md) services, set it to the external **Public IP**.

5.  Confirm the new value. Note that the IP autodetection will be turned off when you update the IP/FQDN.

6.  Return to the main menu.

7.  To apply the changes, restart the DeployR server using option `2` from the main menu.

<a name="gridnodes"></a>
### Install DeployR Grid Nodes

When you install the DeployR server, one local grid node is installed automatically for you. You can also point this default grid node to a remote location, customize its slot limit, and even add additional grid nodes to scale for increasing load. This option also assumes that you have already installed the main [DeployR server](#installserver).

>[!TIP]
>-   For help in determining the right number of grid nodes for you, refer to the [Scale & Throughput](deployr-admin-scale-and-throughput.md#tuning-grid-capacity) document.
>-   Once you have installed and configured one grid node, you can copy the files over from the server that has already been set up over to the next one.
>-   Always install the main DeployR server first.
>-   Install each grid node on a separate host machine.

**To install DeployR grid nodes:**

_After installing the [main server for DeployR Enterprise](#installserver)_, install each grid node on a separate machine as follows:

1.  Log into the operating system on the machine on which you will install the grid node as `root` or a user with `sudo` permissions.

2.  Install Microsoft R Server 2016 and the DeployR Rserve component [as described here](#depend) on the grid node machine.

3.  Create the `deployrdownload` directory and go to that directory. At the prompt, type:

    >Examples are written for user `deployr-user`. For another user, update the commands accordingly.

        mkdir /home/deployr-user/deployrdownload
        cd /home/deployr-user/deployrdownload

4.  Download the software, `DeployR-Enterprise-Linux-8.0.5.tar.gz`, using the link in your customer welcome letter.

5.  Unzip the tar file contents, go the `installFiles` directory, and launch the installation script. At the prompt, type:

        tar -xzf DeployR-Enterprise-Linux-8.0.5.tar.gz
        cd installFiles
        ./installDeployREnterprise.sh

6.  When the installer starts, accept the terms of the agreement to continue.

7.  When prompted by the installer, choose installation option `2` and follow the onscreen installer prompts. This will install a remote grid node.

8.  Enter the directory path in which to install. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

<br>
<br>
**To configure & validate nodes:**

After installing DeployR Enterprise server and any grid node machines, you must configure these grid nodes as follows:

1. [Set the proper firewall rules](#firewall) to open the RServe ports ONLY to the public IP of the DeployR server.

1. Log into the DeployR landing page as `admin` at http://&lt;DEPLOYR\_SERVER\_IP&gt;:8050/deployr/landing where `<DEPLOYR_SERVER_IP>` is the IP of the main DeployR server.

2.  Go to the **Administration Console**.

3.  Click **The Grid** in the main menu.

4.  For each remote grid node you installed earlier, do the following: (You do not need to configure the DeployR Default Node.)

    1.  Click **New Grid Node**.

    2.  Configure the **Name**, **Host**, **Operating Type** and **External Directory** &nbsp; [using these instructions](deployr-admin-managing-the-grid.md#creating-new-nodes). 
    
    3.  When you try to add that new grid node configuration, DeployR will attempt to validate your settings. [Learn more...](deployr-admin-managing-the-grid.md#node-validation-and-errors)

    4.  Run a diagnostic test of each grid node individually as follows:

        1.  Enable **only** that node in the main **The Grid** tab.

        2.  Return to the landing page to run the [diagnostic check](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for help.

    5.  Repeat these steps for each grid node.

    6.  Remember to go back and enable all the grid nodes you want to use when you are done testing.

<a name="postgresql"></a>
### Use a PostgreSQL Database

During the installation of DeployR, a local H2 database is automatically installed and configured for you. After installing DeployR, **but before using it**, you can configure DeployR to use a database in **PostgreSQL 9.1 or greater.**

If you want to use a local or remote PostgreSQL database for DeployR instead of the default local H2 database, you'll need to:

1.  Install and configure PostgreSQL as described for that product.

2.  Create a database with the name `deployr`. Use the owner of this database as the username in the `dataSource` block in a later step.

3.  Assign the proper permissions to the database user to read and write into the database.

4.  [Download the JDBC42 Postgresql Driver for JDK 1.8](https://jdbc.postgresql.org/download.html) for the version of the database you installed and copy them under **both** of the following folders:

    -   `$DEPLOYR_HOME/tomcat/tomcat7/lib`

    -   `$DEPLOYR_HOME/deployr/tools/lib`

5.  [Stop the DeployR server](deployr-common-administration-tasks.md#startstop).

6.  Update the database properties to point to the new database as follows:

    1.  Open the `$DEPLOYR_HOME\deployr\deployr.groovy` file, which is the DeployR server external configuration file.

    2.  Locate the `dataSource` property block.

    3.  Replace the contents of that block with these properties and specify the appropriate port number, username, and password:

            dataSource {
              dbCreate = "update"
              driverClassName = "org.postgresql.Driver"
              url = "jdbc:postgresql://localhost:<PUT_PORT_HERE>/deployr"
              pooled = true
              username = "<PUT_DB_USERNAME_HERE>"
              password = "<PUT_DB_PASSWORD_HERE>"
              }

	>Put the owner of the `deployr` database as the username. For more information, see <http://www.postgresql.org/docs/9.1/static/sql-alterdatabase.html>.
        >
        >If you are using a remote database, use the IP address or FQDN of the remote machine rather than `localhost`.

7.  If you are connecting to a remote PostgreSQL database, be sure to [open the database port to the public IP of the DeployR server](#firewall).

8.  Test the connection to the database and restart the server as follows:

    1.  Launch the DeployR administrator utility script as `root` or a user with `sudo` permissions:

            cd $DEPLOYR_HOME/deployr/tools/ 
            sudo ./adminUtilities.sh

    2.  From the main menu, choose the option **Test Database Connection**.

        -   If there are any issues, you must solve them before continuing.

        -   Once the connection test passes, return the main menu.

    3.  From the main menu, choose the option **Start/Stop Server** to restart DeployR-related services.

    4.  Once the DeployR server has been successfully restarted, return the main menu.

    5.  From the main menu, choose option to run the [DeployR diagnostic tests](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing). If there are any issues, you must solve them before continuing. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535).

    6.  Exit the utility.

### Configure SELinux

Configure SELinux to permit access to DeployR using one of these options:

-   To benefit from the protection of SELinux, enable SELinux and configure the appropriate SELinux exception policies as described here: <http://wiki.centos.org/HowTos/SELinux>

-   Disable SELinux so that access to DeployR is allowed as follows:

    1.  In `/etc/selinux/config`, set `SELINUX=disabled`.
    2.  Save the changes.
    3.  Reboot the machine.

### Set Password on Testuser Account

In addition to the `admin` account, DeployR is delivered with the `testuser` account you can use to interact with our examples. This account is disabled by default.

1.  Log in as `admin` to the DeployR landing page. If you haven't set a password for `admin` user yet, [do so now](#installserver).

    After installing DeployR for Microsoft R Server 2016 and setting the password for the `admin` user account, you can log into the DeployR landing page. The landing page is accessible at `http://<DEPLOYR_SERVER_IP>:8050/deployr/landing`, where `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine.

2.  Go to the **Administration Console**.

3.  Go to the **Users** tab and [use these instructions](../deployr-admin-console/deployr-admin-console-user-accounts.md) to:

    -   Enable the `testuser` account.

    -   Set a new password for this account.

<a name="migrate"></a>
## Migrate to DeployR for Microsoft R Server 2016

Please carefully follow these migration instructions to migrate users, R Scripts, projects, other DeployR data as well as to learn how to update/preserve client application files.

>If you want to upgrade or reinstall your version or R or Microsoft R Server 2016, please [follow these instructions](deployr-admin-configure-reinstall-r.md).

### From Previous DeployR Version to DeployR for Microsoft R Server 2016:

The following instructions will walk you through a migration of DeployR 8.0.0 or earlier to DeployR for Microsoft R Server 2016.

1.  **Do not uninstall** your older version of DeployR until you have backed up the data you want to keep and completed the data migration.

2.  Ensure your previous version of DeployR is running and that all users are logged out of the version.

3.  [Install and configure](#installserver) DeployR for Microsoft R Server 2016 and all its dependencies.

4.  Ensure that all users are logged out of DeployR for Microsoft R Server 2016 as well.

5.  If the MongoDB database is on a different machine than the one on which you installed DeployR for Microsoft R Server 2016, then you must copy both the MongoDB migration tool, `exportMongoDB.sh`, and `MongoMigration.jar`  <br>*from*:  `$DEPLOYR_HOME_VERSION_8.0.5/deployr/tools/mongoMigration`  <br>*to*: `$DEPLOYR_HOME_OLD_VERSION/deployr/tools/mongoMigration` on the machine running MongoDB.  

    If the MongoDB database is on the same machine as where you've installed DeployR for Microsoft R Server 2016, skip this step.

6.  On the **machine running MongoDB**, run `exportMongoDB.sh`, the DeployR MongoDB migration tool:

        cd $DEPLOYR_HOME/deployr/tools/mongoMigration    
        ./exportMongoDB.sh -m "<DEPLOYR_HOME_OLD_VERSION>/mongo/mongo/bin/mongoexport" -p <MongoDB_Database_Password> -o db_backup.zip
    Where `<MongoDB_Database_Password>` is the password defined in the `grails/mongo/password` parameter in the `deployr.groovy` file. 
    
7.  Download `db_backup.zip`.

8.  Restore that data into the DeployR for Microsoft R Server 2016 Administration Console.

    1.  Log into the DeployR for Microsoft R Server 2016 landing page.

    2.  From the landing page, open the DeployR for Microsoft R Server 2016 Administration Console.

    3.  In the **Database** tab, click **Restore DeployR Database**.

    4.  In the **Database restore** window, select `db_backup.zip`, the backup file you created a few steps back.

    5.  Click **Restore** to restore the database.

    6.  Click **The Grid** tab in the console menu.

    7.  Delete any of the old node configurations since those are version-specific and cannot be reused.

        >[!WARNING]
        >Grid node configurations will not work after migration due to their dependence on a specific version of DeployR. After migrating, you will notice that the old grid configuration has been carried over to the newly installed DeployR version. However, since those grid nodes are not compatible with the DeployR server, they appear highlighted in the Administration Console when you first start the server. This highlighting indicates that a node is unresponsive. We recommend deleting these old grid nodes in the Administration Console the first time you log into the console.

9.  Preserve and update any JavaScript client application files. Before you deploy any JavaScript client application files to DeployR for Microsoft R Server 2016, update the client files so that they use the current version of `jsDeployR` [client library](../deployr-tools-and-samples.md). After installation, update your application files to use the latest [JavaScript API calls](https://microsoft.github.io/js-client-library).

<br>
### From DeployR for Microsoft R Server 2016 to Another Instance of This Version

1.  Log into the landing page for the DeployR instance containing the data you wish to migrate.

    After installing DeployR for Microsoft R Server 2016 and setting the password for the `admin` user account, you can log into the DeployR landing page. The landing page is accessible at `http://<DEPLOYR_SERVER_IP>:8050/deployr/landing`, where `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine.

1.  From the landing page, open the Administration Console.

2.  In the **Database** tab, click **Backup DeployR Database**. A zip file is created and downloaded to your local machine.

3.  Then, log into the landing page for the DeployR instance to which you want to migrate the data.

4.  From the landing page, open the Administration Console.

5.  In the **Database** tab, click **Restore DeployR Database**.

6.  In the **Database restore** window, select the backup file you created and downloaded a few steps back.

7.  Click **Restore** to restore the database.

<br>

## Uninstalling DeployR

Follow these instructions uninstall on Linux.

Remember to uninstall DeployR on both the main server and any other grid node machines.

>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

**To uninstall the DeployR main server:**

1.  Launch the DeployR administrator utility script as `root` or a user with `sudo` permissions:

        cd /home/deployr-user/deployr/8.0.5/deployr/tools/ 
        sudo ./adminUtilities.sh

2.  From the main menu of the utility, choose the option to stop the server.

3.  Stop the server and exit the utility.

4.  If you are using a PostgreSQL database for DeployR, then stop the process as described in the documentation for that database.

5.  Remove DeployR, Tomcat and RServe directories. At the prompt, type:

        sudo rm -rf /home/deployr-user/deployr/8.0.5

6.  Remove extraneous files. At the prompt, type:

        rm -rf /home/deployr-user/deployrdownload

7.  If you are using a PostgreSQL database for DeployR, then remove the associated directories.

<br>

**To uninstall remote grid nodes:**

Repeat these steps on each grid node machine.

1.  Stop the DeployR Rserve component service. At the prompt, type:

        /home/deployr-user/deployr/8.0.5/rserve/rserve.sh stop

2.  Remove DeployR and RServe directories. At the prompt, type:

        sudo rm -rf /home/deployr-user/deployr/8.0.5

3.  Remove the extraneous files and directory. At the prompt, type:

        rm -rf /home/deployr-user/deployrdownload