---

# required metadata
title: "Hadoop installation and configuration for Microsoft R Server"
description: "Hadoop installation and configuration guide for Microsoft R Server"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/10/2017"
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

# Hadoop installation and configuration for Microsoft R Server

Microsoft R Server is a scalable data analytics server that can be deployed as a single-user workstation, a local network of connected servers, or on a Hadoop cluster in the cloud. On Hadoop, R Server requires MapReduce, Hadoop Distributed File System (HDFS), and Apache YARN. Optionally, Spark version 1.6-2.0 is supported for Microsoft R Server 9.x.

### Platforms and Dependencies

- [Supported operating systems for Microsoft R Server](rserver-install-supported-platforms.md)
- [Package dependencies for Microsoft R Server installations on Linux and Hadoop](rserver-install-linux-hadoop-packages.md)

### Step-by-Step

- [Command line installation for any supported platform](rserver-install-hadoop-command-line.md)
- [Install an R package parcel using Cloudera Manager](rserver-install-cloudera.md)
- [Offline installation](rserver-install-hadoop-offline.md)
- [Manual package installation](rserver-install-hadoop-manual-package.md)
- [Configure R Server to operationalize R code and host analytic web services](install/operationalize-r-server-one-box-config.md)
- [Uninstall Microsoft R to upgrade to newer versions](rserver-install-uninstall-upgrade.md)

### Configuration

- [Adjust your Hadoop cluster configuration for R Server workloads](rserver-install-hadoop-configuration-r-workloads.md)
- [Enforcing YARN queue usage on R Server for Hadoop](rserver-install-hadoop-yarnqueueusage.md)
- [Manage your R installation on Linux](rserver-install-linux-manage-install.md)

### Start R workloads

- [Practice data import and exploration and Hadoop](scaler-hadoop-getting-started.md)
- [Practice data import and exploration and Spark](scaler-spark-getting-started.md)
