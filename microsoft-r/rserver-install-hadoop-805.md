---
# required metadata
title: "Install Microsoft R Server version 8.0.5 on Hadoop"
description: "Install Microsoft R Server version 8.0.5 on Hadoop"
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/23/2016"
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
# Install Microsoft R Server 8.0.5 on Hadoop

This article explains how to install version 8.0.5 of Microsoft R Server on a Hadoop cluster.

## What's new in the 8.0.5 installer

Version 8.0.5 includes updated installers for deploying R Server in fewer steps, enabled in part by a slipstream installation of **Microsoft R Open for R server** that comes with most of its dependencies built into the package. Version 8.0.5 has fewer post-install configuration requirements, and also includes the Generally Available (GA) version of rxSpark. If you have an older version of R and would like to upgrade, see [Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md) for instructions.

A summary of setup tasks for version 8.0.5 is as follows:

- Download the software
- Unzip to extract packages and an install script (install.sh)
- Run the install script with a -p parameter (for Hadoop)
- Verify the installation

The install script downloads and installs Microsoft R Open for R Server (microsoft-r-server-mro-8.0.rpm) along with its dependencies, plus the following packages that are new in version 8.0.5:

- microsoft-r-server-intel-mkl-8.0.rpm       
- microsoft-r-server-packages-8.0.rpm      
- microsoft-r-server-hadoop-8.0.rpm

## Recommendations for installation

We recommend installing R Server on all nodes of the cluster to avoid Hadoop queuing up jobs on nodes that don't actually have R. Although the task will eventually get reassigned to a node that has R, you will see errors from the worker node and experience unnecessary delay while waiting for the error to resolve.

We recommend installing Microsoft R Server as `root` on each node of your Hadoop cluster if you want all users to be granted access by default. Non-root installs are supported, but require that the path to the R executable files be added to each user’s path.

Microsoft Azure offers virtual machines with Hadoop templates. If you don't have a Hadoop cluster, you can purchase and provision virtual machines on Azure using templates provided by several vendors.

1. Sign in to [Azure Portal](https://ms.portal.azure.com).
2. Click **New** in the top left side bar.
3. In the search box, type the name of one of these vendors: Cloudera, HortonWorks, and MapR. Several of these vendors offer sandbox deployments that make it easier to get started.

## System requirements

R Server must be installed on at least one master or client node which will serve as the submit node; it should be installed on as many workers as practicable to maximize the available compute resources. Nodes must have the same version of R Server (side-by-side is not supported).

Setup checks the operating system and detects the Hadoop cluster, but it doesn't check for specific distributions. Microsoft R Server 8.0.5 works with the following Hadoop distributions:

- Cloudera CDH 5.5-5.7
- HortonWorks HDP 2.3-2.4
- MapR 5.0-5.1

Microsoft R Server requires Hadoop MapReduce, the Hadoop Distributed File System (HDFS), and Apache YARN. Optionally, Spark version 1.5.0-1.6.1 is supported for Microsoft R Server 8.0.5.

In version 8.0.5, the installer should provide most of the dependencies required by R Server, but if a missing dependency error is reported, see [Package Dependencies for Microsoft R Server installations on Linux and Hadoop](rserver-install-linux-hadoop-packages.md) for a complete list of the dependencies required for installation.

Minimum system configuration requirements for Microsoft R Server are as follows:

**Processor:** 64-bit CPU with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 CPUs). Itanium-architecture CPUs (also known as IA-64) are not supported. Multiple-core CPUs are recommended.

**Operating System:** The Hadoop distribution must be installed on Red Hat Enterprise Linux (RHEL) 6.x and 7.x (or a fully compatible operating system like CentOS), or SUSE Linux Enterprise Server 11 (SLES11). See [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md) for more information.

**Memory:** A minimum of 8 GB of RAM is required for Microsoft R Server; 16 GB or more are recommended. Hadoop itself has substantial memory requirements; see your Hadoop distribution’s documentation for specific recommendations.

