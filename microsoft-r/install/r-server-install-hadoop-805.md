---
# required metadata
title: "Install Microsoft R Server 2016 on Hadoop"
description: "Installation and configuration Microsoft R Server 2016 (version 8.0.5) on Hadoop"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/03/2016"
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
ms.technology: "r-server"
ms.custom: ""

---
# Install Microsoft R Server 2016 (8.0.5) on Hadoop

Older versions of R Server for Hadoop are no longer available on the Microsoft download sites, but if you already have an older distribution, you can follow these instructions to deploy version 8.0.5. For the current release, see [Install R Server for Hadoop](r-server-install-hadoop-901.md).

## About the R Server 2016 installer

Microsoft R Server 2016 provides updated installers that allow you to deploy R Server in fewer steps, enabled in part by a slipstream installation of **Microsoft R Open for R Server 2016** that comes with most dependencies built into the package. In addition to the installers, several new features and enhancements are [new in this release](../rserver-whats-new.md). If you have an older version of R and would like to upgrade, see [Uninstall Microsoft R Server to upgrade to a newer version](r-server-install-uninstall-upgrade.md) for instructions.

A summary of setup tasks for R Server 2016 is as follows:

- Download the software
- Unzip to extract packages and an install script (install.sh)
- Run the install script with a -p parameter (for Hadoop)
- Verify the installation

The install script downloads and installs Microsoft R Open for R Server 2016 (microsoft-r-server-mro-8.0.rpm). This distribution provides the following packages that are new in this version:

- microsoft-r-server-intel-mkl-8.0.rpm       
- microsoft-r-server-packages-8.0.rpm      
- microsoft-r-server-hadoop-8.0.rpm

In contrast with previous releases, this version  comes with a requirement for `root` installation. Non-root installations are not supported in R Server 2016.

## Recommendations for installation

We recommend installing R Server on all nodes of the cluster to avoid Hadoop queuing up jobs on nodes that don't actually have R. Although the task will eventually get reassigned to a node that has R, you will see errors from the worker node and experience unnecessary delay while waiting for the error to resolve.

Microsoft Azure offers virtual machines with Hadoop templates. If you don't have a Hadoop cluster, you can purchase and provision virtual machines on Azure using templates provided by several vendors.

1. Sign in to [Azure Portal](https://ms.portal.azure.com).
2. Click **New** in the top left side bar.
3. In the search box, type the name of one of these vendors: Cloudera, HortonWorks, and MapR. Several of these vendors offer sandbox deployments that make it easier to get started.

## System requirements

R Server must be installed on at least one master or client node which will serve as the submit node; it should be installed on as many workers as is practical to maximize the available compute resources. Nodes must have the same version of R Server within the cluster.

Setup checks the operating system and detects the Hadoop cluster, but it doesn't check for specific distributions. Microsoft R Server 2016 works with the following Hadoop distributions:

- Cloudera CDH 5.5-5.7
- HortonWorks HDP 2.3-2.4
- MapR 5.0-5.1

Microsoft R Server requires Hadoop MapReduce, the Hadoop Distributed File System (HDFS), and Apache YARN. Optionally, Spark version 1.5.0-1.6.1 is supported for Microsoft R Server 2016 (version 8.0.5).

In this version, the installer should provide most of the dependencies required by R Server, but if the installer reports a missing dependency, see [Package Dependencies for Microsoft R Server installations on Linux and Hadoop](r-server-install-linux-hadoop-packages.md) for a complete list of the dependencies required for installation.

Minimum system configuration requirements for Microsoft R Server are as follows:

**Processor:** 64-bit CPU with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 CPUs). Itanium-architecture CPUs (also known as IA-64) are not supported. Multiple-core CPUs are recommended.

**Operating System:** The Hadoop distribution must be installed on Red Hat Enterprise Linux (RHEL) 6.x and 7.x (or a fully compatible operating system like CentOS), or SUSE Linux Enterprise Server 11 (SLES11). See [Supported platforms in Microsoft R Server](r-server-install-supported-platforms.md) for more information.

