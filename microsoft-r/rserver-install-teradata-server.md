---

# required metadata
title: "R Server installation for Teradata servers"
description: "Install Microsoft R Server 9.0.1 on Teradata servers."
keywords: ""
author: "jeffstokes72"
manager: "jhubbard"
ms.date: "12/09/2016"
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
ms.technology: "r-server"
ms.custom: ""

---

# R Server 9.0.1 installation instructions for Teradata Servers

Microsoft R Server for Teradata is an R-based analytical engine embedded in your Teradata data warehouse. Together with a Microsoft R Server client, it provides a comprehensive set of tools for interacting with the Teradata database and performing in-database analytics. This article provides detailed instructions for installing Microsoft R Server 9.0.1 for Teradata in the Teradata data warehouse. For configuring local workstations to submit jobs to run within your Teradata data warehouse, see [Microsoft R Server Client Installation Manual for Teradata](rserver-install-teradata-client.md).

>[!NOTE]
>Microsoft R Server for Teradata is required for running Microsoft R Server scalable analytics in-database. If you do not need to run your analytics in-database, but simply need to access Teradata data via Teradata Parallel Transport or ODBC, you do not need to install Microsoft R Server in your Teradata data warehouse. You will, however, need to configure your local workstations as described in [Microsoft R Server Client Installation Manual for Teradata](rserver-install-teradata-client.md).

## System Requirements

Microsoft R Server for Teradata has the following system requirements:

**Processor:** 64-bit processor with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

**Operating System:** SUSE Linux Enterprise Server. Microsoft R Server has different operating system requirements depending on the version you install. See [Supported platforms](rserver-install-supported-platforms.md) for specifics. Only 64-bit operating systems are supported.

**Teradata Version:** Teradata 15.10, 15.00, or 14.10.

**Memory:** A minimum of 1GB of RAM is required; 4GB or more are recommended.

**Disk Space:** A minimum of 500MB of disk space is required

The following specific libraries must be installed on the Teradata appliance from the SLES 11 installation DVDs:

* SLES-11-SP1-DVD-x86_64-GM-DVD1.iso/suse/x86_64/ghostscript-fonts-std-8.62-32.27.31.x86_64.rpm

* SLES-11-SP1-DVD-x86_64-GM-DVD1.iso/suse/x86_64/libicu-4.0-7.24.11.x86_64.rpm

* SLE-11-SP1-SDK-DVD-x86_64-GM-DVD1.iso/suse/x86_64/libstdc++45-4.5.0_20100414-1.2.7.x86_64.rpm

* SLE-11-SP1-SDK-DVD-x86_64-GM-DVD1.iso/suse/x86_64/libgfortran43-4.3.4_20091019-0.7.35.x86_64.rpm

* SLE-11-SP1-SDK-DVD-x86_64-GM-DVD1.iso/suse/x86_64/gcc43-fortran-4.3.4_20091019-0.7.35.x86_64.rpm

* SLE-11-SP1-SDK-DVD-x86_64-GM-DVD1.iso/suse/x86_64/gcc-fortran-4.3-62.198.x86_64.rpm

**Dependency:** The file libstdc++6-5.3.1.x86_64.rpm is **included** with MRS 9.0.1. This may cause conflicts with existing rpms. If you opt to perform this install, the rpm will need to be installed on **each node**. If you cannot install this rpm MRS 9.0.1 will not work.

A method of doing so is to execute the following command on each node:

```
zypper -n install --force-resolution libstdc++6-5.3.1.x86_64.rpm
```

## Installing the Microsoft R Server rpms

We recommend using the Teradata Parallel Update Tool (PUT) to install the Microsoft R Server rpms.

PUT is a browser-based utility, and we recommend that you upgrade to the latest version (3.06.01). This version contains the *PUT Customer Mode*, which is the easiest way to install Microsoft R Server.