**Disk Space:** A minimum of 500 MB of disk space is required on each node for RRE installation. Hadoop itself has substantial disk space requirements; see your Hadoop distribution’s documentation for specific recommendations.

## Download Microsoft R software

Microsoft R Server is distributed in two different formats. Through VLSC, it is in the form of a DVD img file. Through MSDN or Dev Essentials, it is a tar.gz file.

1. Download the Microsoft R Server 2016 distribution, which will either be a DVD img file through VLSC, or a gzipped tar file through Dev Essentials or MSDN. The distribution file includes one installer for Microsoft R Server, along with an installer for DeployR, an optional component. You can obtain the software from these locations:

	- [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409)
	- [MSDN subscription](http://go.microsoft.com/fwlink/?LinkId=717967&clcid=0x409)
	- [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409)

2. Unpack the distribution. If you have an .img file, first mount the file. The following commands create a mount point and mount the file to that mount point:

		mkdir /mnt/mrsimage
		mount –o loop <filename> /mnt/mrsimage

  If you have a gzipped tar file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as /tmp):

For RHEL/CENTOS systems:
  		tar zxvf MRS80RHEL.tar.gz

 For SLES systems:
  		tar zxvf MRS80SLES.tar.gz

3. In either case, you will then want to copy the installer gzipped tar file to a writable directory, such as /tmp:

From the mounted img file:
 		cp /mnt/mrsimage/Microsoft-R-Server-`*`.tar.gz /tmp

  From the unpacked tar file:
		cp /tmp/MRS80*/Microsoft-R-Server-`*`.tar.gz /tmp

4. Unpack the packages and installer script, as follows (the tarball name may include an operating system ID denoted below by `<OS>`):

		cd /tmp
		tar xvzf Microsoft-R-Server-`<OS>`.tar.gz

This installs Microsoft R Server with the standard options (including loading the rpart and lattice packages by default when RevoScaleR is loaded).

## Run the install script

Microsoft R Server 8.0.5 for Hadoop is deployed by running the install script with the **-p** parameter, which you can install at the root, or as super user via `sudo`.

1. Log in as root or a user with sudo privileges. The following instructions assume user privileges with the sudo override.
2. Verify system repositories are up to date:
		[username] $ sudo yum clean all
3. Change to the directory to which you downloaded the rpm (for example, /tmp):
		[username] $ cd /tmp
4. Run the script with the **-p** parameter, specifying the Hadoop component:
		[tmp] $ sudo bash install.sh -p
5. When prompted to accept the license terms for Microsoft R open, click Enter to read the EULA, click **y** to accept the terms, and then click **q** to continue.
6. Installer output shows the packages and location of the log file.
7. Check the version of Microsoft R Open using `rpm -qi`:
		[tmp] $ rpm -qi microsoft-r-server-mro-8.0
8. Check the version of the intel-mkl package:
		[tmp] $ rpm -qi microsoft-r-server-intel-mkl-8.0

9. Check the output to verify version 8.0.5. Partial output appears as follows:

		Name        : microsoft-r-server-mro-8.0   Relocations: /usr/lib64
		Version     : 8.0.5                         Vendor: Microsoft
		. . .

## Verify install

Run the Revo64 program to verify the installation.

1. Send sample data to HDFS.

		[tmp]$ hadoop fs -mkdir -p /share/SampleData
		[tmp]$ hadoop fs -copyFromLocal /usr/lib64/microsoft-r/8.0/lib64/R/library/RevoScaleR/SampleData/AirlineDemoSmall.csv /share/SampleData/
		[ltmp]$ hadoop fs -ls /share/SampleData

2. Start Revo64.

		[tmp MRS_Linux]$ Revo64

3. Run a simple local computation.

This step uses the proprietary Microsoft libraries.

		> rxSummary(~., iris)

