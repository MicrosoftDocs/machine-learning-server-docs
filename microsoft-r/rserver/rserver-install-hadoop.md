---

# required metadata
title: "R Server Hadoop Configuration Guide"
description: "RevoScaleR Hadoop installation and configuration."
keywords: ""
author: "richcalaway"
manager: "mblythe"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
ms.prod: "rserver"
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

# R Server Hadoop Configuration Guide

## Introduction

Microsoft R Server is the scalable data analytics solution, and it is designed to work seamlessly whether your computing environment is a single-user workstation, a local network of connected servers, or a cluster in the cloud. This manual is intended for those who need to configure a Hadoop cluster for use with Microsoft R Server.

### Obtaining the Software

Microsoft R Server consists of two parts: Microsoft R Open for Microsoft R Server, and Microsoft R Server 2016. These parts are downloaded from separate locations:

- [Microsoft R Open for Microsoft R Server](http://go.microsoft.com/fwlink/?LinkID=699383&clcid=0x409)
- Microsoft R Server 2016 is available through the following distribution channels, depending upon how you purchased the product:

    - [Volume Licensing Service Center](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) (VLSC)
    - [MSDN](http://go.microsoft.com/fwlink/?LinkId=717967&clcid=0x409)
    - [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409)

Microsoft R Open for Microsoft R Server is distributed as an rpm file (or, if you are installing via Cloudera Manager, a Cloudera Manager parcel file). Microsoft R Server is distributed in two different formats: if you obtain it via VLSC, it is in the form of a DVD img file; if you obtain it via MSDN or Dev Essentials, it is a tar.gz file.

### System Requirements

Microsoft R Server works with the following Hadoop distributions:

- Cloudera CDH 5.0, 5.1, 5.2, 5.3, 5.4
- HortonWorks HDP 1.3.0, HDP 2.0.0, HDP 2.1.0, HDP 2.2.0, HDP 2.3.0
- MapR 3.0.2, MapR 3.0.3, MapR 3.1.0, MapR 3.1.1, MapR 4.0.1, MapR 4.0.2 (provided this version of MapR has been updated to mapr-patch-4.0.2.29870.GA-30600; contact MapR to obtain the patch), MapR 4.1

Your cluster installation must include the C APIs contained in the libhdfs package; these are required for Microsoft R Server. See your Hadoop documentation for information on installing this package. The Hadoop distribution must be installed on Red Hat Enterprise Linux 5 or 6, or fully compatible operating system. Microsoft R Server should be installed on all nodes of the cluster.

Microsoft R Server requires Hadoop MapReduce and the Hadoop Distributed File System (HDFS) (for HDP 1.3.0 and MapR 3.x installations), or HDFS, Hadoop YARN, and Hadoop MapReduce2 for CDH5, HDP 2.x, and MapR 4.x installations. The HDFS, YARN, and MapReduce clients must be installed on all nodes on which you plan to run Microsoft R Server, as must Microsoft R Server itself.

Minimum system configuration requirements for Microsoft R Server are as follows:

**Processor:** 64-bit CPU with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 CPUs). Itanium-architecture CPUs (also known as IA-64) are not supported. Multiple-core CPUs are recommended.

**Operating System:** Red Hat Enterprise Linux 6.0, 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, or 6.7. Only 64-bit operating systems are supported. (For HDP 1.3.0 systems *only*, RHEL 5.x operating systems are also supported.)

**Memory:** A minimum of 4GB of RAM is required for Microsoft R Server; 8GB or more are recommended. Hadoop itself has substantial memory requirements; see your Hadoop distribution’s documentation for specific recommendations

**Disk Space:** A minimum of 500MB of disk space is required on each node for RRE installation. Hadoop itself has substantial disk space requirements; see your Hadoop distribution’s documentation for specific recommendations.

**Package Dependencies:** Microsoft R Server, like most Linux applications, depends upon a number of Linux packages. A few of these, listed in Table 1, are explicitly required by Microsoft R Server. The remainder are in turn required by these dependencies. These are automatically installed while the automated script is running. These are listed in Table 2.

Table 1. Packages Explicitly Required by Microsoft R Server

| ed                | cairo-devel     |
|-------------------|-----------------|
| tk-devel          | make            |
| gcc-objc          | gcc-c++         |
| readline-devel    | libtiff-devel   |
| ncurses-devel     | pango-devel     |
| perl              | texinfo         |
| libgfortran       | pango           |
| libicu            | libjpeg\*-devel |
| ghostscript-fonts | gcc-gfortran    |
| libSM-devel       | libicu-devel    |
| libXmu-devel      | bzip2-devel     |

Table 2. Secondary Dependencies Installed for Microsoft R Server

| cloog-ppl         | cpp                      |
|-------------------|--------------------------|
| font-config-devel | freetype                 |
| freetype-devel    | gcc                      |
| glib2-devel       | libICE-devel             |
| libobjc           | libpng-devel             |
| libstdc++-devel   | libX11-devel             |
| libXau-devel      | libxcb-devel             |
| libXext-devel     | libXft-devel             |
| libXmu            | libXrender-devel         |
| libXt-devel       | mpfr                     |
| pixman-devel      | ppl glibc-headers        |
| tcl gmp           | tcl-devel kernel-headers |
| tk                | xorg-x11-proto-devel     |
| zlib-devel        | -                       |

### Basic Hadoop Terminology

The following terms apply to computers and services within the Hadoop cluster, and define the roles of hosts within the cluster:

**Hadoop 1.x Installations (HDP 1.3.0, MapR 3.x)**

**JobTracker:** The Hadoop service that distributes MapReduce tasks to specific nodes in the cluster. The JobTracker queries the NameNode to find the location of the data needed for the tasks, then distributes the tasks to TaskTracker nodes near (or co-extensive with) the data. For small clusters, the JobTracker may be running on the NameNode, but this is not recommended for production use.

**NameNode:** A host in the cluster that is the master node of the HDFS file system, managing the directory tree of all files in the file system. In small clusters, the NameNode may host the JobTracker, but this is not recommended for production use.

**TaskTracker:** Any host that can accept tasks (Map, Reduce, and Shuffle operations) from a JobTracker. TaskTrackers are usually, but not always, also DataNodes, so that tasks assigned to the TaskTracker can work on data on the same node.

**DataNode:** A host that stores data in the Hadoop Distributed File System. DataNodes connect to the NameNode, and responds to requests from the NameNode for file system operations.

**Hadoop 2.x Installations (CDH5, HDP 2.x, MapR 4.0.x)**

**Resource Manager:** The Hadoop service that distributes MapReduce and other Hadoop tasks to specific nodes in the cluster. The Resource Manager takes over the scheduling functions of the old JobTracker, determining which nodes are appropriate for the current job.

**NameNode:** A host in the cluster that is the master node of the HDFS file system, managing the directory tree of all files in the file system.

**Application Master:** New in MapReduce2/YARN, the application master takes over the task progress coordination from the old JobTracker, working with node managers on the individual task nodes. The application master negotiates with the Resource Manager for cluster resources, which are allocated as a set of containers, with each container running an application-specific task on a particular node.

**NodeManager:** Node managers manage the containers allocated for a given task on a given node, coordinating with the Resource Manager and the Application Masters. NodeManagers are usually, but not always, also DataNodes, and most frequently the containers on a given node are working with data on the same node.

**DataNode:** A host that stores data in the Hadoop Distributed File System. DataNodes connect to the NameNode, and responds to requests from the NameNode for file system operations.

### Verifying the Hadoop Installation

We assume you have already installed Hadoop on your cluster. If not, use the documentation provided with your Hadoop distribution to help you perform the installation; Hadoop installation is complicated and involves many steps--following the documentation carefully does not guarantee success, but it does make troubleshooting easier. In our testing, we have found the following documents helpful:

- [Cloudera CDH5, package install](http://go.microsoft.com/fwlink/?LinkId=699464&clcid=0x409)
- [Cloudera CDH5, Cloudera Manager parcel install](http://go.microsoft.com/fwlink/?LinkId=699465&clcid=0x409)
- [Hortonworks HDP 1.3](http://go.microsoft.com/fwlink/?LinkId=699466&clcid=0x409)
- [Hortonworks HDP 2.3](http://go.microsoft.com/fwlink/?LinkId=699467&clcid=0x409)
- [Hortonworks HDP 2.x, Ambari install](http://go.microsoft.com/fwlink/?LinkId=717421&clcid=0x409)
- [Hortonworks HDP 1.x or 2.0/2.1, Ambari install](http://go.microsoft.com/fwlink/?LinkId=699468&clcid=0x409)
- [MapR 3.x install](http://go.microsoft.com/fwlink/?LinkId=699469&clcid=0x409)
- [MapR 4.1](http://go.microsoft.com/fwlink/?LinkId=699471&clcid=0x409)

If you are using Cloudera Manager, it is important to know if your installation was via packages or parcels; the Microsoft R Server Cloudera Manager parcel can be used only with parcel installs. If you have installed Cloudera Manager via *packages*, do not attempt to use the RRE Cloudera Manager parcel; use the standard Microsoft R Server for Linux installer instead.

It is useful to confirm that Hadoop itself is running correctly before attempting to install Microsoft R Server on the cluster. Hadoop comes with example programs that you can run to verify that your Hadoop installation is running properly, in the jar file hadoop-mapreduce-examples.jar. The following command should display a list of the available examples:

	hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar

(On MapR, the quick installation installs the Hadoop files to /opt/mapr by default; the path to the examples jar file is */opt/mapr/hadoop/hadoop-0.20.2/hadoop-0.20.2-dev-examples.jar*. Similarly, on Cloudera Manager parcel installs, the default path to the examples is */opt/cloudera/parcels/CDH/lib/hadoop-mapreduce-examples.jar*.)

The following runs the pi example, which uses Monte Carlo sampling to estimate pi; the 5 tells Hadoop to use 5 mappers, the 300 says to use 300 samples per map:

	hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar pi 5 300

If you can successfully run one or more of the Hadoop examples, your Hadoop installation was successful and you are ready to install Microsoft R Server.

### Adjusting Hadoop Memory Limits (Hadoop 2.x Systems Only)

On YARN-based Hadoop systems (CDH5, HDP 2.x, MapR 4.x), we have found that the default settings for Map and Reduce memory limits are inadequate for large RevoScaleR jobs. The memory available for R is the difference between the container’s memory limit and the memory given to the Java Virtual Machine. To allow large RevoScaleR jobs to run, we need to modify four properties in mapred-site.xml and one in yarn-site.xml, as follows (these files are typically found in /etc/hadoop/conf):

(in mapred-site.xml)

	<name>mapreduce.map.memory.mb</name>
	<value>2048</value>
	<name>mapreduce.reduce.memory.mb</name>
	<value>2048</value>
	<name>mapreduce.map.java.opts</name>
	<value>-Xmx1229</value>
	<name>mapreduce.reduce.java.opts</name>
	<value>-Xmx1229m</value>

(in yarn-site.xml)

	<name>yarn.nodemanager.resource.memory-mb</name>
	<value>3198</value>

If you are using a cluster manager such as Cloudera Manager or Ambari, these settings must usually be modified using the Web interface.

## Hadoop Security with Kerberos Authentication


By default, most Hadoop configurations are relatively insecure. Security features such as SELinux and IPtables firewalls are often turned off to help get the Hadoop cluster up and running quickly. However, Cloudera and Hortonworks distributions of Hadoop support Kerberos authentication, which allows Hadoop to operate in a much more secure manner. To use Kerberos authentication with your particular version of Hadoop, see one of the following documents:

- [Cloudera CDH5](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CDH5/latest/CDH5-Security-Guide/CDH5-Security-Guide.html)
- [Cloudera CDH5 with Cloudera Manager 5](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CM5/latest/Configuring-Hadoop-Security-with-Cloudera-Manager/cm5chs_using_cm_sec_config.html)
- [Hortonworks HDP 1.3](http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.1/bk_installing_manually_book/content/rpm-chap14.html)
- [Hortonworks HDP 2.x](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.2/bk_installing_manually_book/content/rpm-chap14.html)
- [Hortonworks HDP (1.3 or 2.x) with Ambari](http://docs.hortonworks.com/HDPDocuments/Ambari-2.1.2.0/bk_Ambari_Security_Guide/content/ch_amb_sec_guide.html)

If you have trouble restarting your Hadoop cluster after enabling Kerberos authentication, the problem is most likely with your keytab files. Be sure you have created all the required Kerberos principals and generated appropriate keytab entries for all of your nodes, and that the keytab files have been located correctly with the appropriate permissions. (We have found that in Hortonworks clusters managed with Ambari, it is important that the spnego.service.keytab file be present on *all* the nodes of the cluster, not just the name node and secondary namenode.)

The MapR distribution also supports Kerberos authentication, but most MapR installations use that distribution’s *wire-level security* feature. See the [MapR Security Guide](http://doc.mapr.com/display/MapR/Security+Guide) for details.

## Installing Microsoft R Server on a Cluster

It is highly recommended that you install Microsoft R Server as root on each node of your Hadoop cluster. This ensures that all users will have access to it by default. Non-root installs are supported, but require that the path to the R executable files be added to each user’s path.

If you are installing on a Cloudera Manager system using a parcel install, skip to Section 3.6, Installing on a Cloudera Manager System Using a Cloudera Manager Parcel.

### Standard Command Line Install

For most users, installing on the cluster means simply running the standard Microsoft R Server installers on each node of the cluster:

1.  Log in as root or a user with sudo privileges. If the latter, precede commands requiring root privileges with sudo.
2.  Make sure the system repositories are up to date prior to installing Microsoft R Open for Microsoft R Server:

	*sudo yum clean all*

3.  [Download the Microsoft R Open for Microsoft R Server rpm](http://go.microsoft.com/fwlink/?LinkId=699383&clcid=0x409).
4.  Change to the directory to which you downloaded the rpm (for example, /tmp):

		cd /tmp
5.  Use the following command to install Microsoft R Open for Microsoft R Server:

		yum install MRO-for-MRS-8.0.0.*.x86_64.rpm
6.  Download and unpack the Microsoft R Server 2016 distribution, which will either be a DVD img file (if you obtained Microsoft R Server via Microsoft Volume Licensing) or a gzipped tar file (if you obtained Microsoft R Server via MSDN). The distribution file includes one or more Microsoft R Server installers, along with installers for DeployR, an optional additional component.
7.  If you have an img file, you must first mount the file. The following commands create a mount point and mount the file to that mount point:

		mkdir /mnt/mrsimage
		mount –o loop <filename> /mnt/mrsimage

	If you have a gzipped tar file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as /tmp):

		[for RHEL/CENTOS systems]
		tar zxvf MRS80RHEL.tar.gz

		[for SLES systems]
		tar zxvf MRS80SLES.tar.gz
8.  In either case, you will then want to copy the installer gzipped tar file to a writable directory, such as /tmp:

		[From the mounted img file]
		cp /mnt/mrsimage/Microsoft-R-Server-*.tar.gz /tmp

		[From the unpacked tar file]
		cp /tmp/MRS80*/Microsoft-R-Server-*.tar.gz /tmp

9.  Unpack and run the installer script, as follows (the tarball name may include an operating system ID denoted below by <OS>):

		cd /tmp
		tar xvzf Microsoft-R-Server-<OS>.tar.gz
		pushd rrent
		./install.sh –a –y -p /usr/lib64/MRO-for-MRS-8.0.0/R-3.2.2
		popd

This installs Microsoft R Server with the standard options (including loading the rpart and lattice packages by default when RevoScaleR is loaded).

### Distributed Installation

If you have multiple nodes, you can automate the installation across nodes using any distributed shell. (You can, of course, automate installation with a non-distributed shell such as bash using a for-loop over a list of hosts, but distributed shells usually provide the ability to run commands over multiple hosts simultaneously.) Examples include [dsh (“Dancer’s shell”)](http://www.netfort.gr.jp/~dancer/software/dsh.html.en), [pdsh (Parallel Distributed Shell)](http://sourceforge.net/projects/pdsh/), [PyDSH (the Python Distributed Shell)](http://pydsh.sourceforge.net/), and [fabric](http://www.fabfile.org/). Each distributed shell has its own methods for specifying hosts, authentication, and so on, but ultimately all that is required is the ability to run a shell command on multiple hosts. (It is convenient if there is a top-level copy command, such as the pdcp command that is part of pdsh, but not necessary—the “cp” command can always be run from the shell.)

Obtain the Microsoft R Open for Microsoft R Server rpm and the Microsoft R Server installer tar.gz file and copy all to /tmp as described in Section 3.1, steps 3 through 8.

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

### Installing the Microsoft R Server JAR File

Using Microsoft R Server in Hadoop requires the presence of the Microsoft R Server Java Archive (JAR) file scaleR-hadoop-0.1-SNAPSHOT.jar. This file is installed in the scripts directory of your Microsoft R Server installation (typically at /usr/lib64/MRS-8.0/scripts), and is typically linked to the standard Hadoop jar file location (typically $HADOOP\_HOME/lib or $HADOOP\_PREFIX/lib).

If you are installing RRE as a non-root user, you may need to obtain root access to link this file appropriately.

### Environment Variables for Hadoop

The file RevoHadoopEnvVars.site in the scripts directory of your Microsoft R Server installation (typically at /usr/lib64/MRS-8.0/scripts) should be sourced by all users, by adding the following line to the .bash\_profile file:

	. /usr/lib64/MRS-8.0/scripts/RevoHadoopEnvVars.site

(The period (“.”) at the beginning is part of the command, and must be included.)

This file sets the following environment variables for use by Microsoft R Server:

**HADOOP_HOME** This should be set to the directory containing the Hadoop files.

**HADOOP_CMD**: This should be set to the command used to invoke Hadoop

**HADOOP_CLASSPATH**: This should be set to include the full path to the RRE jar files (typically /usr/lib64/MRS-8.0/scripts).

**CLASSPATH**: This should be a fully expanded CLASSPATH with access to all required Hadoop JAR files.

**JAVA_LIBRARY_PATH:** If necessary, this should be set to include paths to the directories containing Hadoop jar files.

**HADOOP_STREAMING**: This should be set to the path of the Hadoop streaming jar file.

These environment variables are written to the file automatically on installation, but can be edited by hand if necessary.

### Creating Directories for Microsoft R Server


Each user should ensure that the appropriate user directories exist, and if necessary, create them with the following commands:

	hadoop fs -mkdir /user/RevoShare/$USER
	hadoop fs -chmod uog+rwx /user/RevoShare/$USER
	mkdir -p /var/RevoShare/$USER
	chmod uog+rwx /var/RevoShare/$USER

The HDFS directory can also be created in a user’s R session (provided the top-level /user/RevoShare has the appropriate permissions) using the following RevoScaleR commands (substitute your actual user name for “username”):

	rxHadoopMakeDir("/user/RevoShare/username")
	rxHadoopCommand("fs -chmod uog+rwx /user/RevoShare/username")

### Installing on a Cloudera Manager System Using a Cloudera Manager Parcel

If you are running a Cloudera Hadoop cluster managed by Cloudera Manager, and if Cloudera itself was installed via a Cloudera Manager parcel , you can use the Microsoft R Server Cloudera Manager parcels to install Microsoft R Server on all the nodes of your cluster. Two parcels are required:

- Microsoft R Open parcel—installs open-source R and additional open-source components on the nodes of your Cloudera cluster
- Microsoft R Server parcel—installs proprietary components on the nodes of your Cloudera cluster

Microsoft R Server requires several packages that may not be in a default Red Hat Enterprise Linux installation, run the following yum command as root to install them:

	yum install gcc-gfortran cairo-devel python-devel \\
		tk-devel libicu-devel

Run this command on all the nodes of your cluster that will be running Microsoft R Server. You can also use a distributed shell such as pdsh to distribute the command (here we use the pdshw alias we defined in section 3.2; this alias includes the list of host names and specifies the use of ssh):

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

		[From the mounted img file]
		cp /mnt/mrsimage/MRS-8.0.0-1-* /opt/cloudera/parcel-repo

		[From the unpacked tar file]
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

12.  Click **Activate**. Activation prepares Microsoft R Server to be used by the cluster.

When you have installed the parcels, download, install, and run the Revolution Custom Service Descriptor as follows:

1.  Copy the Custom Service Descriptor file MRS\_CONFIG-8.0.jar from either your mounted image file or your unpacked MRS80HADOOP directory to the Cloudera CSD directory, typically /opt/cloudera/csd.

2.  Stop and restart the cloudera-scm-server service using the following shell commands:

		service cloudera-scm-server stop
		service cloudera-scm-server start

3.  Confirm the CSD is installed by checking the Custom Service Descriptor list in Cloudera Manager at <hostname>/cmf/csd/list, where <hostname> is the host name of your Cloudera Manager server.

4.  On the Cloudera Manager home page, click the dropdown beside the cluster name and click Add a Service.

5.  From the Add Service Wizard, select Revolution R and click Continue.

6.  Select all hosts, and click Continue.

7.  Accept defaults through the remainder of the wizard.

Each user should ensure that the appropriate user directories exist, and if necessary, create them with the following shell commands:

	hadoop fs -mkdir /user/RevoShare/$USER
	hadoop fs -chmod uog+rwx /user/RevoShare/$USER
	mkdir -p /var/RevoShare/$USER
	chmod uog+rwx /var/RevoShare/$USER

The HDFS directory can also be created in a user’s R session (provided the top-level /user/RevoShare has the appropriate permissions) using the following RevoScaleR commands (substitute your actual user name for “username”):

	rxHadoopMakeDir("/user/RevoShare/username")
	rxHadoopCommand("fs -chmod uog+rwx /user/RevoShare/username")

As part of this process make sure to check that the base directories /user and /user/RevoShare have uog+rwx permissions as well.

## Verifying Installation

After completing installation, do the following to verify that Microsoft R Server will actually run commands in Hadoop:

1.  If the cluster is security-enabled, obtain a ticket using kinit (for Kerberos authentication) or mapr password (for MapR wire security).

2.  Start Microsoft R Server on a cluster node by typing Revo64 at a shell prompt.

3.  At the R prompt “> “, enter the following commands (these commands are drawn from the [*RevoScaleR Hadoop Getting Started Guide*](rserver-scaler-hadoop-getting-started.md)*,* which explains what all of them are doing. For now, we are just trying to see if everything works):

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

## Troubleshooting the Installation

No two Hadoop installations are exactly alike, but most are quite similar. This section brings together a number of common errors seen in attempting to run Microsoft R Server commands on Hadoop clusters, and the most likely causes of such errors from our experience.

### No Valid Credentials

If you see a message such as “No valid credentials provided”, this means you do not have a valid Kerberos ticket. Quit Microsoft R Server, obtain a Kerberos ticket using kinit, and then restart Microsoft R Server.

### Unable to Load Class RevoScaleR

If you see a message about being unable to find or load main class RevoScaleR, this means that the jar file scaleR-hadoop-0.1-SNAPSHOT.jar could not be found. This jar file must be in a location where it can be found by the getHadoopEnvVars.py script, or its location must be explicitly added to the CLASSPATH.

### Classpath Errors

If you see other errors related to Java classes, these are likely related to the settings of the following environment variables:

- PATH
- CLASSPATH
- JAVA\_LIBRARY\_PATH

Of these, the most commonly misconfigured is the CLASSPATH.

### Unable to Load Shared Library

If you see a message about being unable to load libhdfs.so, you may need to create a symbolic link from your installed version of libhdfs.so to the system library, such as the following:

	ln -s /path/to/libhdfs.so /usr/lib64/libhdfs.so

Or, update your LD\_LIBRARY\_PATH environment variable to include the path to the libjvm shared object:

	export LD\_LIBRARY\_PATH=$LD_LIBRARY_PATH:/path/to/libhdfs.so

(This step is normally performed automatically during the RRE install. If you continue to see errors about libhdfs.so, you may need to both create the symbolic link as above and set LD_LIBRARY_PATH.)

Similarly, if you see a message about being unable to load libjvm.so, you may need to create a symbolic link from your installed version of libjvm.so to the system library, such as the following:

	ln -s /path/to/libjvm.so /usr/lib64/libjvm.so

Or, update your LD\_LIBRARY\_PATH environment variable to include the path to the libjvm shared object:

export LD\_LIBRARY\_PATH=$LD\_LIBRARY\_PATH:/path/to/libjvm.so

## Getting Started with Hadoop

To get started with Microsoft R Server on Hadoop, we recommend the [*RevoScaleR Hadoop Getting Started Guide*](rserver-scaler-hadoop-getting-started.md). This provides a tutorial introduction to using RevoScaleR with Hadoop.

## Using HDFS Caching

*HDFS caching*, more formally *centralized cache management in HDFS*, can greatly improve the performance of your Hadoop jobs by keeping frequently used data in memory. You enable HDFS caching on a path by path basis, first by creating a *pool* of cached paths, and then adding paths to the pool.

The HDFS command cacheadmin is used to perform these tasks. This command should be run by the hdfs user (the mapr user on MapR installations). The cacheadmin command has many subcommands; the Apache Software Foundation has [complete documentation](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/CentralizedCacheManagement.html). To get started, the addPool and addDirective commands will suffice.

For example, to specify HDFS caching for our /share/AirlineDemoSmall directory, we can first create a pool as follows:

	hdfs cacheadmin –addPool rrePool

You can then add the path to /share/AirlineDemoSmall to the pool with an addDirective command as follows:

	hdfs cacheadmin –addDirective –path /share/AirlineDemoSmall –pool rrePool

## Creating an R Package Parcel for Cloudera Manager

If you are using Cloudera Manager to manage your Cloudera Hadoop cluster, you can use the Microsoft R Server Parcel Generator to create a Cloudera Manager parcel containing additional R packages, and use the resulting parcel to distribute those packages across all the nodes of your cluster.

The Microsoft R Server Parcel Generator is a Python script that takes a library of R packages and creates a Cloudera Manager parcel that **excludes any base or recommended packages, or packages included with the standard Microsoft R Server distribution**. Make sure to consider any dependencies your packages might have and be sure to include those in your library. If you installed Microsoft R Server with Cloudera Manager parcels, you will find the Parcel Generator in the *Revo.home()/scripts* directory. (You may need to ensure that the script has execute permission using the chmod command, or you can call it as “python generate\_r\_parcel.py”.)

When you call the script, you must provide a name and a version number for the resulting parcel, together with the path to the library you would like to package. When choosing a name for your parcel, be sure to pick a name that is unique in your parcel repository (typically /opt/cloudera/parcel-repo). For example, to package the library /home/RevoUser/R/library, you might call the script as follows:

	generate\_r\_parcel.py –p "RevoUserPkgs" –v "0.1" –l /home/RevoUser/R/library

By default, the path to the library you package should be the same as the path to the library on the Hadoop cluster. You can specify a different destination using the –d flag:

	generate\_r\_parcel.py –p "RevoUserPkgs" –v "0.1" \
		-l /home/RevoUser/R/library –d /var/RevoShare/RevoUser/library

To distribute and activate your parcel perform the following steps:

1.  Copy or move your .parcel and .sha files to the parcel repository on your Cloudera cluster (typically, /opt/cloudera/parcel-repo)
2.  Ensure that the .parcel and .sha files are owned by root and have 755 permissions (that is, read, write, and execute permission for root, and read and execute permissions for group and others).
3.  In your browser, open Cloudera Manager.
4.  Click **Hosts** in the upper navigation bar to bring up the All Hosts page.
5.  Click **Parcels** to bring up the Parcels page.
6.  Click **Check for New Parcels**. Your new parcel should appear with a **Distribute** button. After clicking Check for New Parcels you may need to click on “All Clusters” under the “Location” section on the left to see the new parcel.
7.  Click the **Distribute** button for your parcel. The parcel will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.
8.  Click **Activate**. Activation prepares your parcel to be used by the cluster.

After your parcel is distributed and activated your R packages should be present in the libraries on each node and can be loaded into your next R session.