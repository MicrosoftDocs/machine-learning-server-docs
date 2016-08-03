---
# required metadata
title: "Uninstall R Server on Linux or Hadoop to upgrade to a newer version"
description: "Upgrading Microsoft R Server and the RevoScaleR package is achieved by uninstalling the existing version and installing a newer version."
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "08/02/2016"
ms.topic: ""
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
# Uninstall R Server to upgrade to a newer version

This article explains how to uninstall Microsoft R Server on either Linux or Hadoop. Upgrading to any new version of R Server, regardless of whether it's a major or minor release, requires that you first uninstall the existing deployment so that you can install the new distribution.

When you do reinstall R, we recommend [installing R Server 2016 (version 8.0.5)](rserver-install-hadoop-805.md) because it provides the newest features, including significant enhancements to the installers.

## Hadoop considerations for uninstall and reinstallation of R Server

You can uninstall and reinstall node by node across the cluster, but don’t try to submit any jobs until all nodes are at the same functional level.

If you are upgrading the Hadoop cluster, you must uninstall and reinstall Microsoft R Server on the cluster. You can either uninstall R Server before upgrading your cluster, or create and run a user-defined script that repairs the R Server links as a post-upgrade operation. The simpler approach is to uninstall R Server first, upgrade the cluster, and then reinstall R Server.

## Before you uninstall

Uninstall reverses the installation steps, including uninstalling any package dependencies used only by R Server, so the best place to start is by collecting some information about your installation:

- Version of R Server in the cluster
- Package location
- Which nodes in the cluster have R Server

## Program version and file locations

There are several approaches for identifying a program version, including `rpm -qi` as noted in the installation instructions.

- `rpm -qi microsoft-r-server-mro-8.0` (applies to Microsoft R Server 2016, build 8.0.5)
- `rpm -qi MRO-for-MRS-8.0.0` (applies to Microsoft R Server 2016, build 8.0.0)
- `rpm -qi RRO-8.0.3` (applies to Revolution R Server 7.4 and 8.0.3)

Version information is also evident in the file path, indicated in the examples below.

- `/usr/lib64/microsoft-r/8.0` (applies to Microsoft R Server 2016, build 8.0.5)
- `/usr/lib64/MRO-for-MRS` and `/usr/lib64/MRS-8.0` (applies to Microsoft R Server 2016, build 8.0.0)
- `/usr/lib64/RRO-8.0.3` and `usr/lib64/Revo-7.4` (applies to Revolution R Server 7.4 and 8.0.3)

You might see this path, given a Cloudera Manager parcel installation.

- `/opt/cloudera/parcel`

## How to uninstall 8.0.5

Packages are registered in a database that tracks all package installations in the cluster. To update the database, use a package manager to remove the package: **yum** for Red Hat and CentOS, or **zypper** for SUSE.

1. Uninstall the package (use one of the following):

    - `sudo yum erase microsoft-r-server-mro-8.0`
    - `sudo zypper rm microsoft-r-server-mro-8.0`

2. On the root node, verify the location of other files that need to be removed:
        `$ ls /usr/lib64/microsoft-r/8.0`

3. Remove the entire directory:
        `$ rm -fr /usr/lib64/microsoft-r`

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## How to uninstall 8.0.0

1. Erase Microsoft R Open:
    `yum erase MRO-for-MRS-8.0.0`
2. Remove directories of MRS and MRO, in this order:
    `rm -rf /usr/lib64/MRS-8.0`
    `rm -rf /usr/lib64/MRO-for-MRS-8.0.0`
3. Remove symlinks:
    `rm -f /usr/bin/Revo64 /usr/bin/Revoscript`

## How to uninstall 7.4

1. Run the uninstall script to remove Revolution R Enterprise and Revolution R Connector:
    `/usr/lib64/Revo-7.4/uninstall_revo.sh`
2. Erase Revolution R Open:
    `yum erase RRO-8.0.3`
3. Remove directories of RRE and RRO, in this order:
     `rm -rf /usr/lib64/Revo-7.4`
     `rm -rf /usr/lib64/RRO-8.0.3`

## How to uninstall individual packages

If you remove Microsoft R Open (microsoft-r-server-mro-8.0-8.0.5-1.x86_64), you will also remove any dependent packages used only by R Open.

Uninstall order is important. Due to package dependencies, be sure to remove the packages in the order given below.

**For RPM**

        $ rpm -e microsoft-r-server-hadoop-8.0-8.0.5-1.x86_64
        $ rpm -e microsoft-r-server-packages-8.0-8.0.5-1.x86_64
        $ rpm -e microsoft-r-server-intel-mkl-8.0-8.0.5-1.x86_64
        $ rpm -e microsoft-r-server-mro-8.0-8.0.5-1.x86_64

**For DEB**

    $ dpkg --remove microsoft-r-server-hadoop-8.0-8.0.5-1.x86_64
    $ dpkg --remove microsoft-r-server-packages-8.0-8.0.5-1.x86_64
    $ dpkg --remove microsoft-r-server-intel-mkl-8.0-8.0.5-1.x86_64
    $ dpkg --remove microsoft-r-server-mro-8.0-8.0.5-1.x86_64

## How to uninstall the Hadoop component

If you are uninstalling R Server 2016 (version 8.0.5), there is a Hadoop component that can be uninstalled independently of other packages in the distribution: `microsoft-r-server-hadoop-8.0-8.0.5-1.x86_64`

For versions prior to 8.0.5, do the following:

- Remove symlinks to libhfs.so and libjvm.so
- Remove scaleR JARs (or symlinks to those JARs) from HADOOP_HOME/lib

## See Also

[Install R on Hadoop overview](rserver-install-hadoop.md)
[Install R Server 2016 on Hadoop](rserver-install-hadoop-805.md)
[Install Microsoft R Server on Linux](rserver-install-linux-server.md)
[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)
