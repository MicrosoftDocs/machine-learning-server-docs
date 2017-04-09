---

# required metadata
title: "Manual package install on R Server for Linux or Hadoop"
description: "Install individual packages on R Server on Linux or Hadoop"
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

# Manual package installation (Microsoft R Server for Linux or Hadoop)

An alternative to running the install.sh script at the command line is manual installation of each package and component, or building a custom script that satisfies your technical or operational requirements.

Assuming that the packages for Microsoft R Open and Microsoft R Server are already unpacked, a manual or custom installation must create the appropriate folders and set permissions.

**RPM or DEB Installers**

- `/var/RevoShare/` and `hdfs://user/RevoShare` must exist and have folders for each user running Microsoft R Server in Hadoop or Spark.
- `/var/RevoShare/<user>` and `hdfs://user/RevoShare/<user>` must have full permissions (all read, write, and executive permissions for all users).

**Cloudera Parcel Installers**

1. Create `/var/RevoShare/` and `hdfs://user/RevoShare`. Parcels cannot create them for you.
2. Give `/var/RevoShare/<user>` and `hdfs://user/RevoShare/<user>` for each user.
3. Grant full permissions to both `/var/RevoShare/<user>` and `hdfs://user/RevoShare/<user>`.

## Install additional packages on each node using rxExec

Once you have R Server installed on a node, you can use the `rxExec` function in RevoScaleR to install additional packages, including third-party packages from CRAN or another repository. 

For example, to install the `SuppDists` package on all the nodes of your cluster, call `rxExec` as follows:

	rxExec(install.packages, "SuppDists")
	
