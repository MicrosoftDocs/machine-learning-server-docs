---
# required metadata
title: "Install Microsoft R Server version 8.0.0 on Hadoop"
description: "Install Microsoft R Server version 8.0.0 on Hadoop"
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/14/2016"
ms.topic: ""
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
# Install Microsoft R Server 8.0.0 on Hadoop

This article explains how to install version 8.0.0 of Microsoft R Server on a Hadoop cluster.

## Recommendations for installation

For a first-time installation of Microsoft R Server 8, we recommend [installing Microsoft R Server version 8.0.5](rserver-install-hadoop-805.md) instead of 8.0.0. The 8.0.5 installer performs more system verification, installation, and configuration steps. Version 8.0.5 also includes the Generally Available (GA) version of rxSpark. If you already have 8.0.0 and would like to upgrade, see [Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md) for instructions.

We recommend installing R Server on all nodes of the cluster to avoid Hadoop queuing up jobs on nodes that don't actually have R. Although the task will eventually get reassigned to a node that has R, you will see errors from the worker node and experience unnecessary delay while waiting for the error to resolve.

We recommend installing Microsoft R Server as `root` on each node of your Hadoop cluster if you want all users to be granted access by default. Non-root installs are supported, but require that the path to the R executable files be added to each user’s path.

Microsoft Azure offers virtual machines with Hadoop templates. If you don't have a Hadoop cluster, you can purchase and provision virtual machines on Azure using templates provided by several vendors.

1. Sign in to [Azure Portal](https://ms.portal.azure.com).
2. Click **New** in the top left side bar.
3. In the search box, type the name of one of these vendors: Cloudera, HortonWorks, and MapR. Several of these vendors offer sandbox deployments that make it easier to get started.

## System Requirements

R Server must be installed on at least one master or client node which will serve as the submit node; it should be installed on as many workers as practicable to maximize the available compute resources. Nodes must have the same version of R Server (side-by-side is not supported).

Setup checks the operating system and detects the Hadoop cluster, but it doesn't check for specific distributions. Microsoft R Server 8.0.5 works with the following Hadoop distributions:

- Cloudera CDH 5.0, 5.1, 5.2, 5.3, 5.4
- HortonWorks HDP 1.3.0, HDP 2.0.0, HDP 2.1.0, HDP 2.2.0, HDP 2.3.0
- MapR 3.0.2, MapR 3.0.3, MapR 3.1.0, MapR 3.1.1, MapR 4.0.1, MapR 4.0.2 (provided this version of MapR has been updated to mapr-patch-4.0.2.29870.GA-30600; contact MapR to obtain the patch), MapR 4.1

Microsoft R Server requires Hadoop MapReduce and the Hadoop Distributed File System (HDFS) (for HDP 1.3.0 and MapR 3.x installations), or HDFS, Hadoop YARN, and Hadoop MapReduce2 for CDH5, HDP 2.x, and MapR 4.x installations. The HDFS, YARN, and MapReduce clients must be installed on all nodes on which you plan to run Microsoft R Server, as must Microsoft R Server itself.

Your cluster installation must include the C APIs contained in the libhdfs package; these are required for Microsoft R Server. See your Hadoop documentation for information on installing this package.

If Setup reports an error about uninstalled dependencies, see [Package Dependencies for Microsoft R Server installations on Linux and Hadoop](rserver-install-linux-hadoop-packages.md) for a complete list.

Minimum system configuration requirements for Microsoft R Server are as follows:

**Processor:** 64-bit CPU with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 CPUs). Itanium-architecture CPUs (also known as IA-64) are not supported. Multiple-core CPUs are recommended.

**Operating System:** The Hadoop distribution must be installed on Red Hat Enterprise Linux 6.x (or a fully compatible operating system like CentOS). For HDP 1.3.0 systems *only*, RHEL 5.x operating systems are also supported.See [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md) for more information.

**Memory:** A minimum of 8 GB of RAM is required for Microsoft R Server; 16 GB or more are recommended. Hadoop itself has substantial memory requirements; see your Hadoop distribution’s documentation for specific recommendations.

**Disk Space:** A minimum of 500 MB of disk space is required on each node for R Server. Hadoop itself has substantial disk space requirements; see your Hadoop distribution’s documentation for specific recommendations.

<a name="DownloadR"></a>
## Download Microsoft R Components

