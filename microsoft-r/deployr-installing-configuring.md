---

# required metadata
title: "Installing and Configuring DeployR"
description: "How to install, migrate, and configure DeployR"
keywords: "install, installation, DeployR, configuration, configure"
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
# Installing & Configuring DeployR

>**Get More DeployR Power:**  
>Get DeployR Enterprise today to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

## System Requirements for DeployR

Before you begin installing DeployR, make sure the computer meets the minimum hardware and software requirements.

**Operating System (all 64-bit processor)**. 

|DeployR Enterprise|DeployR Open|
|------------------------------------------------------|--------------|
|Supported Platforms.<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-&nbsp;SUSE&nbsp;Linux&nbsp;Enterprise&nbsp;Server&nbsp;11&nbsp;(SP2)<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-&nbsp;RHEL/CentOS 5.8, 6.x<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-&nbsp;Windows Server 2008 (R2 SP1), 2012|Supported Platforms.<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- SUSE Linux Enterprise Server 11 (SP2)<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- RHEL/CentOS 5.8, 6.x, 7.0<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Windows Server 2008 (R2 SP1), 2012<br/>Experimental platforms. While DeployR Open can run on these OS, they are neither fully tested nor officially supported. Not recommended for production environments.<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- OpenSUSE 13.1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Ubuntu 12.04, 14.04<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Mac OS X Mavericks (10.9), Yosemite (10.10)<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Windows 7.0 (SP1), 8.1, 10

**Hardware**. Intel Pentium®-class processor; 3.0 GHz recommended

**Free disk space**. 250 GB or more recommended

**RAM**. 4 GB or more is recommended

**Java JVM heap size**. 1 GB or more in production

**Swap space**. 8 GB or more for larger data sets is recommended

**Internet access**. To download DeployR and software dependencies as well as to interact with the Repository Manager, Administration Console, API Explorer Tool, and this comprehensive website. If using Microsoft Internet Explorer, use version 8 or later.

>[!IMPORTANT]
>We highly recommend installing DeployR on a *dedicated server machine*.

## Software Dependencies

DeployR depends on the installation and configuration of a number of software dependencies. The list and steps depend on the platform onto which you are installing.

See the DeployR dependencies for your OS:

