---

# required metadata
title: "R Server Client Installation Guide for Teradata"
description: "Microsoft R Server install guide for Teradata clients."
keywords: ""
author: "jeffstokes72"
manager: "jhubbard"
ms.date: "04/20/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# R Server Client Installation Guide for Teradata

## Quick Overview

Microsoft R Server for Teradata is an R-based analytical engine embedded in your Teradata data warehouse. Together with a Microsoft R Server client, it provides a comprehensive set of tools for interacting with the Teradata database and performing in-database analytics. This manual provides detailed instructions for configuring local workstations to submit jobs to run within your Teradata data warehouse. For installing Microsoft R Server for Teradata in the Teradata data warehouse, see the companion manual [*Microsoft R Server Server Installation Manual for Teradata*](r-server-install-teradata-client.md).

> [!NOTE]
> Microsoft R Server for Teradata is required for running Microsoft R Server scalable analytics in-database. If you do not need to run your analytics in-database, but simply need to access Teradata data via Teradata Parallel Transport or ODBC, you do not need to install Microsoft R Server in your Teradata data warehouse. You will, however, need to configure your local workstations as described in this manual.

## System Requirements

Microsoft R Server for Windows has the following system requirements:

**Processor:** 64-bit processor with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

**Operating System:** 64-bit Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2012, and HPC Server 2008 R2. Distributed computing features require access to a cluster running HPC Server Pack 2012 or HPC Server 2008. Only 64-bit operating systems are supported.

**Teradata Version:** Teradata  Tools and Utilities 15.00 or 14.10.

**Memory:** A minimum of 1GB of RAM is required; 4GB or more are recommended.

**Disk Space:** A minimum of 500MB of disk space is required.

Microsoft R Server on Linux systems has the following system requirements:

**Processor:** 64-bit processor with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

**Operating System:** SUSE Linux Enterprise Server 11, Red Hat Enterprise Linux 5, Red Hat Enterprise Linux 6, or fully-compatible equivalents. Only 64-bit operating systems are supported.

**Teradata Version:** Teradata Tools and Utilities 15.00 or 14.10.

**Memory:** A minimum of 1GB of RAM is required; 4GB or more are recommended.

**Disk Space:** A minimum of 500MB of disk space is required.

## Installing the Client Software