Partial output is as follows (showing the first 4 lines).

		Rows Read: 150, Total Rows Processed: 150, Total Chunk Time: 0.003 seconds
		Computation time: 0.010 seconds.
		Call:
		rxSummary(formula = ~., data = iris)

4. Run a sample local job.

This step uses the sample dataset and downloads data from HDFS, confirming that your local session can access HDFS.

Paste the following code into your Revo64 session.

		input <- file.path("/share/SampleData/AirlineDemoSmall.csv")

		colInfo <- list(DayOfWeek = list(type = "factor",
		levels = c("Monday", "Tuesday", "Wednesday", "Thursday","Friday", "Saturday", "Sunday")))

		airDS <- RxTextData(file = input, missingValueString = "M", colInfo  = colInfo, fileSystem = RxHdfsFileSystem())

		adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek, data = airDS)
		adsSummary

Partial output is as follows (showing the last 8 lines).

		DayOfWeek Counts
		Monday    97975
		Tuesday   77725
		Wednesday 78875
		Thursday  81304
		Friday    82987
		Saturday  86159
		Sunday    94975

5. Run a sample Hadoop job.

This step uses the sample dataset to run a Hadoop job.

Paste the following code into your Revo64 session. This snippet differs from the previous snippet by the first line.

		SetComputeContext(RxHadoopMR(consoleOutput=TRUE))
		input <- file.path("/share/SampleData/AirlineDemoSmall.csv")

		colInfo <- list(DayOfWeek = list(type = "factor",
		levels = c("Monday", "Tuesday", "Wednesday", "Thursday","Friday", "Saturday", "Sunday")))

		airDS <- RxTextData(file = input, missingValueString = "M", colInfo  = colInfo, fileSystem = RxHdfsFileSystem())

		adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek, data = airDS)
		adsSummary

Partial output is as follows (showing the first 10 lines).

		======  sandbox.hortonworks.com (Master HPA Process) has started run at Fri Jun 10 18:26:15 2016  ======
		Jun 10, 2016 6:26:21 PM RevoScaleR main
		INFO: DEBUG: allArgs = '[-Dmapred.reduce.tasks=1, -libjars=/usr/hdp/2.4.0.0-169/hadoop/conf,/usr/hdp/2.4.0.0-169/hadoop-mapreduce/hadoop-streaming.jar, /user/RevoShare/A73041E9D41041A18CB93FCCA16E013E/.input, /user/RevoShare/lukaszb/A73041E9D41041A18CB93FCCA16E013E/IRO.iro, /share/SampleData/AirlineDemoSmall.csv, default, 0, /usr/bin/Revo64-8]'
		End of allArgs.
		16/06/10 18:26:26 INFO impl.TimelineClientImpl: Timeline service address:

<a name="ManualInstallation"><a/>
## Manual Installation

An alternative to running the install.sh script is manual installation of each package and component, or building a custom script that satisfies your technical or operational requirements.

Assuming that the packages for Microsoft R Open for R Server and Microsoft R Server 8.0.5 are already installed, a manual or custom installation must accomplish the following:

**RPM Installers**

- `/var/RevoShare/` and `hdfs://user/RevoShare` must exist and have folders for each user running Microsoft R Server in Hadoop or Spark.

-`/var/RevoShare/` and `hdfs://user/RevoShare` must have required permissions.

**DEB Installers**

-`/var/RevoShare/` and `hdfs://user/RevoShare` must exist and have folders for each user running Microsoft R Server in Hadoop or Spark.

-`var/RevoShare/` and `hdfs://user/RevoShare` must have required permissions.

**Cloudera Parcel Installers**

