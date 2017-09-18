---
# required metadata
title: "Install Machine Learning Server on Cloudera"
description: "How to install, connect to, and use Machine Learning Server on a Cloudera Hadoop disribution."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/15/2017"
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

# Install Machine Learning Server using Cloudera Manager

Cloudera offers a parcel installation methodology for adding services and features to a cluster. On a Hadoop cluster, Machine Learning Server is installed on all data nodes. You can use a generated parcel to distribute Machine Learning Server packages across nodes within your CDH cluster.

This article explains how to generate, deploy, and activate an installation parcel for Machine Learning Server 9.2.1 on a Cloudera distribution of Apache Hadoop (CDH). 

If your operating system is not 7.0, or if you want to add [operationalization features](../operationalize/concept-operationalize-deploy-consume.md) on edge nodes, use the regular [Hadoop installation instuctions](machine-learning-server-hadoop-install.md) instead.

## Download a distribution

A package manager installation used for Linux or Hadoop won't provide the parcel generation scripts. To get the scripts, obtain a gzipped distribution of Machine Learning Server from [Visual Studio Subscriptions](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) or [Volume licensing](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409):

+ Search for "SQL Server 2017" to list features licenced through SQL Server. For licensing purposes, Machine Learning Server is an add-on feature to SQL Server.
+ Download **Machine Learning Server 9.2.1 for Hadoop** to a writable directory, such as **/tmp**.

## Unpack the distribution

1. On the master node, log in as root or a user with super user privileges (`sudo su`). In our examples, the master node is a machine named `cdh4-mn0`.
2. Switch to the **/tmp** directory (assuming it's the download location): `cd /tmp`
3. Unpack the file:
        `[root@cdh4-mn0 tmp] $ tar zxvf en_microsoft_r_server_910_for_hadoop_x64_10323951.tar.gz`

The distribution is unpacked into an `MRS91Hadoop` folder at the download location. The distribution includes the following files:

| File | Description |
|------|-------------|
|`install.sh` | Script for installing Machine Learning Server. Do not use this for a parcel install. |
|`generate_mrs_parcel.sh` | Script for generating a parcel used for installing Machine Learning Server on CDH. |
| `EULA.txt` | End user license agreements for each separately licensed component. |
| DEB folder | Contains Machine Learning packages for deployment on Ubuntu. |
| RPM folder | Contains Machine Learning packages for deployment on CentOS/RHEL and SUSE |
| Parcel folder | Contains files used to generate a parcel for installation on CDH. |





+ Parcel name is `MLServer-9.2.1` (used to be MRS-9.2.1)
+ Parcel generator is `generate_mlserver_parcel.sh`
+ The CSD is also called `MLServer`

Python is a part of the Parcel installer, controlled by the command line option, `-l` or `--add-mml`.

## Parcel generation

Create the parcel on a computer that has an installation of Machine Learning Server for Linux.

1. Log on as root: `sudo su`
2. Go to the installation folder: `cd /opt/microsoft/mlserver/9.2.1/libraries/common/hadoop`
3. List the contents: `ls` to confirm the following files exist:

  ```compute  create-dirs.sh  discover-java.sh  getHadoopEnvVars.py  jar  mrs-clean-hadoop-workspace  mrs-hadoop  mrs-spark-submit  templates  utils  workflow``

4. Do a dry run of the parcel generator to `bash generate_mlserver_parcel.sh -n`

## Parcel execution

## Rollback (optional)

## Next steps

We recommend starting with [How to use RevoScaleR with Spark](../r/how-to-revoscaler-spark.md) or [How to use RevoScaleR with Hadoop MapReduce](../r/how-to-revoscaler-hadoop.md). 

For a list of functions that utilize Yarn and Hadoop infrastructure to process in parallel across the cluster, see [Distributed computing > Function list](../r/how-to-revoscaler-distributed-computing.md#distributed-computing-overview).

## See also

+ [Install on Linux](machine-learning-server-linux-install.md)
+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)

[Install R Server 9.1 on the Cloudera distribution](r-server-install-cloudera.md)