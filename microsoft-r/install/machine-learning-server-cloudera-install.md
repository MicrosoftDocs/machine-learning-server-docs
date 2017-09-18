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

Cloudera offers a parcel installation methodology for adding services and features to a cluster. This article explains how to generate, deploy, and activate an installation parcel for Machine Learning Server 9.2.1 on a Cloudera distribution of Apache Hadoop (CDH). 

On a Hadoop cluster, Machine Learning Server is installed on all data nodes. You can use a generated parcel to distribute Machine Learning Server packages across nodes within your CDH cluster.

## Requirements

The Machine Learning Server parcel installation script can be generated on any [supported version of Linux](r-server-install-supported-platforms.md), but must execute on RHEL or CentOS 7.0 as the native operating system. 

We no longer use installation scripts that divide distribution download and unpacking into separate tasks. To get the files required for script generation, [install Machine Learning Server for Linux](machine-learning-server-linux-install.md) on any node or Linux box. You can install the developer edition to avoid using up an enterprise license on a computer outside your cluster.

If your operating system is not 7.0, or if you want to add [operationalization features](../operationalize/concept-operationalize-deploy-consume.md) on edge nodes, use the regular [Hadoop installation instuctions](machine-learning-server-hadoop-install.md) instead.

## Preparation

+ Machine Learning Server 9.2.1 for Hadoop, downloaded to a writable directory such as /tmp/, and unpacked.

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