**Memory:** A minimum of 8 GB of RAM is required for Microsoft R Server; 16 GB or more are recommended. Hadoop itself has substantial memory requirements; see your Hadoop distribution’s documentation for specific recommendations.

**Disk Space:** A minimum of 500 MB of disk space is required on each node for R Server. Hadoop itself has substantial disk space requirements; see your Hadoop distribution’s documentation for specific recommendations.

## Unpack the distribution

Unpack the software to a writable directory, such as **/tmp**, and then run the installation script.

The distribution includes one installer for Microsoft R Server. For a gzipped TAR file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as **/tmp**):

1. Log in as root or a user with sudo privileges.
2. Switch to the **/tmp** directory (assuming it's the download location)
3. Unpack the file:
        `[tmp] $ tar zxvf en_microsoft_r_server_for_hadoop_x64_8944644.tar.gz`

**Unpacking an ISO file**

Volume licensing makes the download available as an ISO file. To unpack this file, create a mount point, and then mount the ISO file to that mount point:

      `mkdir /mnt/mrsimage`
      `mount –o loop sw_dvd5_r_server_2016_english_-2_for_HadoopRdHt_mlf_x20-98709.iso /mnt/mrsimage`

The download file is **sw_dvd5_r_server_2016_english_-2_for_HadoopRdHt_mlf_x20-98709.iso**.

## Run the install script

Microsoft R Server 2016 for Hadoop is deployed by running the install script with the **-p** parameter, which you can install at the root, or as super user via `sudo`.

1. Log in as root or a user with sudo privileges. The following instructions assume user privileges with the sudo override.
2. Verify system repositories are up to date:
		[username] $ sudo yum clean all
3. Change to the directory to which you mounted or unpacked the installer (for example, /mnt/mrsimage for an .img file, or /tmp/MRS80HADOOP if you unpacked the tar.gz file):
		[username] $ cd /tmp
		[username tmp] $ cd /MRS80HADOOP
4. Run the script with the **-p** parameter, specifying the Hadoop component:
		[username tmp MRS80HADOOP] $ sudo bash install.sh -p
5. When prompted to accept the license terms for Microsoft R open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.
6. Installer output shows the packages and location of the log file.
7. Check the version of Microsoft R Open using `rpm -qi`:
		[username tmp MRS80HADOOP] $ rpm -qi microsoft-r-server-mro-8.0
8. Optionally, you can also use `rpm -qi` to check the version of microsoft-r-server-intel-mkl-8.0, microsoft-r-server-packages-8.0, and microsoft-r-server-hadoop-8.0.
9. Check the output to verify version 8.0.5. Partial output appears as follows:

		Name        : microsoft-r-server-mro-8.0   Relocations: /usr/lib64
		Version     : 8.0.5                         Vendor: Microsoft
		. . .

## Verify install

As a verification step, check folders and permissions. Following that, you should run the Revo64 program, a sample Hadoop job, and if applicable, a sample Spark job.

**Check folders and permissions**

Each user should ensure that the appropriate user directories exist, and if necessary, create them with the following shell commands:

		$ hadoop fs -mkdir /user/RevoShare/$USER
		$ hadoop fs -chmod uog+rwx /user/RevoShare/$USER
		$ mkdir -p /var/RevoShare/$USER
		$ chmod uog+rwx /var/RevoShare/$USER

The HDFS directory can also be created in a user’s R session (provided the top-level /user/RevoShare has the appropriate permissions) using the following RevoScaleR commands (substitute your actual user name for "username"). Run the RevoScaleR commands in a Revo64 session.

		$ cd MRS80Linux
		$ Revo64
		> rxHadoopMakeDir("/user/RevoShare/username")
		> rxHadoopCommand("fs -chmod uog+rwx /user/RevoShare/username")

Output for each command should be `[1] TRUE`.

As part of this process, make sure the base directories /user and /user/RevoShare have `uog+rwx` permissions as well.

**Run programs and sample jobs using sample data**

The next procedure loads sample data and runs the Revo64 program to further verify the installation.

1. Send sample data to HDFS.

		$ hadoop fs -mkdir -p /share/SampleData
		$ hadoop fs -copyFromLocal /usr/lib64/microsoft-r/8.0/lib64/R/library/RevoScaleR/SampleData/AirlineDemoSmall.csv /share/SampleData/
		$ hadoop fs -ls /share/SampleData

2. Start Revo64.

		$ cd MRS80Linux
		$ Revo64

3. Run a simple local computation. This step uses the proprietary Microsoft libraries.

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

		rxSetComputeContext(RxHadoopMR(consoleOutput=TRUE))
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

6. Run a sample Spark job.

		rxSetComputeContext(RxSpark(consoleOutput=TRUE, executorMem="1g", driverMem="1g", executorOverheadMem="1g", numExecutors=2))
		    input <- file.path("/share/SampleData/AirlineDemoSmall.csv")

		    colInfo <- list(DayOfWeek = list(type = "factor",
		    levels = c("Monday", "Tuesday", "Wednesday", "Thursday","Friday", "Saturday", "Sunday")))

		    airDS <- RxTextData(file = input, missingValueString = "M", colInfo  = colInfo, fileSystem = RxHdfsFileSystem())

		    adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek, data = airDS)
		    adsSummary

