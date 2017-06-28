---
# required metadata
title: "Install Microsoft R Server 9.0.1 on Hadoop"
description: "Installation and configuration Microsoft R Server 9.0.1 on Hadoop"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/10/2017"
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
# Install Microsoft R Server 9.0.1 on Hadoop

Older versions of R Server for Hadoop are no longer available on the Microsoft download sites, but if you already have an older distribution, you can follow these instructions to deploy version 9.0.1. For the current release, see [Install R Server for Hadoop](rserver-install-hadoop.md).

**Side-by-side Installation**

You can install major versions of R Server (such as an 8.x and 9.x) side-by-side on Hadoop, but not minor versions. For example, you already installed Microsoft R Server 8.0, you must uninstall it before installing 8.0.5.

**Upgrade Versions**

If you want to replace an older version rather than run side-by-side, you can uninstall the older distribution before installing the new version (there is no in-place upgrade). See [Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md) for instructions.

**Installation Steps**

A summary of setup tasks is as follows:

- Unzip to extract packages and an install script (install.sh)
- Run the install script with a -p parameter (for Hadoop)
- Verify the installation

The install script downloads and installs Microsoft R Open (microsoft-r-server-mro-9.0.x86_64.rpm). This distribution provides the following packages that are new in this version:

- microsoft-r-server-intel-mkl-9.0.rpm       
- microsoft-r-server-packages-9.0.rpm      
- microsoft-r-server-hadoop-9.0.rpm

In contrast with previous releases, this version  comes with a requirement for `root` installation. Non-root installations are not supported.

## Recommendations for installation

We recommend installing R Server on all nodes of the cluster to avoid Hadoop queuing up jobs on nodes that don't actually have R Server. Although the task will eventually get reassigned to a node that has R Server, you will see errors from the worker node and experience unnecessary delay while waiting for the error to resolve.

Microsoft Azure offers virtual machines with Hadoop templates. If you don't have a Hadoop cluster, you can purchase and provision virtual machines on Azure using templates provided by several vendors.

1. Sign in to [Azure portal](https://ms.portal.azure.com).
2. Click **New** in the top left side bar.
3. In the search box, type the name of one of these vendors: Cloudera, HortonWorks, and MapR. Several of these vendors offer sandbox deployments that make it easier to get started.

## System requirements

R Server must be installed on at least one master or client node which will serve as the submit node; it should be installed on as many workers as is practical to maximize the available compute resources. Nodes must have the same version of R Server within the cluster.

Setup checks the operating system and detects the Hadoop cluster, but it doesn't check for specific distributions. Microsoft R Server works with the Hadoop distributions listed here: [Supported platforms](rserver-install-supported-platforms.md)

Microsoft R Server requires Hadoop MapReduce, the Hadoop Distributed File System (HDFS), and Apache YARN. Optionally, Spark version 1.6-2.0 is supported for Microsoft R Server 9.0.1.

In this version, the installer should provide most of the dependencies required by R Server, but if the installer reports a missing dependency, see [Package Dependencies for Microsoft R Server installations on Linux and Hadoop](rserver-install-linux-hadoop-packages.md) for a complete list of the dependencies required for installation.

Minimum system configuration requirements for Microsoft R Server are as follows:

**Processor:** 64-bit CPU with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 CPUs). Itanium-architecture CPUs (also known as IA-64) are not supported. Multiple-core CPUs are recommended.

**Memory:** A minimum of 8 GB of RAM is required for Microsoft R Server; 16 GB or more are recommended. Hadoop itself has substantial memory requirements; see your Hadoop distribution’s documentation for specific recommendations.

**Disk Space:** A minimum of 500 MB of disk space is required on each node for R Server. Hadoop itself has substantial disk space requirements; see your Hadoop distribution’s documentation for specific recommendations.

## Unpack the distribution

Download the software to a writable directory, such as **/tmp**, unpack the distribution and then run the installation script.

The distribution includes one installer for Microsoft R Server. For a gzipped TAR file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as **/tmp**):