To get started, do the following:

  1. Download the Microsoft R Server distribution appropriate for your Linux distribution. Microsoft R Server consists of two separate downloads, as follows:

		Microsoft R Open 3.3.2 (can be found at [the mro repository](https://mran.microsoft.com/install/mro/3.3.2/microsoft-r-open-3.3.2.tar.gz).)

		Microsoft R Server 9.0.1 for Teradata

  2.  Download the Microsoft R Open rpm file for your Teradata appliance’s operating system (SLES 11).

  3.  Download and unpack the Microsoft R Server 9.0.1 distribution, which will either be a DVD iso file (if you obtained Microsoft R Server via Microsoft Volume Licensing) or a gzipped tar file (if you obtained Microsoft R Server via MSDN). (can be found on [MSDN](https://msdn.microsoft.com/en-us/subscriptions/downloads/?fileid=70865#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70865))

  4.  If you have an iso file, you must first mount the file. The following commands create a mount point and mount the file to that mount point (replacing **MSDN** with the name of the current ISO downloaded):

		mkdir /mnt/mrsimage
		mount –o loop MSDN.iso /mnt/mrsimage

	If you have a gzipped tar file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as /tmp):

		tar -zxf en_r_server_901_for_teradata_x64_9649029.gz

		This creates the directory MRS90Teradata
		This is where the you can find the libstdc++6-5.3.1.x86_64.rpm (dependency discussed above). If you cannot install this rpm MRS 9.0.1 will not work.

  5. Agree to license agreements:
		/MRS90Teradata/ MRO_EULA.txt
		/MRS90Teradata/ MRO_EULA.txt


  6. Copy the following files to the Customer Mode directory (which you may need to create) _/var/opt/teradata/customermodepkgs_:

		microsoft-r-open-3.3.2.tar.gz
		MRS90Teradata/RPM/microsoft-r-server-packages-9.0.rpm 
		MRS90Teradata/RPM/microsoft-r-server-teradata-9.0.rpm


  7. Point your Java-enabled browser to `https://<HOSTIP>:8443/put` where `<HOSTIP>` is the IP address of your Teradata data warehouse node and log in to Customer Mode using a *Linux* account such as root (*not* a database account).

To install the Microsoft R Server rpms on all the nodes, do the following:

  1.  The initial PUT screen asks you to select an operation. Select **Install/Upgrade Software**.

  2.  At the **Install/Upgrade Software: Customer Mode** step, confirm the summary and click **Next**.

  3.  You may receive a dialog box saying that PUT was unable to find any packages to auto-select. If you see this dialog box, click OK.

  4.  Select microsoft-r-server-teradata-9.0.

  5.  Select all Groups and click &gt;&gt; to install on all groups, then click Next.

  6.  Review the list of dependencies and files to be installed, then click Yes.

  7.  Click Next

  8.  Clear the “Restore normal Teradata Vital Infrastructure (TVI) escalation path (recommended)” check box.

  9.  Click Finish.


## Setting Up the Revolution Analytics Database

After you have installed the rpms, change directory to the "teradata" directory and run the revoTdSetup.sh script as follows:

	Depending on Teradata version cd to:
	/usr/lib64/microsoft-r/3.3/teradata/1510 or
	/usr/lib64/microsoft-r/3.3/teradata/1500 or
	/usr/lib64/microsoft-r/3.3/teradata/1410
	
	chmod u+x ./revoTdSetup.sh
	./revoTdSetup.sh


Enter the parent user database name and password at the prompts. Do not modify this script, and in particular, do not modify the name of the revoAnalytics_Zqht2 database. The database administrator running the script must have specific grants, as described in Chapter 5, Adding and Configuring Users.

If you have previously run Microsoft R Server on your Teradata database, restart the database before proceeding.

## Adding and Configuring Users

All users who will interact with the database, including the database administrator, need to have specific grants. The following example provides an existing “sysdba” user the required grants to run the revoTdSetup.sh script:

	grant all on sysdba to sysdba with grant option;

If you don’t already have such a user, the following example creates a database administrator user named "sysdba" with the required grants to run the revoTdSetup.sh script:

	# Create the user who will run revoTdSetup.sh (e.g., sysdba):
	bteq .logon localhost/dbc,dbc
	create user SYSDBA from dbc as perm=25E9, spool=15E9, temporary=15E9, password=sysdba;
	grant select, execute on dbc to sysdba with grant option;
	grant all on sysdba to sysdba with grant option;
	grant select on sys_calendar to sysdba with grant option;
	grant select on dbc to public;

You will often want to allow a user account to run jobs on data in other databases also. The lines shown below are examples of the types of permissions your Revolution users may require. (For convenience, these steps are organized according to the purpose of the permissions granted.)

  1.  First, you will need to grant permissions for the user account to work with the Revolution database.  Logon to bteq with your admin account, and run the following lines.  Substitute the new user account name for 'ut1' in the sample lines below.

			-- grant ut1 various permissions on revoAnalytics_Zqht2 DB
			grant execute procedure on revoAnalytics_Zqht2.InitRevoJob to ut1;
			grant execute procedure on revoAnalytics_Zqht2.StartRevoJobXsp to ut1;
			grant execute procedure on revoAnalytics_Zqht2.GetRevoJobState to ut1;
			grant execute procedure on revoAnalytics_Zqht2.RemoveRevoJobResults to ut1;
			grant execute function on revoAnalytics_Zqht2.FinishRevoJob to ut1;
			grant execute function on revoAnalytics_Zqht2.DistributeRevoJobDataUdf to ut1;
			grant execute function on revoAnalytics_Zqht2.revoAnalyticsTblOp to ut1;
			grant select on revoAnalytics_Zqht2.RevoAnalyticsJobResults to ut1;

			-- the following two create view and drop view grants are for the
			-- table operator PARTITION BY work-around
			-- for Teradata's Issue DR166609, which was fixed in 14.10.02d
			grant create view on revoAnalytics_Zqht2 to ut1;
			grant drop view on revoAnalytics_Zqht2 to ut1;

  2.  Next you will need to grant permissions for the user account and the revolution database to work with data in the user account database.  Run the following lines, substituting your user account name for 'ut1'.

			-- grant ut1 rights on db with data to be analyzed -
			-- in this case itself
			grant all on ut1 to ut1;

			-- give revoAnalytics_Zqht2 rights on database with data to
			-- be analyzed - in this case ut1
			grant select on ut1 to revoAnalytics_Zqht2;
			grant create table on ut1 to revoAnalytics_Zqht2;
			grant drop table on ut1 to revoAnalytics_Zqht2;

  3.  Finally, you will want to grant permissions on any other databases you wish that account to have access to.  Run the following lines, substituting your user account name for 'ut1', and your other database name for 'RevoTestDB'.

			-- grant ut1 rights on db with data to be analyzed
			grant select on RevoTestDB to ut1;

			-- give revoAnalytics_Zqht2 rights on database with data to
			-- be analyzed
			grant select on RevoTestDB to revoAnalytics_Zqht2;
			grant create table on RevoTestDB to revoAnalytics_Zqht2;

## Managing Memory in In-Teradata Computations

Because a Teradata cluster node typically has many worker processes (AMPs) running constantly, it is important to limit how much memory a distributed analysis consumes. Microsoft R Server provides stored procedures in the revoAnalytics\_Zqht2 scheduler database that allow a database administrator to set the memory limits for master and worker processes working on a RevoScaleR job. By default, the master process memory limit is 2000 MB (approximately 2GB) and the worker process memory limit is 1000MB (approximately 1GB).

These limits may be customized by a database administrator using the SetMasterMemoryLimitMB() and SetWorkerMemoryLimitMB() stored procedures, defined in the revoAnalytics\_Zqht2 database created on installation.

For example, to set the master memory limit to 3000MB:

	call revoAnalytics_Zqht2.SetMasterMemoryLimitMB(3000);

To set the worker memory limit to 1500MB:

	call revoAnalytics_Zqht2.SetWorkerMemoryLimitMB(1500);

The current memory limits can be viewed in the revoAnalytics\_Zqht2.Properties table.

Note that memory limits that have been changed are in effect immediately, and no restart of the database is required.

## Maintaining the Revolution Database

Two tables in the revoAnalytics\_Zqht2 database may require periodic cleanup:

- **RevoJobs** – one row is added to this table for each job. Each job is assigned a job ID between 1 and 2,147,483,647. The table is not automatically cleaned up. If never cleaned up, it can grow to up to 2,147,483,647 rows. In version 8.0.0 and earlier, values did not recycle unless old rows were deleted, and the next ID was always computed as the max ID in the table plus 1, which could result in integer overflow. In version 8.0.1 and later, after reaching the maximum value, the job IDs cycle back to 1 (or a relatively low number) harmlessly.

- **RevoAnalyticsJobResults** – multiple rows are added for each job. It is automatically cleaned up if wait = TRUE in RxInTeradata(), but not if wait = FALSE. In version 8.0.1 and later, if the job IDs have cycled, then any rows matching a recycled ID are deleted before the ID is reused.

## Installing Additional R Packages

The R community is an active, open-source community, and new packages extending R’s capacity for statistics, data analysis, graphics, and interconnectivity are added frequently. The most up-to-date source for these third-party packages is the Comprehensive R Archive Network (CRAN), a network of servers around the world that store open-source R distributions, extensions, documentation and binaries. The repository (<http://cran.r-project.org/>) has grown from only 12 packages in 1997 to over 5300 packages currently. However, CRAN does not maintain source packages by R version, and Microsoft R Server for Teradata is seldom the latest R version, so packages from CRAN may be incompatible with Microsoft R Server for Teradata. Microsoft’s MRAN archive (<http://mran.microsoft.com>), however, maintains daily snapshots of the CRAN repository. Users may take advantage of this repository and download the chosen packages from any specific date, but notice that they are completely external to Teradata.

In most cases, the natural place to install additional R packages is to the client workstation. Installation to the Teradata data warehouse is required only if you plan to use the package’s functions as transform functions inside RevoScaleR functions, in which case the packages will actually need to be loaded in-database. If you need a package for this purpose, you can do so as follows:

To manually distribute and install the package:

  1.  Download the package and any required dependencies from <http://mran.microsoft.com/>.
  2.  Copy the downloaded packages to each node of your data warehouse.
  3.  For each package, run the command “R CMD INSTALL *package.*tar.gz” on each node. (If your data warehouse is equipped with the psh command, you can use that to run the command on all the nodes in parallel.)

## Removing Microsoft R Server

To remove Microsoft R Server from your computer, run the following commands:

	psh rpm -e microsoft-r-server-teradata-9.0
	psh rpm -e microsoft-r-server-packages-9.0
	psh rpm -e microsoft-r-open-foreachiterators-3.3
	psh rpm -e microsoft-r-open-mkl-3.3
	psh rpm -e microsoft-r-open-mro-3.3

Restart the DBS to unload MRS from udfsectsk processes.