7. To quit the program, type `q()` at the command line with no arguments.

<a name="ManualInstallation"><a/>
## Manual Installation

An alternative to running the install.sh script is manual installation of each package and component, or building a custom script that satisfies your technical or operational requirements.

Assuming that the packages for Microsoft R Open for R Server and Microsoft R Server 2016 are already installed, a manual or custom installation must create the appropriate folders and set permissions.

**RPM or DEB Installers**

- `/var/RevoShare/` and `hdfs://user/RevoShare` must exist and have folders for each user running Microsoft R Server in Hadoop or Spark.
- `/var/RevoShare/<user>` and `hdfs://user/RevoShare/<user>` must have full permissions (all read, write, and executive permissions for all users).

**Cloudera Parcel Installers**

1. Create `/var/RevoShare/` and `hdfs://user/RevoShare`. Parcels cannot create them for you.
2. Give `/var/RevoShare/<user>` and `hdfs://user/RevoShare/<user>` for each user.
3. Grant full permissions to both `/var/RevoShare/<user>` and `hdfs://user/RevoShare/<user>`.

<a name="DistributedInstallation"><a/>
## Distributed Installation

If you have multiple nodes, you can automate the installation across nodes using any distributed shell. (You can, of course, automate installation with a non-distributed shell such as bash using a for-loop over a list of hosts, but distributed shells usually provide the ability to run commands over multiple hosts simultaneously.) Examples include [dsh ("Dancer’s shell")](http://www.netfort.gr.jp/~dancer/software/dsh.html.en), [pdsh (Parallel Distributed Shell)](http://sourceforge.net/projects/pdsh/), [PyDSH (the Python Distributed Shell)](http://pydsh.sourceforge.net/), and [fabric](http://www.fabfile.org/). Each distributed shell has its own methods for specifying hosts, authentication, and so on, but ultimately all that is required is the ability to run a shell command on multiple hosts. (It is convenient if there is a top-level copy command, such as the pdcp command that is part of pdsh, but not necessary—the “cp” command can always be run from the shell.)

Obtain the Microsoft R Open for Microsoft R Server rpm and the Microsoft R Server installer tar.gz file and copy all to /tmp as described in [Standard Command Line Install](r-server-install-hadoop-800.md#standardcommandlineinstall) steps 3 through 8.

The following commands use pdsh and pdcp to distribute and install Microsoft R Server (ensure that each command is run on a single logical line, even if it spans two lines below due to space constraints; lines beginning with “&gt;” indicate commands typed into an interactive pdsh session):

		alias pdshw=’pdsh -w\`cat myhosts.txt\` -R ssh’
		alias pdcpw=’pdcp -w\`cat myhosts.txt\` -R ssh’
		pdshw
		> mkdir -p /var/tmp/revo-install
		> exit
		pdcpw /tmp/MRS80HADOOP.tar.gz /var/tmp/revo-install
		pdshw
		> cd /var/tmp/revo-install; yum clean all
		> tar zxf MRS80HADOOP.tar.gz
		> cd MRS80HADOOP; sudo bash ./install.sh -a -p
		> exit

## Multi-node installation using Cloudera Manager

The following steps walk you through a multi-node installation using Cloudera Manager to create a Cloudera Manager parcel for a Microsoft R Server 2016 installation. In contrast with an 8.0.0 installation, you can skip the steps for creating a Revolution Customer Service Descriptor.

Two parcels are required:

- *Microsoft R Open* parcel installs open-source R and additional open-source components on the nodes of your Cloudera cluster.
- *Microsoft R Server* parcel installs proprietary components on the nodes of your Cloudera cluster.

Install the Cloudera Manager parcels as follows:

1. [Download the Microsoft R Open for Microsoft R Server Cloudera Manager parcel.](http://go.microsoft.com/fwlink/?LinkId=699383&clcid=0x409). Note that the parcel consists of two files, the parcel itself and its associated .sha file. They may be packaged as a single .tar.gz file for convenience in downloading, but that must be unpacked and the two files copied to the parcel-repo for Cloudera Manager to recognize them as a parcel.

2. Download and unpack the Microsoft R Server 2016 distribution, which will either be a DVD img file (if you obtained Microsoft R Server via Microsoft Volume Licensing) or a gzipped tar file (if you obtained Microsoft R Server via MSDN or Dev Essentials). The distribution file includes the required Cloudera Parcel files.

  If you have an img file, you must first mount the file. The following commands create a mount point and mount the file to that mount point:

		mkdir /mnt/mrsimage
		mount –o loop MRS80HADOOP.img /mnt/mrsimage

  If you have a gzipped tar file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as /tmp):

		tar zxvf MRS80HADOOP.tar.gz

3. Copy the parcel files to your local parcel-repo, typically /opt/cloudera/parcel-repo:

  From the mounted img file:
		cp /mnt/mrsimage/MRS-8.0.5-* /opt/cloudera/parcel-repo

  From the unpacked tar file:
		cp /tmp/MRS80HADOOP/MRS-8.0.5-* /opt/cloudera/parcel-repo

4. You should have the following files in your parcel repo:

		MRO-8.0.5-el6.parcel
		MRO-8.0.5-el6.parcel.sha
		MRS-8.0.5-el6.parcel
		MRS-8.0.5-el6.parcel.sha

  Be sure all the files are owned by root and have 644 permissions (read, write, permission for root, and read permission for groups and others).

5. In your browser, open Cloudera Manager.

6. Click **Hosts** in the upper navigation bar to bring up the All Hosts page.

7. Click **Parcels** to bring up the Parcels page.

8. Click **Check for New Parcels**. MRO 8.0.5 and MRS 8.0.5 should each appear with a **Distribute** button. After clicking Check for New Parcels you may need to click on “All Clusters” under the “Location” section on the left to see the new parcels.

9. Click the MRO 8.0.5 **Distribute** button. Microsoft R Open will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.

10. Click **Activate**. Activation prepares Microsoft R Open to be used by the cluster.

11. Click the MRS 8.0.5 **Distribute** button. Microsoft R Server will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.

## Troubleshoot installation problems

See [Troubleshoot Microsoft R installation problems on Hadoop](r-server-install-hadoop-troubleshoot.md) for tips.

If you want to start over, see [Uninstall Microsoft R Server](r-server-install-uninstall-upgrade.md) for instructions.

## Next steps

Developers might want to install DeployR, an optional component that provides a server-based framework for running R code in real time. See [DeployR Installation](../deployr/deployr-installation.md) for setup instructions.

To get started, we recommend the [ScaleR Getting Started Guide for Hadoop](../scaler-hadoop-getting-started.md).

## See Also

[Install R on Hadoop overview](r-server-install-hadoop.md)

[Install R Server 8.0.0 on Hadoop](r-server-install-hadoop-800.md)

[Install Microsoft R Server on Linux](r-server-install-linux-server.md)

[Uninstall Microsoft R Server to upgrade to a newer version](r-server-install-uninstall-upgrade.md)

[Troubleshoot R Server installation problems on Hadoop](r-server-install-hadoop-troubleshoot.md)
