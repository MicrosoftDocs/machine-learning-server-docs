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
# Adjust Hadoop configuration for RevoScaleR workloads

## Increase memory limits for R server workloads

On YARN-based Hadoop systems (CDH5, HDP 2.x, MapR 4.x), we sometimes find that the default settings for Map and Reduce memory limits are inadequate for large RevoScaleR jobs.  

To allow large RevoScaleR jobs to run, you can modify four properties in mapred-site.xml and one in yarn-site.xml, typically found in /etc/hadoop/conf.

The memory available for R is the difference between the container’s memory limit and the memory given to the Java Virtual Machine.

**mapred-site.xml**

	<name>mapreduce.map.memory.mb</name>
	<value>2048</value>
	<name>mapreduce.reduce.memory.mb</name>
	<value>2048</value>
	<name>mapreduce.map.java.opts</name>
	<value>-Xmx1229</value>
	<name>mapreduce.reduce.java.opts</name>
	<value>-Xmx1229m</value>

**yarn-site.xml**

	<name>yarn.nodemanager.resource.memory-mb</name>
	<value>3198</value>

If you are using a cluster manager such as Cloudera Manager or Ambari, these settings must usually be modified using the Web interface.

## Use HDFS Caching

*HDFS caching*, more formally *centralized cache management in HDFS*, can greatly improve the performance of your Hadoop jobs by keeping frequently used data in memory. You enable HDFS caching on a path by path basis, first by creating a *pool* of cached paths, and then adding paths to the pool.

The HDFS command `cacheadmin` is used to perform these tasks. This command should be run by the hdfs user (the mapr user on MapR installations). The cacheadmin command has many subcommands; the Apache Software Foundation has [complete documentation](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/CentralizedCacheManagement.html). To get started, the addPool and addDirective commands will suffice.

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