-   [Windows](#installing-on-Windows)
-   [Linux](#installing-on-linux)
-   [Mac / OS X](#installing-on-mac-os-x)

## Installing on Windows

>[!IMPORTANT]  
>
>1.  Install the DeployR main server first **before** installing any grid nodes.
>2.  We highly recommend installing DeployR on a dedicated server machine.
>3.  Review the [system requirements & supported operating systems list](#system-requirements-for-deployr) before you continue.
>4.  If you already have a version of DeployR, carefully follow the [migration instructions](#upgrading-deployr) before continuing.
>5.  You may need to temporarily disable your anti-virus software to install DeployR. Turn it back on as soon as you are finished.

### Dependencies for Windows

Before you can install DeployR on the main server machine or any additional grid node (DeployR Enterprise only), you must manually install the following dependencies. All other dependencies will be installed for you.

#### DeployR Enterprise Dependencies

DeployR Enterprise depends on the manual installation and configuration of these dependencies.

| Dependency Name                                                                                                                                                                                                    | DeployR Server | DeployR Grid Node |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|-------------------|
| Java™ Runtime Environment 8 or 7u13 (or later)                                                                                                                                                                     | Yes            | No                |
| Revolution R Enterprise for Windows 8.0 and its dependencies                                                                                                                                                       | Yes            | Yes               |
| DeployR Rserve 7.4.2                                                                                                                                                                                               | Yes            | Yes               |
| MongoDB 2.6.7<br />MongoDB is licensed by MongoDB, Inc. under the Affero GPL 3.0 license; commercial licenses are also available. Additional information on MongoDB licensing is available [here](https://www.mongodb.org/licensing).  | Yes            | No                |

**To install the required dependencies for DeployR Enterprise:**

1.  On the DeployR server, [download](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and install **Java™ Runtime Environment 8 or 7u13 (or later)**.

	>Java is only required on the DeployR server, not on any [grid node machines](#grid-node-install-for-windows).

2.  Install **[Revolution R Enterprise for Windows](http://go.microsoft.com/fwlink/?LinkID=698527)**, which includes ScaleR for multi-processor and big data support. **Follow the instructions provided with RRE to install it as well as any of its dependencies**. [Contact technical support](https://support.microsoft.com/) if you cannot find the proper version of Revolution R Enterprise for Windows.

3.  Install **DeployR Rserve 7.4.2** in one of the following ways:

    -   Install using the R GUI. This assumes you have write permissions to the global R library. If not, use the next option.    
		1.  Download DeployR Rserve 7.4.2 for Windows, [deployrRserve\_7.4.2.zip](https://github.com/Microsoft/deployr-rserve/releases/).
		2.  Launch `Rgui.exe`, located by default under `C:\Program Files\Microsoft\MRO-for-RRE\8.0\R-3.2.2\bin\x64`.
		3.  From the menus, choose **Packages &gt; Install Package(s) from local zip files**.
		4.  Select `deployrRserve_7.4.2.zip` from the folder into which it was downloaded.
		5.  Click **Open** to install it.
    -   Alternatively, install using the command prompt:
    	1.  Download DeployR Rserve 7.4.2 for Windows, [deployrRserve\_7.4.2.zip](https://github.com/Microsoft/deployr-rserve/releases/).
		2.  Open a Command Prompt window **as Administrator**.
		3.  Run the following command to install DeployR Rserve 7.4.2:

				<PATH_TO_R>\bin\x64\R.exe CMD INSTALL -l <PATH_TO_R>\library <PATH_TO_RSERVE>\deployrRserve_7.4.2.zip

        `<PATH_TO_R>` is the path to the directory that contains the R executable, by default `C:\Program Files\Microsoft\MRO-for-RRE\8.0\R-3.2.2`.
        And, where `<PATH_TO_RSERVE>` is the path into which you downloaded Rserve.

        Example: A user with administrator privileges might run these commands for example.

            cd "C:\Program Files\Microsoft\MRO-for-RRE\8.0\R-3.2.2\bin\x64"
            R CMD INSTALL -l "C:\Program Files\Microsoft\MRO-for-RRE\8.0\R-3.2.2\library" "%homepath%\Downloads\"

4.  Download and unzip the **MongoDB 2.6.7** contents into `%TEMP%\MONGODB_DEPLOYR` directory as described in the following steps. Applies to DeployR Enterprise only.

	>MongoDB is licensed by MongoDB, Inc. under the Affero GPL 3.0 license; commercial licenses are also available. Additional information on MongoDB licensing is available [here](https://www.mongodb.org/licensing).

	1.  Download [MongoDB 2.6.7](http://downloads.mongodb.org/win32/mongodb-win32-x86_64-2008plus-2.6.7.zip).

    2.  In Windows Explorer, type `%TEMP%` into the file path field and press **Enter**.

		>This path varies from one Windows version to another.  
		>For example, on Windows Server 2012, it might appear as: `C:\Users\<username>\AppData\Local\Temp\2`.

		![File Path](./media/deployr-installing-configuring/mongodb-1.png)

    3.  Create a directory named `MONGODB_DEPLOYR` under the path you just revealed.

    4.  Go to the new `MONGODB_DEPLOYR` directory.

    5.  To capture the path to this directory to use when unzipping later, copy the file path to your clipboard.
        To see the full path, click inside the file path field.

         ![File Path](./media/deployr-installing-configuring/mongodb-3.png)

    6.  Extract the contents of the MongoDB 2.6.7 zip file into the new `MONGODB_DEPLOYR` directory.

**-- The prerequisites for DeployR Enterprise are installed. You can now install the main server for DeployR Enterprise. --**

#### DeployR Open Dependencies

DeployR Open depends on the manual installation and configuration of these dependencies.

| Dependency Name                                                                                                      | DeployR Server | DeployR Grid Node |
|----------------------------------------------------------------------------------------------------------------------|----------------|-------------------|
| Java™ Runtime Environment 8 or 7u13 (or later)                                                                       | Yes            | No                |
| [Revolution R Open](http://mran.microsoft.com) v8.0.x, 3.2.0 - 3.2.2 or R v3.1.x, 3.2.0 - 3.2.2. | Yes            | Yes               |
| DeployR Rserve 7.4.2                                                                                                 | Yes            | Yes               |

**To install the required dependencies for DeployR Open**

1.  On the DeployR server, [download](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and install **Java™ Runtime Environment 8 or 7u13 (or later)**.

	>Java is only required on the DeployR server, not on any [grid node machines](#grid-node-install-for-windows).

2.  Install either [Revolution R Open](http://mran.microsoft.com) v8.0.x, 3.2.0 - 3.2.2 or R v3.1.x, 3.2.0 - 3.2.2. [Revolution R Open](http://mran.microsoft.com) is the enhanced distribution of R from Microsoft.

3.  Install **DeployR Rserve 7.4.2** in one of the following ways:

    -   Install using the R GUI. This assumes you have write permissions to the global R library. If not, use the next option.
    	1.  Download DeployR Rserve 7.4.2 for Windows, [deployrRserve\_7.4.2.zip](https://github.com/Microsoft/deployr-rserve/releases/).
    	2.  Launch `Rgui.exe` for the version of R you just installed.
    	3.  From the menus, choose **Packages &gt; Install Package(s) from local zip files**.
    	4.  Select `deployrRserve_7.4.2.zip` from the folder into which it was downloaded.
    	5.  Click **Open** to install it.

    -   Alternatively, install using the command prompt:
    	1.  Download DeployR Rserve 7.4.2 for Windows, [deployrRserve\_7.4.2.zip](https://github.com/Microsoft/deployr-rserve/releases/).
    	2.  Open a Command Prompt window **as Administrator**.
    	3.  Run the following command to install DeployR Rserve 7.4.2:

				<PATH_TO_R>\bin\x64\R.exe CMD INSTALL -l <PATH_TO_R>\library <PATH_TO_RSERVE>\deployrRserve_7.4.2.zip
	
	        `<PATH_TO_R>` is the path to the directory containing the R executable you just installed.  
			And, where `<PATH_TO_RSERVE>` is the path into which you downloaded Rserve.

		Example: A user with administrator privileges might run these commands for example.

            cd "C:\Program Files\RRO\R-3.2.2\bin\x64"
            R CMD INSTALL -l "C:\Program Files\RRO\R-3.2.2\library" "%homepath%\Downloads\"

**-- The prerequisites for DeployR Open are now installed. You can begin installing the main server for DeployR Open now. --**
 
### Basic DeployR Install for Windows

The basic installation of DeployR will install the DeployR main server. DeployR Enterprise customers can also [install additional grid nodes](#grid-node-install-for-windows) for optimized workload distribution.

>**DeployR Enterprise Only:**  
>Get DeployR Enterprise today to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

After installing [these prerequisites](#dependencies-for-windows), install DeployR as follows:

1.  To install properly on Windows, you must have administrator privileges. Log in as a user with administrator rights. Also, you may need to temporarily turn off or disable your anti-virus software to complete the installation process. Please turn it back on as soon as you are finished.

2.  Download and launch the DeployR installer files.

    -   For **DeployR Enterprise**, download `DeployR-Enterprise-8.0.0.exe`, which can be found in the Microsoft R Server package, and launch this installer. [Contact technical support](https://support.microsoft.com/) if you cannot find this file.

    -   For **DeployR Open**, download `DeployR-Open-8.0.0.exe` and launch the installer.

3.  If a message appears onscreen during installation asking whether you want to *“allow the following program to make changes to this computer”*, click **Continue**.

4.  When prompted, accept the installation of any missing [dependencies](#software-dependencies) required for this version of DeployR.

5.  When prompted, choose to accept the default installation directory or enter a unique directory path for this version of DeployR.

6.  Follow the onscreen prompts to complete the installation. When the setup completes, a script is launched to finalize the installation. This step can take a few minutes. This script will generate backup copies of the files that will be changed. Each backup file uses the original name but with the suffix `YYYY-MM-DD\_HH-MM` such as `deployr.groovy.2016-02-16_08-05`. Additionally, the script will generate a log file called `%TEMP%\DeployR-configuration.log` containing the steps taken during this configuration process.

7.  When the script is complete, press the **Enter** key to exit the window.

8.  If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md) or [AWS EC2 instance](deployr-admin-configure-for-aws.md), you must:

    -   Set the DeployR [server Web context](deployr-admin-diagnostics-troubleshooting.md#set-context) to the external **Public IP** or else you will not be able to access to the DeployR landing page or other DeployR components after installation and be sure the automatic detection of IP address:  (Setup: [Azure](deployr-admin-configure-for-azure.md#setting-the-server-web-context) | [AWS](deployr-admin-configure-for-aws.md) ).

    -   Open [DeployR ports](#updating-your-firewall) `8000`, `8001`, and `8006`:  (Setup: [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints) | [AWS](deployr-admin-configure-for-aws.md) ).

    -   Open the same DeployR ports [in Windows Firewall](deployr-admin-configure-for-azure.md#updating-the-firewall).

**-- The basic installation of DeployR is now complete --**

>**What's Next After Installing?**
>
>1.  Verify your install by running a [diagnostic check of the server](deployr-admin-diagnostics-troubleshooting.md#running-the-diagnostic-check) from the DeployR landing page. Log in as `admin` with the password `changeme` at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing`. You'll be [prompted for a new password](#changing-default-passwords) the first time. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).
>2.  [Add any additional grid nodes](#grid-node-install-for-windows) (DeployR Enterprise only).
>3.  [Finish configuring DeployR](#configuring-deployr). Follow the topics in that section in the order in which they are presented.
>4.  [Download DeployR client-side developer tools](deployr-tools-and-samples.md), including the RBroker framework and client libraries.
>5.  [Create new user accounts](deployr-admin-console/deployr-admin-console-user-accounts.md) in the Administration Console. Then, provide each user with their username and password as well as the address of the DeployR landing page.
>6.  Review the [Administrator Getting Started](deployr-administrator-getting-started.md) to get up and running quickly. Also available are the [Data Scientist Getting Started](deployr-data-scientist-getting-started.md) guide and the [Application Developer Getting Started](deployr-application-developer-getting-started.md) guide.

### Grid Node Install for Windows

When you install the DeployR server, one local grid node is installed automatically for you. DeployR Open supports only this single node installed on `localhost` with a [fixed slot limit](deployr-admin-console/deployr-admin-managing-the-grid.md#node-properties). DeployR Enterprise, on the otherhand, allows you to point this default grid node to a remote location, customize its slot limit, and even add additional grid nodes to scale for increasing load. This option also assumes that you have already installed the DeployR server.

>Get DeployR Enterprise today to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

>[!TIP]
>-   For help in determining the right number of grid nodes for you, refer to the [Scale & Throughput](deployr-admin-scale-and-throughput.md#tuning-grid-capacity) document.
>-   Once you have installed and configured one grid node, you can copy the files over from the server that has already been set up over to the next one.
>-   Install each grid node on a separate host machine.

After installing the [main server for DeployR Enterprise](#basic-deployr-install-for-windows), install each grid node on a separate machine as follows:

1.  Log into the machine on which you will install the grid node with administrator rights.

2.  Install Microsoft R Server and RServe [as described here](#dependencies-for-windows) on the grid node machine.

3.  Download the node installer file, `DeployR-Enterprise-node-8.0.0.exe`, which can be found in the Microsoft R Server package. [Contact technical support](https://support.microsoft.com/) if you cannot find this file.

4.  Launch the installer, `DeployR-Enterprise-node-8.0.0.exe`.

5.  If a message appears onscreen during installation asking whether you want to *“allow the following program to make changes to this computer”*, click **Continue**.

6.  When the script is complete, press the **Enter** key to exit the window.

7.  Repeat these steps on each machine that will host a remote grid node.

8.  When the installer completes, log into the DeployR landing page as `admin` at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing` where `<DEPLOYR_SERVER_IP>` is the IP of the main DeployR server.

	1.  Go to the Adminstrative Console and [configure each grid node](#configuring-the-deployr-grid).
	2.  Return to the landing page and run a [diagnostic check](deployr-admin-diagnostics-troubleshooting.md#running-the-diagnostic-check). Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for help.

**-- The grid node installation is now complete --**

## Installing on Linux

>[!IMPORTANT]
>
>1.  Always install the DeployR main server first before installing any grid nodes.
>2.  We highly recommend installing DeployR on a dedicated server machine.
>3.  Review the [system requirements & supported operating systems list](#system-requirements-for-deployr) before you continue.
>4.  If you already have a version of DeployR, carefully follow the [migration instructions](#upgrading-deployr) before continuing.
>5.  The Ubuntu and OpenSUSE releases of DeployR Open are experimental and not officially supported. They are **not** available for DeployR Enterprise.

### Dependencies for Linux

Before you can install DeployR on the main server machine, a remote database (DeployR Enterprise only), or any additional grid node (DeployR Enterprise only), you must manually install the following dependencies. All other dependencies will be installed for you.

DeployR depends on the manual installation and configuration of these dependencies.

| Dependency                                                                                                                                                                                                         | DeployR Server                                                                          | DeployR Grid Node                                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| Java™ Runtime Environment 8 or 7u13 (or later)                                                                                                                                                                     | Yes                                                                                     | No                                                                                      |
| R Engine:<br />- DeployR Enterprise:                                                                                                                                                                                              <br />[Microsoft R Server](http://go.microsoft.com/fwlink/?LinkID=698527) 8.0 and its dependencies<br />- DeployR Open:<br />    [Revolution R Open](http://mran.microsoft.com) v8.0.x, 3.2.0 - 3.2.2 or R v3.1.x, 3.2.0 - 3.2.2                                                                                            | Yes                                                                                     | Yes                                                                                     |
| DeployR Rserve 7.4.2                                                                                                                                                                                               | Yes                                                                                     | Yes                                                                                     |
| MongoDB 2.6.7 for DeployR Enterprise<Br />MongoDB is licensed by MongoDB, Inc. under the Affero GPL 3.0 license; commercial licenses are also available. Additional information on MongoDB licensing is available [here](https://www.mongodb.org/licensing).  | Yes                                                                                     | No                                                                                      |
| make, gcc, gfortran                                                                                                                                                                                                | Yes                                                                                     | Yes                                                                                     |
| nfs-utils and nfs-utils-lib<br />Note: On Ubuntu, install nfs-common to get nfs-utils.                                                                                                                                                               | Yes, for [external directories](deployr-admin-manage-big-data.md)  | Yes, for [external directories](deployr-admin-manage-big-data.md)  |

**To install the required dependencies:**

1.  On the main DeployR server, install **Java™ Runtime Environment 8 or 7u13 (or later)**, a dependency of DeployR, as follows. For more help installing Java or setting the path, refer to oracle.com.

	>Java is only required on the main DeployR server.

    -   [Download the tar file from Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and install it **OR** install using your local package manager.

    -   Set `JAVA_HOME` to the installation path for the JDK or JRE you just installed.
        For example, if the installation path was `/usr/lib/jvm/jdk1.8.0_45/jre`, then the command would be:

            export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_45/jre

2.  Install R, another dependency, for your edition of DeployR.

    -   **DeployR Enterprise 8.0.0** requires [Microsoft R Server 8.0.0](http://go.microsoft.com/fwlink/?LinkID=698527), which includes ScaleR for multi-processor and big data support. **Follow the instructions provided with Microsoft R Server to install it as well as any of its dependencies.** [Contact technical support](https://support.microsoft.com/) if you cannot find the proper version of Microsoft R Server.

    -   **DeployR Open 8.0.0** requires either [Revolution R Open](http://mran.microsoft.com) v8.0.x, 3.2.0 - 3.2.2 or R v3.1.x, 3.2.0 - 3.2.2. [Revolution R Open](http://mran.microsoft.com) is the enhanced distribution of R from Microsoft.

3.  Install **DeployR Rserve 7.4.2**, a dependency of DeployR, after Microsoft R Server, Revolution R Open, or R is installed as follows:

    1.  [Download the Linux tar file for DeployR Rserve 7.4.2, `deployrRserve_7.4.2.tar.gz`](https://github.com/Microsoft/deployr-rserve/releases/).

    2.  Run the following command to install RServe:

        R CMD INSTALL deployrRserve_7.4.2.tar.gz

4.  **For DeployR Enterprise only:** Prepare for **MongoDB 2.6.7**.

    1.  Download [MongoDB 2.6.7](http://downloads.mongodb.org/linux/mongodb-linux-x86_64-2.6.7.tgz).

    2.  Untar the MongoDB download into home directory of the user who will be installing DeployR Enterprise. For example: `/home/deployr-user/`. The DeployR Enterprise installer will find it later.

	>MongoDB is licensed by MongoDB, Inc. under the Affero GPL 3.0 license; commercial licenses are also available. Additional information on MongoDB licensing is available [here](https://www.mongodb.org/licensing).

5.  Make sure the system repositories are up-to-date prior to installing DeployR. The following commands *do not install anything*; however running them will ensure that the repositories contain the latest software:

    Run the following command:

	**Redhat / CentOS:**	

        sudo yum clean all

    **Ubuntu:**

        sudo apt-get update

    **OpenSUSE / SLES:**

        sudo zypper clean --all

6.  Install the following packages (`make`, `gcc`, and `gfortran`) if missing:

	>Install packages as `root` or a user with `sudo` permissions.

	**Redhat / CentOS:**
	
    1.  Check if the packages are already installed.

        yum list make gcc gfortran

    2.  Install any missing packages:

	        #if make is missing, run 
	        yum install make
	
	        #if gcc is missing, run
	        yum install gcc
	
	        #if gfortran is missing, run
	        yum install gfortran

    **Ubuntu:**

    1.  Check if the packages are already installed.

			dpkg -l make gcc gfortran

    2.  Install any missing packages:

			#if make is missing, run 
			sudo apt-get install make
			
			#if gcc is missing, run
			sudo apt-get install gcc
			
			#if gfortran is missing, run
			sudo apt-get install gfortran

    **OpenSUSE / SLES:**

    1.  Check if the packages are already installed.

			sudo zypper search -i make
			sudo zypper search -i gcc 
			sudo zypper search -i gfortran

    2.  Install any missing packages:

			#if make is missing, run 
			sudo zypper install make
			
			#if gcc is missing, run
			sudo zypper install gcc
			
			#if gfortran is missing, run
			sudo zypper install gfortran


**-- The prerequisites are now installed. You can begin installing DeployR now. --**

 
### Basic DeployR Install for Linux

The basic installation of DeployR will install the DeployR main server and configure a local MongoDB database on the same machine.

>**DeployR Enterprise Only:** In addition to this basic installation, DeployR Enterprise customers can also use a [remote database for DeployR](#deployr-install-with-remote-database) or install [additional grid nodes](#grid-node-install-for-Linux) for optimized workload distribution.

#### DeployR Enterprise

After installing [these prerequisites](#dependencies-for-linux), install DeployR Enterprise as follows:

1.  Log into the operating system.

2.  Create the `deployrdownload` directory and go to that directory. At the prompt, type:

	>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

        mkdir /home/deployr-user/deployrdownload
        cd /home/deployr-user/deployrdownload

3.  Download the software, `DeployR-Enterprise-Linux-8.0.0.tar.gz`, using the link in your customer welcome letter.

4.  Unzip the tar file contents, go the `installFiles` directory, and launch the installation script. At the prompt, type:

        tar -xzf DeployR-Enterprise-Linux-8.0.0.tar.gz
        cd installFiles
        ./installDeployREnterprise.sh

5.  When the installer starts, accept the terms of the agreement to continue.

6.  Choose `Option 1`. This will install the DeployR server along with a local database.

7.  Follow the onscreen installer prompts. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

8.  If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md) or [AWS EC2 instance](deployr-admin-configure-for-aws.md), you must:

    -   Set the DeployR [server Web context](deployr-admin-diagnostics-troubleshooting.md#landing-page-cannot-be-reached) to the external **Public IP** or else you will not be able to access to the DeployR landing page or other DeployR components after installation and be sure the automatic detection of IP address:  (Setup: [Azure](deployr-admin-configure-for-azure.md#setting-the-server-web-context) | [AWS](deployr-admin-configure-for-aws.md) ).

    -   Open [DeployR ports](#updating-your-firewall) `8000`, `8001`, and `8006`:  (Setup: [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints) | [AWS](deployr-admin-configure-for-aws.md) ).

**-- The basic installation of DeployR is now complete --**

#### DeployR Open

After installing [these prerequisites](#dependencies-for-linux), install DeployR Open as follows:

1.  Log into the operating system.

2.  Create the `deployrdownload` directory and go to that directory. At the prompt, type:

	>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

        mkdir /home/deployr-user/deployrdownload
        cd /home/deployr-user/deployrdownload

3.  Download the install bundle for DeployR Open, `DeployR-Open-Linux-8.0.0.tar.gz`.

4.  Unzip the tar file contents, go the `installFiles` directory, and launch the installation script. At the prompt, type:

        tar -xzf DeployR-Open-Linux-8.0.0.tar.gz
        cd installFiles
        ./installDeployROpen.sh

5.  When the installer starts, accept the terms of the agreement to continue.

6.  When prompted by the installer, choose installation `Option 1`. This will install the DeployR server along with a local database.

7.  Follow the remaining onscreen installer prompts. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

8.  If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md) or [AWS EC2 instance](deployr-admin-configure-for-aws.md), you must:

    -   Set the DeployR [server Web context](deployr-admin-diagnostics-troubleshooting.md#landing-page-cannot-be-reached) to the external **Public IP** or else you will not be able to access to the DeployR landing page or other DeployR components after installation and be sure the automatic detection of IP address:  (Setup: [Azure](deployr-admin-configure-for-azure.md#setting-the-server-web-context) | [AWS](deployr-admin-configure-for-aws.md) ).

    -   Open [DeployR ports](#updating-your-firewall) `8000`, `8001`, and `8006`:  (Setup: [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints) | [AWS](deployr-admin-configure-for-aws.md) ).

**-- The basic installation of DeployR is now complete --**

>**What's Next After Installing?**
>
>1.  Verify your install by running a [diagnostic check of the server](deployr-admin-diagnostics-troubleshooting.md#running-the-diagnostic-check) from the DeployR landing page. Log in as `admin` with the password `changeme` at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing`. You'll be [prompted for a new password](#changing-default-passwords) the first time. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).
>2.  [Add any additional grid nodes](#install-deployr-nodes) (DeployR Enterprise only).
>3.  [Finish configuring DeployR](#configuring-deployr). Follow the topics in that section in the order in which they are presented.
>4.  [Download DeployR client-side developer tools](deployr-tools-and-samples.md), including the RBroker framework and client libraries.
>5.  [Create new user accounts](deployr-admin-console/deployr-admin-console-user-accounts.md) in the Administration Console. Then, provide each user with their username and password as well as the address of the DeployR landing page.
>6.  Review the [Administrator Getting Started](deployr-administrator-getting-started.md) to get up and running quickly. Also available are the [Data Scientist Getting Started](deployr-data-scientist-getting-started.md) guide and the [Application Developer Getting Started](deployr-application-developer-getting-started.md) guide.

### Grid Node Install for Linux

When you install the DeployR server, one local grid node is installed automatically for you. DeployR Open supports only this single node installed on `localhost` with a [fixed slot limit](deployr-admin-console/deployr-admin-managing-the-grid.md#node-properties). DeployR Enterprise, on the otherhand, allows you to point this default grid node to a remote location, customize its slot limit, and even add additional grid nodes to scale for increasing load. This option also assumes that you have already installed DeployR using [`Option 1`](#install-deployr-linux) or [`Option 3`](#install-deployr-custom). 

>[!TIP]
>-   For help in determining the right number of grid nodes for you, refer to the [Scale & Throughput](deployr-admin-scale-and-throughput.md#tuning-grid-capacity) document.
>-   Once you have installed and configured one grid node, you can copy the files over from the server that has already been set up over to the next one.
>-   Install each grid node on a separate host machine.

After installing the main [DeployR server](#basic-deployr-install-for-linux), install each grid node on a separate machine as follows:

1.  Log into the operating system on the machine on which you will install the grid node.

2.  Install Microsoft R Server and RServe [as described here](#dependencies-for-linux) on the grid node machine.

3.  Create the `deployrdownload` directory and go to that directory. At the prompt, type:

	>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

        mkdir /home/deployr-user/deployrdownload
        cd /home/deployr-user/deployrdownload

4.  Download the software, `DeployR-Enterprise-Linux-8.0.0.tar.gz`, using the link in your customer welcome letter.

5.  Unzip the tar file contents, go the `installFiles` directory, and launch the installation script. At the prompt, type:

        tar -xzf DeployR-Enterprise-Linux-8.0.0.tar.gz
        cd installFiles
        ./installDeployREnterprise.sh

6.  When the installer starts, accept the terms of the agreement to continue.

7.  When prompted by the installer, choose installation `Option 4` and follow the onscreen installer prompts. This will install a remote grid node.

8.  Enter the directory path in which to install. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

9.  When the installer completes, log into the DeployR landing page as `admin` at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing` where `<DEPLOYR_SERVER_IP>` is the IP of the main DeployR server.

    1.  Go to the Adminstrative Console and [configure each grid node](#configuring-the-deployr-grid).
    2.  Return to the landing page and run a [diagnostic check](deployr-admin-diagnostics-troubleshooting.md#running-the-diagnostic-check). Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for help or post questions to our [DeployR Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).

**-- The grid node installation is now complete --**

### DeployR Install with Remote Database

DeployR Enterprise on Linux supports the installation of a remote database for DeployR. This requires that you first install that database on one machine, and then install [these prerequisites](#dependencies-for-linux) followed by DeployR on a second machine.

#### Part A: Installing the Remote Database for DeployR (Option 2)

1.  Log into the operating system on the machine on which you will install the database.

2.  Create the `deployrdownload` directory and go to that directory. At the prompt, type:

	>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

        mkdir /home/deployr-user/deployrdownload
        cd /home/deployr-user/deployrdownload

3.  Download the software, `DeployR-Enterprise-Linux-8.0.0.tar.gz`, using the link in your customer welcome letter.

4.  Unzip the tar file contents, go the `installFiles` directory, and launch the installation script. At the prompt, type:

        tar -xzf DeployR-Enterprise-Linux-8.0.0.tar.gz
        cd installFiles
        ./installDeployREnterprise.sh

5.  When the installer starts, accept the terms of the agreement to continue.

6.  When prompted by the installer, choose installation `Option 2` and follow the onscreen installer prompts. This will install the database that will be used remotely by DeployR server. This option assumes that you will install `Option 3` on another machine after installing `Option 2`.

7.  Enter the directory path in which to install. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

8.  If using [IPTABLES firewall](#updating-your-firewall), add the MongoDB port to the firewall settings to allow communications from the DeployR main server. For this release, add `8003`.

9.  Once the database has been installed, go to the machine on which you will install DeployR and follow the steps in the next section.

### Part B: Installing DeployR for Use with a Remote Database (Option 3)

Install DeployR as follows:

1.  Install [these prerequisites](#dependancies-for-linux).

2.  Log into the operating system on the machine on which you want to install DeployR.

3.  Create the `deployrdownload` directory and go to that directory. At the prompt, type:

	>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

        mkdir /home/deployr-user/deployrdownload
        cd /home/deployr-user/deployrdownload

4.  Download the software, `DeployR-Enterprise-Linux-8.0.0.tar.gz`, using the link in your customer welcome letter.

5.  Unzip the tar file contents, go the `installFiles` directory, and launch the installation script. At the prompt, type:

        tar -xzf DeployR-Enterprise-Linux-8.0.0.tar.gz
        cd installFiles
        ./installDeployREnterprise.sh

6.  When the installer starts, accept the terms of the agreement to continue.

7.  When prompted by the installer, choose installation `Option 3`. This will install the DeployR server and configure it for the remote database you installed above.

8.  Enter the directory path in which to install. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

9.  Enter and confirm IP address or hostname for the remote MongoDB database server and follow the prompts.

10. Enter the port number for Tomcat or accept the default.

11. If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md) or [AWS EC2 instance](deployr-admin-configure-for-aws.md), you must:

    -   Set the DeployR [server Web context](deployr-admin-diagnostics-troubleshooting.md#landing-page-cannot-be-reached) to the external **Public IP** or else you will not be able to access to the DeployR landing page or other DeployR components after installation and be sure the automatic detection of IP address:  (Setup: [Azure](deployr-admin-configure-for-azure.md#setting-the-server-web-context) | [AWS](deployr-admin-configure-for-aws.md) ).

    -   Open [DeployR ports](#updating-your-firewall) `8000`, `8001`, and `8006`:  (Setup: [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints) | [AWS](deployr-admin-configure-for-aws.md) ).

**-- The installation of the remote database and DeployR is now complete --**

>**What's Next After Installing?**
>
>1. Verify your install by running a [diagnostic check of the server](deployr-admin-diagnostics-troubleshooting.md#running-the-diagnostic-check) from the DeployR landing page. Log in as `admin` with the password `changeme` at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing`. You'll be [prompted for a new password](#changing-default-passwords) the first time. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help.
>2.  [Add any additional grid nodes](#install-deployr-nodes) (DeployR Enterprise only).
>3.  [Finish configuring DeployR](#configuring-deployr). Follow the topics in that section in the order in which they are presented.
>4.  [Download DeployR client-side developer tools](deployr-tools-and-samples.md), including the RBroker framework and client libraries.
>5.  [Create new user accounts](deployr-admin-console/deployr-admin-console-user-accounts.md) in the Administration Console. Then, provide each user with their username and password as well as the address of the DeployR landing page.
>6.  Review the [Administrator Getting Started](deployr-administrator-getting-started.md) to get up and running quickly. Also available are the [Data Scientist Getting Started](deployr-data-scientist-getting-started.md) guide and the [Application Developer Getting Started](deployr-application-developer-getting-started.md) guide.

## Installing on Mac OS X

>[!IMPORTANT]
>
>1.  We highly recommend installing DeployR on a dedicated server machine.
>2.  The Mac OS X release of DeployR is experimental, and therefore not officially supported. It is not available for DeployR Enterprise. See the list of [supported OS](#system-requirements-for-deployr) for more.

### Dependencies

Before you can install DeployR on the main server machine, you must manually install the following dependencies. All other dependencies will be installed for you.

| Dependency                                                              | DeployR Server                                                                          |
|-------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| Java™ Development Kit 8 or 7u13 (or later)                              | Yes                                                                                     |
| Revolution R Open v8.0.x, 3.2.0 - 3.2.2 or else R v3.1.x, 3.2.0 - 3.2.2 | Yes                                                                                     |
| DeployR Rserve 7.4.2                                                    | Yes                                                                                     |
| nfs-utils and nfs-utils-lib                                             | Yes, for [external directories](deployr-admin-manage-big-data.md)  |

**To install the required dependencies:**

1.  On the main DeployR server, install **Java™ Development Kit 8 or 7u13 (or later)**, a dependency of DeployR, as follows. For more help installing Java or setting the path, refer to oracle.com.

    -   [Download the install file from Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and install it.

    -   Set `JAVA_HOME` to the installation path for the JDK you just installed.  
        *For example*, if the installation path was `JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk`, then the command would be:

            export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home

2.  Install [Revolution R Open](http://mran.microsoft.com) v8.0.x, 3.2.0 - 3.2.2 or R v3.1.x, 3.2.0 - 3.2.2 on the DeployR Open host.

3.  If you intend to use CRAN R for DeployR, you must explicitly define your preferred R mirror address as follows:

    1.  Go to `/Library/Frameworks/R.framework/Resources/etc`.
    2.  Create a file called `Rprofile.site`.
    3.  In that new file, add the following line and substitute your preferred R mirror address. For example, here is the address snapshot used for Revolution R Open 3.2.2, such as the `options(repos = c(CRAN = "https://mran.revolutionanalytics.com/snapshot/2015-08-27))`:

            options(repos = c(CRAN = "YOUR FAVORITE MIRROR"))

4.  Install **DeployR Rserve 7.4.2** after Revolution R Open or R as follows:

    1.  [Download](https://github.com/Microsoft/deployr-rserve/releases/) the Mac OS X file of DeployR Rserve 7.4.2. **Choose the file version for the version of R or RRO you installed in the previous step.**

    2.  Change to the directory where the file was downloaded.

    3.  Run the following command to install RServe:

		>Some browsers change the file extension to a **`.tar`** upon download. If this occurs, then update the following command accordingly.

		-   If you have RRO 3.2.x or R 3.2.x, then run this command:
	
	            R CMD INSTALL deployrRserve_7.4.2_for_R_3.2.x.tgz
	
		-   If you have RRO RRO 8.x or R 3.1.x, then run this command:
	
	            R CMD INSTALL deployrRserve_7.4.2_for_R_3.1.x.tgz

### Installing DeployR for Mac OS X

After installing [these prerequisites](#preparing-osx), install DeployR Open as follows:

>Examples on this site are written for user `deployr-user`. For another user, update the commands accordingly.

1.  Log into the operating system.

2.  Create the `/deployrdownload` directory into which the tar file will be downloaded. At the prompt, type:

        mkdir ~/deployrdownload

3.  Download the install bundle, `DeployR-Open-OSX-8.0.0.tar.gz` to the newly created `/deployrdownload` directory. You may need to move the file to that directory.

4.  Unzip the tar file contents, go the `installFiles` directory, and launch the installation script. At the prompt, type:

        cd ~/deployrdownload
        tar -xzf DeployR-Open-OSX-8.0.0.tar.gz
        cd installFiles
        ./installDeployROpen.sh

5.  When the installer starts, accept the terms of the agreement to continue.

6.  When prompted by the installer, choose installation `Option 1`. This will install the DeployR server.

7.  Follow the remaining onscreen installer prompts. If you will be keeping an older version of DeployR on this same machine for the purposes of migrating data, for example, then be sure to install this version in its own directory.

**-- The basic installation of DeployR is now complete --**

>**What's Next After Installing?**
>
>1.  Verify your install by running a [diagnostic check of the server](deployr-admin-diagnostics-troubleshooting.md#running-the-diagnostic-check) from the DeployR landing page. Log in as `admin` with the password `changeme` at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing`. You'll be [prompted for a new password](#changing-default-passwords) the first time. Consult the [Troubleshooting section](deployr-admin-diagnostics-troubleshooting.md) for additional help or post questions to our [DeployR Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).
>2.  [Finish configuring DeployR](#configuring-deployr). Follow the topics in that section in the order in which they are presented.
>3.  [Download DeployR client-side developer tools](deployr-tools-and-samples.md), including the RBroker framework and client libraries.
>4.  [Create new user accounts](deployr-admin-console/deployr-admin-console-user-accounts.md) in the Administration Console. Then, provide each user with their username and password as well as the address of the DeployR landing page.
>5.  Review the [Administrator Getting Started](deployr-administrator-getting-started.md) to get up and running quickly. Also available are the [Data Scientist Getting Started](deployr-data-scientist-getting-started.md) guide and the [Application Developer Getting Started](deployr-application-developer-getting-started.md) guide.

## Upgrading DeployR

Please carefully follow these migration instructions before you install a more recent version of DeployR to migrate users, R Scripts, projects, other DeployR data as well as to learn how to update/preserve client application files.

These migration steps apply to the following versions:

-   [DeployR 7.4.1](#migrating-to-8-0-0)
-   [DeployR 7.4.0](#migrating-to-8-0-0)
-   [DeployR 7.3](#migrating-to-8-0-0)
-   [DeployR 7.2](#migrating-to-8-0-0)
-   [DeployR 7.1](#migrating-to-8-0-0)
-   [RevoDeployR 7.0](#migrating-to-8-0-0)

>[!IMPORTANT]
>If you want to upgrade or reinstall your version or R or Microsoft R Server, please [follow these instructions](deployr-admin-configure-reinstall-r.md).

>**DeployR Enterprise Only:** To extend the DeployR server's grid beyond the **Default Grid Node**, install all of the grid nodes you want to use, and then configure them in **The Grid** tab in the **Administration Console**.
>
>Get DeployR Enterprise today to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

### Migrating to 8.0.0

The following instructions will walk you through a migration of DeployR 7.4.1 or earlier to the current version of DeployR.

**To migrate:**

1.  Be sure to review the following:

    -   **Do not uninstall** your older version of DeployR until you have backed up the data you want to keep and completed the [data migration](#migrating-to-8-0-0).

    -   While it is technically possible to run instances of two different versions of DeployR side-by-side on a single machine, we strongly recommend that you **dedicate one machine for each server instance** that is *in production* so as to avoid resource contention.

    -   **Grid node configurations will not work after migration** due to their dependence on a specific version of DeployR. After migrating, you will notice that the old grid configuration has been carried over to the newly installed DeployR version. However, since those grid nodes are not compatible with the DeployR 8.0.0 server, they appear highlighted in the Administration Console when you first start the DeployR server. This highlighting indicates that a node is unresponsive. We recommend deleting these old grid nodes in the Administration Console the first time you log into the console.

2.  Ensure your previous version of DeployR is running and that all users are logged out of the version.

3.  Install and configure DeployR 8.0.0 and all its dependencies for your operating system ( [Linux](#installing-on-linux) | [Windows](#installing-on-windows) | [Mac OS X](#installing-on-mac-os-x) ).

4.  Ensure that all users are logged out of DeployR 8.0.0 as well.

5.  Run the DeployR 8.0.0 migration script as follows. This script will shut down the DeployR 8.0.0 server, back up data in the MongoDB database of the previous version of DeployR, restore it into the DeployR 8.0.0 MongoDB database, and restart the DeployR 8.0.0 server.

    Type the following at a command prompt:

	**Linux / Mac OS X:**

		cd <8.0.0_Install_Dir>/deployr/tools
        ./databaseUtils.sh

	**Windows:**

		cd <8.0.0_Install_Dir>\deployr\tools
		./databaseUtils.bat

    Where `<8.0.0_Install_Dir>` is the full path to the DeployR 8.0.0 server installation directory.

    When prompted by the migration script:

    -   Choose option `1` to migrate the database.

    -   Enter the version from which you are migrating as `X.Y` (for example, `7.3`).

    -   Enter the database password if you are migrating from 7.3 or later. You can find this password in the `deployr.groovy` file.

    -   Enter the hostname on which MongoDB was installed. The hostname (or ip) is `localhost` unless you installed a remote database.

    -   Enter the MongoDB port number. By default, the MongoDB port number ends in `03` prefixed by the version number, such as `7303`.

6.  Log into the DeployR 8.0.0 Administration Console.

7.  In the **Grid** tab of the console, delete any of the old node configurations since those are version-specific and cannot be reused.

8.  In the **R Scripts** tab of the console, repopulate the DeployR 8.0.0 example scripts that were lost when you migrated your data from a previous version of DeployR to version 8.0.0 in a step 5.

    1.  Click **Import R Scripts**.

    2.  Select the example script named `DeployR8.0_Example_Scripts.csv`. By default, this file is installed under `<8.0.0_Install_Dir>/deployr/`.

    3.  Click **Load**.

    4.  Choose **Import All**.

    5.  Enter the username `testuser` for the field **Assign username as author for the selected scripts**.

    6.  Click **Import**.

9.  Preserve and update any JavaScript client application files. Before you deploy any JavaScript client application files to DeployR 8.0.0, update the client files so that they use the current version of `jsDeployR` [client library](deployr-tools-and-samples.md). After installation, update your application files to use the latest [JavaScript API calls](https://microsoft.github.io/js-client-library).

## Configuring DeployR

After you install DeployR, please review the following topics.

>[!IMPORTANT]
>For the best results, respect the order in which the following information is presented when configuring DeployR.

### Updating Your Firewall

#### For Linux / Mac OS X:

If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the following ports:

>[!IMPORTANT]
>If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints), [AWS](deployr-admin-configure-for-aws.md) or VirtualBox, then you must also configure endpoints for these ports (or use port-forwarding for VirtualBox).

| Machine         | Ports      | Open Ports                                                                                      |
|-----------------------------------------|-------------------|-------------------------------------------------|
| DeployR server machine      Tomcat ports:<br />- `8000` (Tomcat default port)<br />- `8001` (Tomcat HTTPS port)| To the outside|
| DeployR server machine     |- `8006` (DeployR event console port) | To the public IP of DeployR server AND to the public IP of *each* grid node machine<sup>1</sup>|
| Remote grid node machines<sup>1</sup>  | RServe ports:<br />- `8004` (RServe connection port)<br />- `8005` (RServe cancel port)          | To the public IP of the DeployR server                                                          |
| Remote MongoDB host machine<sup>2</sup> | - `8003` (MongoDB port)               | To the public IP of the DeployR server|
<sup>1</sup> Only DeployR Enterprise offers the ability to expand your Grid framework for load distribution by [installing and configuring additional grid nodes](#grid-node-install-for-linux).

<sup>2</sup> Only DeployR Enterprise on Linux offers the ability to [install a remote MongoDB database](deployr-installing-configuring.md#deployr-install-with-remote-database).

#### For Windows:

During installation, the Windows firewall was updated to allow inbound communications to DeployR on the ports listed in the following table.

>[!IMPORTANT]
>If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints), [AWS](deployr-admin-configure-for-aws.md) or VirtualBox, then you must also configure endpoints for these ports (or use port-forwarding for VirtualBox).

| Machine                                | Ports                                 | Open Ports                                                                                      |
|----------------------------------------|---------------------------------------|-------------------------------------------------------------------------------------------------|
| DeployR server machine                 | Tomcat ports: <br />- `8000` (Tomcat default port)<br />- `8001` (Tomcat HTTPS port)           | To the outside                                                                                  |
| DeployR server machine                 | - `8006` (DeployR event console port) | To the public IP of DeployR server AND to the public IP of *each* grid node machine<sup>1</sup> |
| Remote grid node machines<sup>1</sup> | RServe ports:<br />- `8004` (RServe connection port)<br />- `8005` (RServe cancel port)          | To the public IP of the DeployR server|

<sup>1</sup> Only DeployR Enterprise offers the ability to expand your Grid framework for load distribution by [installing and configuring additional grid nodes](deployr-installing-configuring.md#grid-node-install).

---

>[!NOTE]
>-   If you have made any changes to the port numbers designated for communications between DeployR and any of its dependencies, add those port numbers instead.
>-   **For NFS ports** for external directory support, see the Configuration section of the [Managing External Directories for Big Data](deployr-admin-manage-big-data.md#setting-up-nfs-setup) document.

### Changing Default Passwords

DeployR is delivered with two user accounts: `admin` and `testuser`. You must change the default password to the `admin` account the first time you log in.

1.  Go to the DeployR landing page.

    After installing, you can log into the DeployR landing page at `http://<DEPLOYR_SERVER_IP>:8000/deployr/landing`. `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR main server machine. If you do not have a username or password, please contact your administrator.

2.  Log in as the default administrator using the following:  
    username: `admin`       password: `changeme`

    You are redirected to the reset password page.

3.  Enter a new password and confirm this new password for `admin`.

	>To manage and create new user accounts, log into the **Administration Console** as `admin` and go to the **Users** tab and [use these instructions](deployr-admin-console/deployr-admin-console-user-accounts.md#creating-new-user-accounts). We also recommend that you change the passwords to the other [default user accounts](deployr-admin-console/deployr-admin-console-user-accounts.md#preconfigured-user-accounts).

### Configuring Public Access

When the wrong IP is defined, you will not be able to access to the DeployR landing page or other DeployR components after installation or reboot. In some cases, the wrong IP address may be automatically assigned during installation or when the machine is rebooted, and that address may not be the only IP address for this machine or may not be publicly accessible. If this case, you must update the server Web context address.

To fix this issue, ensure that:

1.  The appropriate external server IP address and port number are defined as the **Server web context**. For Azure & AWS services: Set the server Web context to the external **Public IP**. (Learn more: [Azure setup](deployr-admin-configure-for-azure.md#setting-the-server-web-context) | [AWS setup](deployr-admin-configure-for-aws.md) ).

2.  The autodetection of the IP address is disabled.

**To make changes to the IP address:**

-   Either, edit the **Server web context** and **Disable IP auto-detection** settings the [Administration Console's **Server Policies**](deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties) tab. Log into the **Administration Console** as described in the [preceding section](#changing-default-passwords).

-   Or, you can run the [server Web context script](deployr-admin-diagnostics-troubleshooting.md#landing-page-cannot-be-reached).

### Configuring the DeployR Grid

>**More DeployR Power:**  
>DeployR Enterprise offers the ability to expand your Grid framework for load distribution by installing and [configuring](deployr-admin-console/deployr-admin-managing-the-grid.md#creating-new-nodes) additional grid nodes.
>Get DeployR Enterprise today to take advantage of great DeployR features like [enterprise security](deployr-admin-security.md) and [a scalable grid framework](deployr-admin-console/deployr-admin-managing-the-grid.md). Note that DeployR Enterprise is part of Microsoft R Server.

1.  Log into the **Administration Console** as described in the [preceding section](#changing-default-passwords).

2.  Click **The Grid** in the main menu.

3.  If any additional grid nodes were installed, besides the DeployR Default Node, click **New Grid Node** and configure the **Name**, **Host**, **Operating Type** and **External Directory** for each node [using these instructions](deployr-admin-console/deployr-admin-managing-the-grid.md#creating-new-nodes).

>Grid node configurations are dependent on a specific version of DeployR; therefore, any migrated configurations are not forward compatible. You must redefine any grid nodes in the Administration Console.

### Configuring SELinux (Linux Only)

Configure SELinux to permit access to DeployR using one of these options:

-   To benefit from the protection of SELinux, enable SELinux and configure the appropriate SELinux exception policies as described here: <http://wiki.centos.org/HowTos/SELinux>

-   Disable SELinux so that access to DeployR is allowed as follows:

    1.  In `/etc/selinux/config`, set `SELINUX=disabled`.
    2.  Save the changes.
    3.  Reboot the machine.

## Uninstalling DeployR

Follow the instructions provided for your operating system.

### Uninstalling on Linux

Remember to uninstall DeployR on both the main server and any other grid node machines.

>[!IMPORTANT]
>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

**To uninstall the DeployR main server:**

1.  Stop the Tomcat, RServe and MongoDB services. At the prompt, type:

        /home/deployr-user/deployr/8.0.0/stopAll.sh

2.  Remove DeployR, Tomcat, RServe, and MongoDB directories. At the prompt, type:

        rm -rf /home/deployr-user/deployr/8.0.0

3.  Remove extraneous files. At the prompt, type:

        rm -rf /home/deployr-user/deployrdownload

4.  If MongoDB is on a remote machine, then stop the process and remove the associated directories. At the prompt, type:

        /home/deployr-user/deployr/8.0.0/mongo/mongod.sh stop
        rm -rf /home/deployr-user/deployr/8.0.0
        rm -rf /home/deployr-user/deployrdownload

5.  If a non-default data directory (db\_path) for the MongoDB database was used, then remove that directory as well.
    For example, if you configure MongoDB to use the data directory, `/home/deployr-user/mongo/deployr-database`, then you must remove that directory as well. For this example, you would type:

        rm -rf /home/deployr-user/mongo/deployr-database

**To uninstall remote grid nodes:**

Repeat these steps on each grid node machine.

1.  Stop the RServe service. At the prompt, type:

        /home/deployr-user/deployr/8.0.0/rserve/rserve.sh stop

2.  Remove DeployR and RServe directories. At the prompt, type:

        rm -rf /home/deployr-user/deployr/8.0.0

3.  Remove the extraneous files and directory. At the prompt, type:

        rm -rf /home/deployr-user/deployrdownload

### Uninstalling on Windows

1.  Stop the Tomcat, RServe and MongoDB services as [described here](deployr-common-administration-tasks.md#starting-and-stopping-deployr).

2.  Remove DeployR using the Windows instructions for uninstalling a program specific to your version of Windows. For example, on Windows 8, choose **Control Panel &gt; Programs & Features &gt; Uninstall**.

3.  **Manually remove the DeployR install directory**, by default `C:\Program Files\Microsoft\DeployR\8.0\`.

>Remember to uninstall DeployR on both the main server and any other grid node machines.

### Uninstalling on Mac OS X

>[!IMPORTANT]
>Examples are written for user `deployr-user`. For another user, update the commands accordingly.

**To uninstall the DeployR:**

1.  Stop the Tomcat, RServe and MongoDB services. At the prompt, type:

        /Users/deployr-user/deployr/8.0.0/stopAll.sh

2.  Remove DeployR, Tomcat, RServe, and MongoDB directories. At the prompt, type:

        rm -rf /Users/deployr-user/deployr/8.0.0

3.  Remove extraneous files. At the prompt, type:

        rm -rf /Users/deployr-user/deployrdownload

4.  If a non-default data directory (db\_path) for the MongoDB database was used, then remove that directory as well. For example, if you configure MongoDB to use the data directory, `/Users/deployr-user/mongo/deployr-database`, then you must remove that directory as well. For this example, you would type:

        rm -rf /Users/deployr-user/mongo/deployr-database