The Teradata client software for Windows is available from the [Teradata At Your Service](https://tays.teradata.com/) web site (this web site requires a customer login).

### Installing the Teradata Client on Windows

**If you are using the 14.10 client:**

The Teradata 14.10 Client for Windows is contained in the file BCD0-1560-0000-Windows_14.10.00_V1-1.exe, Teradata Tools and Utilities Windows (the exact file name will change with update releases; the basic process should be similar for all updates of a given major release). Download this file and run it to start the installation process.

Accept defaults until you reach the “Select Features” dialog. Choose the following items:

- ODBC Driver for Teradata
- Teradata Parallel Transporter Base
- Teradata Parallel Transporter Stream
- BTEQ (optional)
- FastExport (optional)
- FastLoad (optional)
- Teradata SQL Assistant (optional)
- Teradata Administrator (optional)

Complete the installation. You may want to define a DSN connection, but this is not required for use with RxTeradata (but can be useful for testing your client and troubleshooting installation problems).

**If you are using the 15.00 client:**

The Teradata 15.00 Client for Windows is contained on Teradata At Your Service in the file BCD0-1740-0000-Windows_15.00.00_V1-1.zip, Teradata Tools and Utilities Windows (the exact file name will change with update releases; the basic process should be similar for all updates of a given major release). Download this file, extract the files from the zip, then run the TTU.exe in the TTU_Foundation_windows\Windows subdirectory to start the installation process.

Accept defaults until you reach the “Select Features” dialog. Choose the following items:

- ODBC Driver for Teradata
- Teradata Parallel Transporter Base
- Teradata Parallel Transporter Stream
- BTEQ (optional)
- FastExport (optional)
- FastLoad (optional)
- Teradata SQL Assistant (optional)
- Teradata Administrator (optional)

Complete the installation. You may want to define a DSN connection, but this is not required for use with RxTeradata (but can be useful for testing your client and troubleshooting installation problems).

**Creating a Teradata DSN:**

A DSN is a common way of encapsulating database connection information. On Windows, you create DSNs with the ODBC Data Source Administrator, found under Administrative Tools in the System and Security section of the Windows Control Panel. To create the DSN, do the following:

1.	Launch the 64-bit ODBC Data Source Administrator.
2.	Click **System DSN**.
3.	Click **Add…**
4.	Select **Teradata** in the list view and then click **Finish**. The ODBC Driver Setup for Teradata Database dialog appears.
5.	In the **Name** field, type **TDDSN**.
6.	In the **Teradata Server Info** field, enter the fully-qualified domain name or IP address of the Teradata server.
7.	Click **OK** to complete the DSN.
8.	Click **OK** to close the ODBC Data Source Administrator.

**Testing the ODBC Connection:**

To test that you can communicate with your Teradata server using your Teradata ODBC driver, use the Teradata client program tdxodbc.exe, as follows:

1.	Open a Command Prompt as Administrator.

2.	Change directory to the directory containing the tdxodbc.exe program (you may need to quote the path:


    	cd C:\Program Files\Teradata\Client\<version>\ODBC Driver for Teradata nt-x8664\Bin


    where <version> is the client version number.

3.	Start the program by typing its name:


    	tdxodbc.exe

4.	At the prompt, enter the DSN name TDDSN.

5.	At the prompt, enter your database user name.

6.	At the prompt, enter your database user password.

7.	If your ODBC connection is successful, you will see something like the following, followed by a prompt to enter a SQL string:

		.....ODBC connection successful.

		ODBC version        = -03.80.0000-
	    DBMS name           = -Teradata-
	    DBMS version        = -15.00.0011  15.00.00.11-
	    Driver name         = -TDATA32.DLL-
	    Driver version      = -15.00.00.01-
	    Driver ODBC version = -03.80-

8.	Enter “quit” to exit.

### Installing the Teradata Client on Linux

**If you are using the 14.10 client:**

The Teradata 14.10 Client for Linux, also known as the TTU 14.10 Teradata Tool and Utilities, is contained in the file BCD0-1560-0000-Linux-i386-x8664_14.10.00_V1-1_101747728.tar.gz, available from the [Teradata At Your Service](https://tays.teradata.com/) web site (this web site requires a customer login). Download this file to a temporary directory, such as /tmp/Teradata, and unpack it using the following commands:

	cd /tmp/Teradata
	tar zxf BCD0-1560-0000-Linux-i386-x8664_14.10.00_V1-1_101747728.tar.gz

The Teradata client setup script is designed to use ksh; if your system does not have ksh, you can install it using yum as follows (you must be root or a sudo user to run yum):

	yum –y install ksh

By default, yum installs ksh into /bin/ksh, but the Teradata setup script expects it in /usr/bin; if necessary, create a symbolic link as follows:

	ln –s /bin/ksh /usr/bin/ksh

Verify that the zip program is installed, as this is required by Microsoft R Server InTeradata operation:

	yum –y install zip

Change directory to the ttu directory created when you unpacked the tarball:

	cd ttu

As root, or as a sudo user, run the setup script as follows:

	./setup.bat

At the prompt “Enter one or more selections (separated by space):”, enter “9 12” to specify the ODBC drivers and the TPT Stream package. You should see the following warnings:

	Warning: The Teradata Parallel Transporter Base software
	         is a prerequisite for the following software:
	         Teradata Parallel Transporter Stream Operator.
	Warning: The Teradata Parallel Transporter Base software
	         will be installed.

	Do you want to continue with the installation? [y/n (default: y)]:

This indicates that the necessary dependencies will also be installed.

If the setup script fails, you can try installing the packages using rpm directly. The rpm files are located in subdirectories of the Linux/ i386-x8664/ directory created under your /tmp/Teradata directory (again, these commands should be run as root or as a sudo user):

1.	Install the tdicu package:

		rpm –ihv tdicu-14.10.00.00-1.noarch.rpm

2.	Source /etc/profile:

		source /etc/profile

3.	Install the TeraGSS package:

		rpm –ihv TeraGSS_linux_x64-14.10.00.00-1.noarch.rpm

	On RHEL6 systems, TeraGSS may not install cleanly. If you see an error about a bad XML file, try copying /opt/Teradata/teragss/linux-i386/14.10.00.00/etc/TdgssUserConfigFile.xml to /opt/teradata/teragss/site, then run /usr/teragss/linux-x8664/14.10.00.00/bin/run_tdgssconfig. This should then complete with a message about something being written to a binary file. Then proceed to the next step.

4.	Install the tdodbc package:

		rpm –ihv tdodbc-14.10.00.00-1.noarch.rpm

5.	Install the cliv2 package:

		rpm -ihv cliv2-14.10.00.00-1.noarch.rpm
		source /etc/profile

6.	Install the tptbase package:

		rpm -ihv tptbase1410-14.10.00.00-1.noarch.rpm
		source /etc/profile

7.	Install the tptstream package:

		rpm -ihv tptstream1410-14.10.00.00-1.noarch.rpm

	On RHEL6 systems, tptstream may complain about missing glibc and nsl dependencies; these can be resolved by using the “yum provides” command to find a package or packages that can provide the missing dependencies, then using yum to install those packages.

8.	After installing the tptstream package, update your system LD_LIBRARY_PATH environment variable to include the path to the 64-bit version of libtelapi.so (typically /opt/teradata/client/14.10/tbuild/lib64) and the path to your unixODBC 2.3 libraries (typically /usr/lib64). (This is most effectively done in the /etc/profile file; be sure to source the file after making any changes to it.)

**If you are using the 15.00 client:**

The Teradata 15.00 Client for Linux, also known as the TTU 15.00 Teradata Tool and Utilities, is contained in the file BCD0-1740-0000-Linux-i386_15.00.00_V1-1_2291720869.tar.gz. , available from the [Teradata At Your Service](https://tays.teradata.com/) web site (this web site requires a customer login). Download this file to a temporary directory, such as /tmp/Teradata, and unpack it using the following commands:

	cd /tmp/Teradata
	tar zxf BCD0-1740-0000-Linux-i386_15.00.00_V1-1_2291720869.tar.gz

The Teradata client setup script is designed to use ksh; if your system does not have ksh, you can install it using yum as follows (you must be root or a sudo user to run yum):

	yum –y install ksh

By default, yum installs ksh into /bin/ksh, but the Teradata setup script expects it in /usr/bin; if necessary, create a symbolic link as follows:

	ln –s /bin/ksh /usr/bin/ksh

Verify that the zip program is installed, as this is required by Microsoft R Server InTeradata operation:

	yum –y install zip

Change directory to the ttu directory created when you unpacked the tarball:

	cd ttu

As root, or as a sudo user, run the setup script as follows:

	./setup.bat

At the prompt “Enter one or more selections (separated by space):”, enter “9 12” to specify the ODBC drivers and the TPT Stream package. You should see the following warnings:

	Warning: The Teradata Parallel Transporter Base software
	         is a prerequisite for the following software:
	         Teradata Parallel Transporter Stream Operator.
	Warning: The Teradata Parallel Transporter Base software
	         will be installed.

	Do you want to continue with the installation? [y/n (default: y)]:

This indicates that the necessary dependencies will also be installed.

After installing the tptstream package, update your system LD_LIBRARY_PATH environment variable to include the path to the 64-bit version of libtelapi.so (typically /opt/teradata/client/15.00/tbuild/lib64) and the path to your unixODBC 2.3 libraries (typically /usr/lib64). (This is most effectively done in the /etc/profile file; be sure to source the file after making any changes to it.) Also, export the NLSPATH environment variable as follows:

    export NLSPATH=/opt/teradata/client/15.00/tbuild/msg/%N:$NLSPATH

#### Updating Your ODBC Driver Manager

Database operations with ODBC depend upon having both an ODBC driver and an ODBC driver manager. Teradata ODBC drivers are provided in a client package that includes an ODBC driver manager; if you will be using Microsoft R Server exclusively with a Teradata database, we recommend that you use this supplied ODBC driver manager. If you will be using Microsoft R Server with other databases in addition to Teradata, we recommend installing unixODBC 2.3.1 for all your ODBC data management.

##### Configuring the Teradata ODBC Driver Manager

To use the ODBC driver manager supplied with the Teradata client package when you have another ODBC driver manager installed (typically unixODBC):

1.	Identify the directory where the existing ODBC driver manager libraries reside (almost certainly /usr/lib64) and cd to that directory:

		cd /usr/lib64

2.	Rename or remove any existing soft link in that directory named libodbc.so.2:

		mv libodbc.so.2 libodbc.so.2.previous

3.	Add a soft link, libodbc.so.2, that points to the Teradata driver manager library libodbc.so (almost certainly in /opt/teradata/client/ODBC_64/lib)

		ln -s /opt/teradata/client/ODBC_64/lib/libodbc.so ./libodbc.so.2

4.	Export an environment variable definition for ODBCINI in the /etc/profile file (which runs for everyone at login), set to the pathname of the currently used odbc.ini file (probably /etc/odbc.ini):

		ODBCINI=/etc/odbc.ini
		export ODBCINI

5.	Logout and log back in so that the profile changes take effect.

##### Configuring the unixODBC 2.3.1 Driver Manager

The unixODBC 2.3.1 driver manager is not available as part of standard RHEL yum repositories. You must, therefore, install it from source. It is important that unixODBC 2.3.1 be the only unixODBC installation on your system, so be sure to perform the following steps:

1.	Ensure that you do not currently have unixODBC installed via yum:

		rpm –qa | grep unixODBC

	This should return nothing; if any packages are listed, use yum to remove them:

		yum remove <package>

2.	Download the unixODBC 2.3.1 sources from www.unixodbc.org.

3.	Unpack the sources, then change directory to the top-level source directory and run the following commands to build and install the source:

		./configure --enable-iconv --with-iconv-char-enc=UTF8 \
		   --with-iconv-ucode-enc=UTF16LE --libdir=/usr/lib64 \
		   --prefix=/usr --sysconfdir=/etc
		make
		make install

After installing unixODBC, edit the file /etc/odbcinst.ini and add a section such as the following (making sure the path to the driver matches that on your machine):

	[Teradata]
	Driver=/opt/teradata/client/ODBC_64/lib/tdata.so
	APILevel=CORE
	ConnectFunctions=YYY
	DriverODBCVer=3.51
	SQLLevel=1

If you will be using RxOdbcData with a DSN, you need to define an appropriate DSN in the file /etc/odbc.ini, such as the following:

	[TDDSN]
	Driver=/opt/teradata/client/ODBC_64/lib/tdata.so
	Description=Teradata running Teradata V1R5.2
	DBCName=my-teradata-db-machine
	LastUser=
	Username=TDUser
	Password=TDPwd
	Database=RevoTestDB
	DefaultDatabase=

#### Installing RODBC

The RODBC package is not required to use RxTeradata, but it can be useful for timing comparisons with other databases. You can download RODBC from the MRAN source package repository at [http://mran.microsoft.com](http://mran.microsoft.com/).

## Installing Microsoft R Server or R Client on the Client

To download and install Microsoft R Server, you will need an MSDN subscription or a Microsoft Volume License Center sign-in. For development purposes, you can install the free Developer edition of R Server on Linux or Windows:

+ [Install R Server for Linux](r-server-install-linux-server.md)	
+ [Install R Server for Windows](r-server-install-windows.md)

Optionally, you can use Microsoft R Client. It also runs on Linux and Windows:

+ [Install R Client on Linux](../r-client/install-on-linux.md)	
+ [Install R Client on Windows](../r-client/install-on-windows.md)

## Testing the Client Installation

After installing your Teradata client software, you should test that you can communicate with your Teradata database. For this, you will typically need a connection string, which may look something like the following:

	"DRIVER=Teradata;DBCNAME=machineNameOrIP;DATABASE=RevoTestDB;UID=myUserID;PWD=myPassword;"

The connection string is typically quite long, but should remain contained on a single input line.

The following commands can be used to verify that your Windows client can communicate with your Teradata data warehouse using the Revolution test database (instructions for creating that database are contained in the [*Microsoft R Server Server Installation Manual for Teradata*](r-server-install-teradata-server.md):

	query <- "SELECT tablename FROM dbc.tables WHERE databasename = 'RevoTestDB' order by tablename"
	connectionString <- "Driver=Teradata;DBCNAME=157.54.160.204;Database=RevoTestDB;Uid=RevoTester;pwd=RevoTester;"


###### ODBC

	rxSetComputeContext("local")
	odbcDS <- RxOdbcData(sqlQuery=query, connectionString = connectionString)
	rxImport(odbcDS)

###### TPT

	rxSetComputeContext("local")
	teradataDS <- RxTeradata(sqlQuery = query, connectionString = connectionString)
	rxImport(teradataDS)

###### InTeradata

	myCluster <- RxInTeradata(
	      connectionString = connectionString,
	      shareDir =  paste("c:/AllShare/",
	      Sys.info()["login"], sep=""),
	      remoteShareDir = "/tmp/revoJobs",
	      revoPath = "/usr/lib64/MRO-for-MRS-9.1.0/R-3.3.3/lib64/R",
	      consoleOutput = TRUE,
	      wait=TRUE,
	      autoCleanup=TRUE,
	      traceEnabled = FALSE,
	      traceLevel = 7
	            )
	rxSetComputeContext(myCluster)

	tdQuery <- "select * from RevoTestDB.claims"
	teradataDS <- RxTeradata(connectionString = rxGetComputeContext()@connectionString, sqlQuery = tdQuery)
	rxSummary( ~., data=teradataDS )

For Linux clients, the only change that should need to be made is to the shareDir argument to the RxInTeradata call that creates the myCluster object:

	myCluster <- RxInTeradata(
	      connectionString = connectionString,
	      shareDir =  paste("/AllShare/",
	      Sys.info()["login"], sep=""),
	      remoteShareDir = "/tmp/revoJobs",
	      revoPath = "/usr/lib64/MRO-for-MRS-9.1.0/R-3.3.3/lib64/R",
	      consoleOutput = TRUE,
	      wait=TRUE,
	        autoCleanup=TRUE,
	        traceEnabled = FALSE,
	        traceLevel = 7
	               )
