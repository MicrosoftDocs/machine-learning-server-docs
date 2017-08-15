---

# required metadata
title: "Hadoop installation and configuration for Microsoft R Server"
description: "Hadoop installation and configuration guide for Microsoft R Server"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "04/10/2017"
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

# Hadoop installation and configuration for Microsoft R Server

Microsoft R Server is a scalable data analytics server that can be deployed as a single-user workstation, a local network of connected servers, or on a Hadoop cluster in the cloud. On Hadoop, R Server requires MapReduce, Hadoop Distributed File System (HDFS), and Apache YARN. Optionally, Spark version 1.6-2.0 is supported for Microsoft R Server 9.1.

### Platforms and Dependencies

- [Supported operating systems for Microsoft R Server](r-server-install-supported-platforms.md)
- [Package dependencies for Microsoft R Server installations on Linux and Hadoop](r-server-install-linux-hadoop-packages.md)

### Step-by-Step

- [Command line installation for any supported platform](r-server-install-hadoop-command-line.md)
- [Install an R package parcel using Cloudera Manager](r-server-install-cloudera.md)
- [Offline installation](r-server-install-hadoop-offline.md)
- [Manual package installation](r-server-install-hadoop-manual-package.md)
- [Configure R Server to operationalize R code and host analytic web services](operationalize-r-server-one-box-config.md)
- [Uninstall Microsoft R to upgrade to newer versions](r-server-install-uninstall-upgrade.md)

### Configuration

- [Adjust your Hadoop cluster configuration for R Server workloads](r-server-install-hadoop-configuration-r-workloads.md)
- [Enforcing YARN queue usage on R Server for Hadoop](r-server-install-hadoop-yarnqueueusage.md)
- [Manage your R installation on Linux](r-server-install-linux-manage-install.md)

### Start R workloads

- [Practice data import and exploration and Hadoop](../r/how-to-revoscaler-hadoop.md)
- [Practice data import and exploration and Spark](../r/how-to-revoscaler-spark.md)