Deploying Microsoft R 8.0.0 on a Hadoop cluster is a 2-part installation of the following software in the order listed:

Component | Download location |
----------|-------------------|
Microsoft R Open for Microsoft R Server | [Microsoft R Open for Microsoft R Server](http://go.microsoft.com/fwlink/?LinkID=699383&clcid=0x409) <br /><br />Microsoft R Open for Microsoft R Server is distributed as an rpm file (or, if you are installing via Cloudera Manager, a Cloudera Manager parcel file).|
Microsoft R Server 2016 | Available through the following distribution channels, depending upon how you purchased the product:<br />[Volume Licensing Service Center](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) (VLSC)<br />[MSDN subscription](http://go.microsoft.com/fwlink/?LinkId=717967&clcid=0x409)<br />[Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409)<br /><br />Microsoft R Server is distributed in two different formats. Through VLSC, it is in the form of a DVD img file. Through MSDN or Dev Essentials, it is a tar.gz file.

## Recommendations for Microsoft R Server on a Hadoop cluster

We recommend installing Microsoft R Server as `root` on each node of your Hadoop cluster. This grants access to all users by default. Non-root installs are supported, but require adding the path to the R executable files to each user’s path.

If you are installing on a Cloudera Manager system using a parcel install, see [Installing on a Cloudera Manager System Using a Cloudera Manager Parcel](rserver-install-hadoop-create-r-package-cloudera-manager.md) for details.

<a name="StandardCommandLineInstall"></a>
## Standard Command Line Install

For most users, installing on the cluster means simply running the standard Microsoft R Server installers on each node of the cluster:

1. Log in as root or a user with sudo privileges. If using sudo, precede commands requiring root privileges with `sudo`.
2. Make sure the system repositories are up to date prior to installing Microsoft R Open for Microsoft R Server:
 		sudo yum clean all
3. [Download the Microsoft R Open for Microsoft R Server rpm](http://go.microsoft.com/fwlink/?LinkId=699383&clcid=0x409).
4.  Change to the directory to which you downloaded the rpm (for example, /tmp):
		cd /tmp
5. Use the following command to install Microsoft R Open for Microsoft R Server:
		yum install MRO-for-MRS-8.0.0.`*`.x86_64.rpm
6. [Download and unpack the Microsoft R Server 2016 distribution](#DownloadR), which will either be a DVD img file (if you obtained Microsoft R Server via Microsoft Volume Licensing) or a gzipped tar file (if you obtained Microsoft R Server via MSDN). The distribution file includes one or more Microsoft R Server installers, along with installers for DeployR, an optional additional component.
7. If you have an img file, you must first mount the file. The following commands create a mount point and mount the file to that mount point:

		mkdir /mnt/mrsimage
		mount –o loop <filename> /mnt/mrsimage

  If you have a gzipped tar file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as /tmp):

  For RHEL/CENTOS systems:
		tar zxvf MRS80RHEL.tar.gz

  For SLES systems:
		tar zxvf MRS80SLES.tar.gz

8. In either case, you will then want to copy the installer gzipped tar file to a writable directory, such as /tmp:

  From the mounted img file:
		cp /mnt/mrsimage/Microsoft-R-Server-`*`.tar.gz /tmp

  From the unpacked tar file:
		cp /tmp/MRS80*/Microsoft-R-Server-`*`.tar.gz /tmp

9. Unpack and run the installer script, as follows (the tarball name may include an operating system ID denoted below by <OS>):

		cd /tmp
		tar xvzf Microsoft-R-Server-<OS>.tar.gz
		pushd rrent
		./install.sh –a –y -p /usr/lib64/MRO-for-MRS-8.0.0/R-3.2.2
		popd

This installs Microsoft R Server with the standard options (including loading the rpart and lattice packages by default when RevoScaleR is loaded).

<a name="DistributedInstallation"><a/>
## Distributed Installation

If you have multiple nodes, you can automate the installation across nodes using any distributed shell. (You can, of course, automate installation with a non-distributed shell such as bash using a for-loop over a list of hosts, but distributed shells usually provide the ability to run commands over multiple hosts simultaneously.) Examples include [dsh ("Dancer’s shell")](http://www.netfort.gr.jp/~dancer/software/dsh.html.en), [pdsh (Parallel Distributed Shell)](http://sourceforge.net/projects/pdsh/), [PyDSH (the Python Distributed Shell)](http://pydsh.sourceforge.net/), and [fabric](http://www.fabfile.org/). Each distributed shell has its own methods for specifying hosts, authentication, and so on, but ultimately all that is required is the ability to run a shell command on multiple hosts. (It is convenient if there is a top-level copy command, such as the pdcp command that is part of pdsh, but not necessary—the “cp” command can always be run from the shell.)

Obtain the Microsoft R Open for Microsoft R Server rpm and the Microsoft R Server installer tar.gz file and copy all to /tmp as described in [Standard Command Line Install](#StandardCommandLineInstall) steps 3 through 8.

The following commands use pdsh and pdcp to distribute and install Microsoft R Server (ensure that each command is run on a single logical line, even if it spans two lines below due to space constraints; lines beginning with “&gt;” indicate commands typed into an interactive pdsh session):

		alias pdshw=’pdsh -w\`cat myhosts.txt\` -R ssh’
		alias pdcpw=’pdcp -w\`cat myhosts.txt\` -R ssh’
		pdshw
		> mkdir -p /var/tmp/revo-install
		> exit
		pdcpw /tmp/MRO-for-MRS-8.0.0-\*.rpm /var/tmp/revo-install
		pdshw
		> cd /var/tmp/revo-install;yum clean all
		> cd /var/tmp/revo-install;yum install MRO-for-MRS-8.0.0-\*.rpm
		> exit
		pdcpw /tmp/Microsoft-R-Server-8.0.0-<OS>.tar.gz /var/tmp/revo-install
		pdshw
		> cd /var/tmp/revo-install;tar zxf Microsoft-R-Server-8.0.0\*.tar.gz
		> cd rrent;./install.sh -y -a -p /usr/lib64/MRO-for-MRS-8.0.0/R-3.2.2
		> exit

## Installing the Microsoft R Server JAR File

Using Microsoft R Server in Hadoop requires the presence of the Microsoft R Server Java Archive (JAR) file scaleR-hadoop-0.1-SNAPSHOT.jar. This file is installed in the scripts directory of your Microsoft R Server installation (typically at /usr/lib64/MRS-8.0/scripts), and is typically linked to the standard Hadoop jar file location (typically $HADOOP\_HOME/lib or $HADOOP\_PREFIX/lib).

If you are installing R Server as a non-root user, you may need to obtain root access to link this file appropriately.

## Environment Variables for Hadoop

The file RevoHadoopEnvVars.site in the scripts directory of your Microsoft R Server installation (typically at /usr/lib64/MRS-8.0/scripts) should be sourced by all users, by adding the following line to the .bash\_profile file:

	. /usr/lib64/MRS-8.0/scripts/RevoHadoopEnvVars.site

The period (“.”) at the beginning is part of the command, and must be included.

This file sets the following environment variables for use by Microsoft R Server:

**HADOOP_HOME** This should be set to the directory containing the Hadoop files.

**HADOOP_CMD**: This should be set to the command used to invoke Hadoop

**HADOOP_CLASSPATH**: This should be set to include the full path to the R Server jar files (typically /usr/lib64/MRS-8.0/scripts).

**CLASSPATH**: This should be a fully expanded CLASSPATH with access to all required Hadoop JAR files.

**JAVA_LIBRARY_PATH:** If necessary, this should be set to include paths to the directories containing Hadoop jar files.

**HADOOP_STREAMING**: This should be set to the path of the Hadoop streaming jar file.

These environment variables are written to the file automatically on installation, but can be edited by hand if necessary.

## Creating Directories for Microsoft R Server

Each user should ensure that the appropriate user directories exist, and if necessary, create them with the following commands:

	hadoop fs -mkdir /user/RevoShare/$USER
	hadoop fs -chmod uog+rwx /user/RevoShare/$USER
	mkdir -p /var/RevoShare/$USER
	chmod uog+rwx /var/RevoShare/$USER

The HDFS directory can also be created in a user’s R session (provided the top-level /user/RevoShare has the appropriate permissions) using the following RevoScaleR commands (substitute your actual user name for “username”):

	rxHadoopMakeDir("/user/RevoShare/username")
	rxHadoopCommand("fs -chmod uog+rwx /user/RevoShare/username")

## Installing on a Cloudera Manager System Using a Cloudera Manager Parcel

If you are running a Cloudera Hadoop cluster managed by Cloudera Manager, and if Cloudera itself was installed via a Cloudera Manager parcel, you can use the Microsoft R Server Cloudera Manager parcels to install Microsoft R Server on all the nodes of your cluster. Two parcels are required:

- Microsoft R Open parcel—installs open-source R and additional open-source components on the nodes of your Cloudera cluster
- Microsoft R Server parcel—installs proprietary components on the nodes of your Cloudera cluster

Microsoft R Server requires several packages that may not be in a default Red Hat Enterprise Linux installation. Run the following yum command as root to install them:

		yum install gcc-gfortran cairo-devel python-devel \\
		tk-devel libicu-devel

Run this command on all the nodes of your cluster that will be running Microsoft R Server. You can also use a distributed shell such as pdsh to distribute the command (here we use the pdshw alias we defined in [Distributed Installation](#DistributedInstallation); this alias includes the list of host names and specifies the use of ssh):

		pdshw yum install gcc-gfortran cairo-devel python-devel tk-devel libicu-devel

Once you have installed the Microsoft R Server prerequisites, install the Cloudera Manager parcels as follows:

1.  [Download the Microsoft R Open for Microsoft R Server Cloudera Manager parcel.](http://go.microsoft.com/fwlink/?LinkId=699383&clcid=0x409) (Note that the parcel consists of two files, the parcel itself and its associated .sha file. They may be packaged as a single .tar.gz file for convenience in downloading, but that must be unpacked and the two files copied to the parcel-repo for Cloudera Manager to recognize them as a parcel.)

2.  Download and unpack the Microsoft R Server 2016 distribution, which will either be a DVD img file (if you obtained Microsoft R Server via Microsoft Volume Licensing) or a gzipped tar file (if you obtained Microsoft R Server via MSDN or Dev Essentials). The distribution file includes the required Cloudera Parcel files.

  If you have an img file, you must first mount the file. The following commands create a mount point and mount the file to that mount point:

		mkdir /mnt/mrsimage
		mount –o loop MRS80HADOOP.img /mnt/mrsimage

  If you have a gzipped tar file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as /tmp):

		tar zxvf MRS80HADOOP.tar.gz

3. Copy the parcel files to your local parcel-repo, typically /opt/cloudera/parcel-repo:

  From the mounted img file:
		cp /mnt/mrsimage/MRS-8.0.0-1-* /opt/cloudera/parcel-repo

  From the unpacked tar file:
		cp /tmp/MRS80HADOOP/MRS-8.0.0-1-* /opt/cloudera/parcel-repo

4. You should have the following files in your parcel repo:

	    MRO-3.2.2-1-el6.parcel
	    MRO-3.2.2-1-el6.parcel.sha
	    MRS-8.0.0-1-el6.parcel
	    MRS-8.0.0-1-el6.parcel.sha

  Be sure all the files are owned by root and have 755 permissions (that is, read, write, execute permission for root, and read and execute permissions for group and others).

5. In your browser, open Cloudera Manager.

6. Click **Hosts** in the upper navigation bar to bring up the All Hosts page.

7. Click **Parcels** to bring up the Parcels page.

8. Click **Check for New Parcels**. MRO 3.2.2-1and MRS 8.0.0-1 should each appear with a **Distribute** button. After clicking Check for New Parcels you may need to click on “All Clusters” under the “Location” section on the left to see the new parcels.

9. Click the MRO 3.2.2 **Distribute** button. Microsoft R Open will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.

10. Click **Activate**. Activation prepares Microsoft R Open to be used by the cluster.

11. Click the MRS 8.0.0-1 **Distribute** button. Microsoft R Server will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.

12. Click **Activate**. Activation prepares Microsoft R Server to be used by the cluster.

When you have installed the parcels, download, install, and run the Revolution Custom Service Descriptor as follows:

1. Copy the Custom Service Descriptor file MRS\_CONFIG-8.0.jar from either your mounted image file or your unpacked MRS80HADOOP directory to the Cloudera CSD directory, typically /opt/cloudera/csd.

2. Stop and restart the cloudera-scm-server service using the following shell commands:

		service cloudera-scm-server stop
		service cloudera-scm-server start

3.  Confirm the CSD is installed by checking the Custom Service Descriptor list in Cloudera Manager at <hostname>/cmf/csd/list, where <hostname> is the host name of your Cloudera Manager server.

4.  On the Cloudera Manager home page, click the dropdown beside the cluster name and click **Add a Service**.

5.  From the Add Service Wizard, select **Revolution R** and click **Continue**.

6.  Select **all hosts**, and click **Continue**.

7.  Accept defaults through the remainder of the wizard.

Each user should ensure that the appropriate user directories exist, and if necessary, create them with the following shell commands:

		hadoop fs -mkdir /user/RevoShare/$USER
		hadoop fs -chmod uog+rwx /user/RevoShare/$USER
		mkdir -p /var/RevoShare/$USER
		chmod uog+rwx /var/RevoShare/$USER

The HDFS directory can also be created in a user’s R session (provided the top-level /user/RevoShare has the appropriate permissions) using the following RevoScaleR commands (substitute your actual user name for "username"):

		rxHadoopMakeDir("/user/RevoShare/username")
		rxHadoopCommand("fs -chmod uog+rwx /user/RevoShare/username")

As part of this process make sure to check that the base directories /user and /user/RevoShare have uog+rwx permissions as well.

## Verify Installation

After completing installation, do the following to verify that Microsoft R Server will actually run commands in Hadoop:

1. If the cluster is security-enabled, obtain a ticket using kinit (for Kerberos authentication) or mapr password (for MapR wire security).

2. Start Microsoft R Server on a cluster node by typing Revo64 at a shell prompt.

3. At the R prompt ">", enter the following commands (these commands are drawn from the [*RevoScaleR Hadoop Getting Started Guide*](scaler-hadoop-getting-started.md)*,* which explains what all of them are doing. For now, we are just trying to see if everything works):

		bigDataDirRoot <- "/share"
		myHadoopCluster <- RxHadoopMR(consoleOutput=TRUE)
		rxSetComputeContext(myHadoopCluster)
		source <- system.file("SampleData/AirlineDemoSmall.csv",
			package="RevoScaleR")
		inputDir <- file.path(bigDataDirRoot,"AirlineDemoSmall")
		rxHadoopMakeDir(inputDir)
		rxHadoopCopyFromLocal(source, inputDir)
		hdfsFS <- RxHdfsFileSystem()
		colInfo <- list(DayOfWeek = list(type = "factor",
			levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
			"Friday", "Saturday", "Sunday")))
		airDS <- RxTextData(file = inputDir, missingValueString = "M",
			colInfo = colInfo, fileSystem = hdfsFS)
		adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek,
			data = airDS)
		adsSummary

If you installed Microsoft R Server in a non-default location, you must specify the location using both the hadoopRPath and revoPath arguments to RxHadoopMR:

	myHadoopCluster <- RxHadoopMR(hadoopRPath=/path/to/Revo64,
		revoPath=/path/to/Revo64)

If you see the following, congratulations:

	Call:
	rxSummary(formula = ~ArrDelay + CRSDepTime + DayOfWeek, data = airDS)
	Summary Statistics Results for: ~ArrDelay + CRSDepTime + DayOfWeek
	Data: airDS (
	RxTextData Data Source)
	File name: /share/AirlineDemoSmall
	Number of valid observations: 6e+05
	Name Mean StdDev Min Max ValidObs MissingObs
	ArrDelay 11.31794 40.688536
	-
	86.000000 1490.00000 582628 17372
	CRSDepTime
	13.48227 4.697566 0.016667 23.98333 600000 0
	Category Counts for DayOfWeek
	Number of categories: 7
	Number of valid observations: 6e+05
	Number of missing observations: 0
	DayOfWeek Counts
	Monday 97975
	Tuesday 77725
	Wednesday 78875
	Thursday 81304
	Friday 82987
	Saturday 86159
	Sunday 94975

Next try to run a simple rxExec job:

	rxExec(list.files)

That should return a list of files in the native file system. If either the call to rxSummary or the call to rxExec results in an error, see [Troubleshooting the Installation](#troubleshooting-the-installation), for a few of the more common errors and how to fix them.

## Next Steps

To get started with Microsoft R Server on Hadoop, we recommend the [*RevoScaleR Hadoop Getting Started Guide*](scaler-hadoop-getting-started.md). This provides a tutorial introduction to using RevoScaleR with Hadoop.
