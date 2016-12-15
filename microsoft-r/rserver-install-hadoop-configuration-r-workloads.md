---
# required metadata
title: "Adjust your Hadoop cluster configuration for R server workloads"
description: "Adjust your Hadoop cluster configuration for R server workloads."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "06/14/2016"
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
# Adjust your Hadoop cluster configuration for R server workloads

For best results, check the configuration settings of your Hadoop cluster to make sure there is sufficient memory for loading and analyzing data across the cluster. This topic also explains how to leverage HDFS caching and Kerberos security. It also includes a terminology list that explains concepts and terms used in R Server documentation for Hadoop.

## Verifying the Hadoop Installation

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

## Adjusting Hadoop Memory Limits (Hadoop 2.x Systems Only)

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

## Use HDFS Caching

*HDFS caching*, more formally *centralized cache management in HDFS*, can greatly improve the performance of your Hadoop jobs by keeping frequently used data in memory. You enable HDFS caching on a path by path basis, first by creating a *pool* of cached paths, and then adding paths to the pool.

The HDFS command cacheadmin is used to perform these tasks. This command should be run by the hdfs user (the mapr user on MapR installations). The cacheadmin command has many subcommands; the Apache Software Foundation has [complete documentation](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/CentralizedCacheManagement.html). To get started, the addPool and addDirective commands will suffice.

For example, to specify HDFS caching for our /share/AirlineDemoSmall directory, we can first create a pool as follows:

	hdfs cacheadmin –addPool rrePool

You can then add the path to /share/AirlineDemoSmall to the pool with an addDirective command as follows:

	hdfs cacheadmin –addDirective –path /share/AirlineDemoSmall –pool rrePool

## Hadoop Security with Kerberos Authentication

By default, most Hadoop configurations are relatively insecure. Security features such as SELinux and IPtables firewalls are often turned off to help get the Hadoop cluster up and running quickly. However, Cloudera and Hortonworks distributions of Hadoop support Kerberos authentication, which allows Hadoop to operate in a much more secure manner. To use Kerberos authentication with your particular version of Hadoop, see one of the following documents:

- [Cloudera CDH5](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CDH5/latest/CDH5-Security-Guide/CDH5-Security-Guide.html)
- [Cloudera CDH5 with Cloudera Manager 5](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CM5/latest/Configuring-Hadoop-Security-with-Cloudera-Manager/cm5chs_using_cm_sec_config.html)
- [Hortonworks HDP 1.3](http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.1/bk_installing_manually_book/content/rpm-chap14.html)
- [Hortonworks HDP 2.x](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.2/bk_installing_manually_book/content/rpm-chap14.html)
- [Hortonworks HDP (1.3 or 2.x) with Ambari](http://docs.hortonworks.com/HDPDocuments/Ambari-2.1.2.0/bk_Ambari_Security_Guide/content/ch_amb_sec_guide.html)

If you have trouble restarting your Hadoop cluster after enabling Kerberos authentication, the problem is most likely with your keytab files. Be sure you have created all the required Kerberos principals and generated appropriate keytab entries for all of your nodes, and that the keytab files have been located correctly with the appropriate permissions. (We have found that in Hortonworks clusters managed with Ambari, it is important that the spnego.service.keytab file be present on *all* the nodes of the cluster, not just the name node and secondary namenode.)

The MapR distribution also supports Kerberos authentication, but most MapR installations use that distribution’s *wire-level security* feature. See the [MapR Security Guide](http://doc.mapr.com/display/MapR/Security+Guide) for details.

## Basic Hadoop Terminology

If you're not familiar with Hadoop, this terminology list can help you understand the recommendations and steps in Hadoop-related documentation for R Server. The following terms apply to computers and services within the Hadoop cluster, and define the roles of hosts within the cluster:

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