1. Create `/var/RevoShare/` and `hdfs://user/RevoShare`. Parcels cannot create them for you.
2. Give pull permission to both `/var/RevoShare/` and `hdfs://user/RevoShare`.
3. On Cloudera Manager, configure the location of the parcel-repo so parcels can be discovered in the correct location, and change the rate at which Cloudera Manager checks for new parcels.

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
		pdcpw /tmp/MRO-for-MRS--\*.rpm /var/tmp/revo-install
		pdshw
		> cd /var/tmp/revo-install;yum clean all
		> cd /var/tmp/revo-install;yum install MRO-for-MRS-8.0.5-\*.rpm
		> exit
		pdcpw /tmp/Microsoft-R-Server-8.0.5-<OS>.tar.gz /var/tmp/revo-install
		pdshw
		> cd /var/tmp/revo-install;tar zxf Microsoft-R-Server-8.0.5\*.tar.gz
		> cd rrent;./install.sh -y -a -p /usr/lib64/MRO-for-MRS-8.0.5/R-3.2.2
		> exit

## Troubleshoot installation problems
See [Troubleshoot Microsoft R installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md) for tips.

If you want to start over, see [Uninstall Microsoft R Server](rserver-install-uninstall-upgrade.md) for instructions.

## Multi-node installation on a Cloudera Manager System using a Cloudera Manager Parcel

The following steps walk you through a multi-node installation using Cloudera Manager for a Microsoft R Server version 8.0.5 installation. In contrast with an 8.0.0 installation, you can skip the steps for creating a Revolution Customer Service Descriptor.

Two parcels are required:

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

3.  Copy the parcel files to your local parcel-repo, typically /opt/cloudera/parcel-repo:

  From the mounted img file:
		cp /mnt/mrsimage/MRS-8.0.0-1-* /opt/cloudera/parcel-repo

  From the unpacked tar file:
		cp /tmp/MRS80HADOOP/MRS-8.0.0-1-* /opt/cloudera/parcel-repo

4.  You should have the following files in your parcel repo:

	    MRO-3.2.2-1-el6.parcel
	    MRO-3.2.2-1-el6.parcel.sha
	    MRS-8.0.0-1-el6.parcel
	    MRS-8.0.0-1-el6.parcel.sha

  Be sure all the files are owned by root and have 755 permissions (that is, read, write, execute permission for root, and read and execute permissions for group and others).

5.  In your browser, open Cloudera Manager.

6.  Click **Hosts** in the upper navigation bar to bring up the All Hosts page.

7.  Click **Parcels** to bring up the Parcels page.

8.  Click **Check for New Parcels**. MRO 3.2.2-1and MRS 8.0.0-1 should each appear with a **Distribute** button. After clicking Check for New Parcels you may need to click on “All Clusters” under the “Location” section on the left to see the new parcels.

9.  Click the MRO 3.2.2 **Distribute** button. Microsoft R Open will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.

10.  Click **Activate**. Activation prepares Microsoft R Open to be used by the cluster.

11.  Click the MRS 8.0.0-1 **Distribute** button. Microsoft R Server will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.

As a verification step, each user should ensure that the appropriate user directories exist, and if necessary, create them with the following shell commands:

		hadoop fs -mkdir /user/RevoShare/$USER
		hadoop fs -chmod uog+rwx /user/RevoShare/$USER
		mkdir -p /var/RevoShare/$USER
		chmod uog+rwx /var/RevoShare/$USER

The HDFS directory can also be created in a user’s R session (provided the top-level /user/RevoShare has the appropriate permissions) using the following RevoScaleR commands (substitute your actual user name for "username"):

		rxHadoopMakeDir("/user/RevoShare/username")
		rxHadoopCommand("fs -chmod uog+rwx /user/RevoShare/username")

As part of this process make sure to check that the base directories /user and /user/RevoShare have uog+rwx permissions as well.

## Next steps

DeployR is an optional component. See [DeployR Installation](deployr-installation.md) for setup instructions.

To get started with Microsoft R Server on Hadoop, we recommend the [*RevoScaleR Hadoop Getting Started Guide*](scaler-hadoop-getting-started.md). This article is an introduction to RevoScaleR with Hadoop.