1. Log in as root or a user with sudo privileges.
2. Switch to the **/tmp** directory (assuming it's the download location)
3. Unpack the file:
        `[tmp] $ tar zxvf en_r_server_901_for_hadoop_x64_9648875.gz`


## Run the install script

Microsoft R Server 2016 for Hadoop is deployed by running the install script with the **-p** parameter, which you can install at the root, or as super user via `sudo`.

1. Log in as root or a user with sudo privileges (`sudo su`). The following instructions assume user privileges with the sudo override.
2. Verify system repositories are up to date:
		[username] $ `sudo yum clean all`
3. Change to the directory to which you mounted or unpacked the installer (for example, /mnt/mrsimage for an .img file, or /tmp/MRS90HADOOP if you unpacked the tar.gz file):
		[username] $ `cd /tmp`
		[username tmp] $ `cd /MRS90HADOOP`
4. Run the script with the **-p** parameter, specifying the Hadoop component:
		[username tmp MRS90HADOOP] $ `sudo bash install.sh -p`
5. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.
6. Repeat to accept license terms for Microsoft R Server.
7. Installer output shows the packages and location of the log file.

## Verify installation

1. List installed packages and get package names:
        [MRS90HADOOP] $ `yum list \*microsoft\*`
        [MRS90HADOOP] $ `yum list \*deployr\*`
2. Check the version of Microsoft R Open using `rpm -qi`:
		[MRS90HADOOP] $ `rpm -qi microsoft-r-open-mro-3.3.x86_64`
3. Check the version of Microsoft R Server:
        [MRS90HADOOP] $ `rpm -qi microsoft-r-server-packages-9.0.x86_64`
4. Partial output is as follows (note version 9.0.1):

		Name        : microsoft-r-server-packages-9.0     Relocations: /usr/lib64
		Version     : 9.0.1                               Vendor: Microsoft
		. . .

For further verification, check folders and permissions. Following that, you should run the Revo64 program, a sample Hadoop job, and if applicable, a sample Spark job.

**Check folders and permissions**

Each user should ensure that the appropriate user directories exist, and if necessary, create them with the following shell commands:

		$ `hadoop fs -mkdir /user/RevoShare/$USER`
		$ `hadoop fs -chmod uog+rwx /user/RevoShare/$USER`
		$ `mkdir -p /var/RevoShare/$USER`
		$ `chmod uog+rwx /var/RevoShare/$USER`

The HDFS directory can also be created in a user’s R session (provided the top-level /user/RevoShare has the appropriate permissions) using the following RevoScaleR commands (substitute your actual user name for "username"). Run the RevoScaleR commands in a Revo64 session.

		$ `cd MRS90HADOOP`
		$ `Revo64`
		> rxHadoopMakeDir("/user/RevoShare/username")
		> rxHadoopCommand("fs -chmod uog+rwx /user/RevoShare/username")

Output for each command should be `[1] TRUE`.

As part of this process, make sure the base directories /user and /user/RevoShare have `uog+rwx` permissions as well.

**Run programs and sample jobs using sample data**

The next procedure loads sample data and runs the Revo64 program to further verify the installation.

1. Send sample data to HDFS.

		$ hadoop fs -mkdir -p /share/SampleData
		$ hadoop fs -copyFromLocal /usr/lib64/microsoft-r/9.0/lib64/R/library/RevoScaleR/SampleData/AirlineDemoSmall.csv /share/SampleData/
		$ hadoop fs -ls /share/SampleData

2. Start Revo64.

		$ cd MRS90Linux
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

Assuming that the packages for Microsoft R Open and Microsoft R Server are already unpacked, a manual or custom installation must create the appropriate folders and set permissions.

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

Download Microsoft R Open rpm and the Microsoft R Server installer tar.gz file and copy all to /tmp as described in [Standard Command Line Install](install/r-server-install-hadoop-800.md#StandardCommandLineInstall) steps 3 through 8.

The following commands use pdsh and pdcp to distribute and install Microsoft R Server (ensure that each command is run on a single logical line, even if it spans two lines below due to space constraints; lines beginning with “&gt;” indicate commands typed into an interactive pdsh session):

		alias pdshw=’pdsh -w\`cat myhosts.txt\` -R ssh’
		alias pdcpw=’pdcp -w\`cat myhosts.txt\` -R ssh’
		pdshw
		> mkdir -p /var/tmp/revo-install
		> exit
		pdcpw /tmp/MRS90HADOOP.tar.gz /var/tmp/revo-install
		pdshw
		> cd /var/tmp/revo-install; yum clean all
		> tar zxf MRS90HADOOP.tar.gz
		> cd MRS90HADOOP; sudo bash ./install.sh -a -p
		> exit

## Install additional packages on each node using rxExec

Once you have R Server installed on a node, you can the `rxExec` function in RevoScaleR to install additional packages, including third-party packages from CRAN or another repository. For example, to install the `SuppDists` package on all the nodes of your cluster, call `rxExec` as follows:

	rxExec(install.packages, "SuppDists")

## Configure R Server to operationalize your analytics

Developers might want to configure R Server after its installation to benefit from the deployment and consumption of web services on your Hadoop cluster. This optional feature provides a server-based framework for running R code in real time.  We recommend that you set up the ["one-box configuration"](install/operationalize-r-server-one-box-config.md#onebox), where the compute node and web node are on the same machine. If you need to scale the configuration, then you can add web nodes and compute nodes on the other edge nodes. Do not configure a web node or compute node on any data nodes since they are not YARN resources.


## See Also

[Install R on Hadoop overview](rserver-install-hadoop.md)

[Install R Server 8.0.0 on Hadoop](install/r-server-install-hadoop-800.md)

[Install Microsoft R Server on Linux](rserver-install-linux-server.md)

[Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md)

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)
