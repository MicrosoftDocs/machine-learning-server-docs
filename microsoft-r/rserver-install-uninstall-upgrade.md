---
# required metadata
title: "Uninstall R Server on Linux or Hadoop to upgrade to a newer version"
description: "Upgrading Microsoft R Server and the RevoScaleR package is done by uninstalling the existing version and installing a newer version."
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/20/2016"
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

This article explains how to uninstall Microsoft R Server on Linux or Hadoop. Upgrading to any new version of R Server, regardless of whether it's a major or minor release, requires that you first uninstall the existing deployment so that you can install the new version.

Additionally, if you upgrade the Hadoop cluster, you will also need to uninstall and reinstall Microsoft R Server. You can either uninstall R Server before upgrading your cluster, or create and run a user-defined script that repairs the R Server links as a post-upgrade operation. The simpler approach is to uninstall R Server first, and then reinstall R Server after the cluster is upgraded.

You can uninstall and reinstall node by node across the cluster, but don’t try to submit any jobs until all nodes are at the same functional level.

When you do reinstall R, we recommend [installing R Server 2016 (version 8.0.5)](rserver-install-hadoop-805.md) because it provides the newest features, including significant enhancements to the installers.

## Before you uninstall

Uninstall reverses the installation steps, including uninstalling any package dependencies used only by R Server, so the best place to start is by collecting some information about your installation:

- Version of R Server in the cluster
- Package location
- Which nodes in the cluster have R Server

## Program version and file locations

There are several approaches for identifying a program version, including `rpm -qi` as noted in the installation instructions. However, version information is also evident in the file path, as the examples below demonstrate.

- `/usr/lib64/microsoft-r/8.0` is the program file location if you installed Microsoft R using the installer.
- `/log/cloudera/parcels/MRO-8.0.5/lib64/R` (a link to `/opt/cloudera/parcel`) if you installed using Cloudera Manager parcels.

Note: Pre-8.0 versions of R Server will have a path that includes the Revolution brand name (`/usr/lib64/revolution-r/7.0`).

## How to fully uninstall R from a node

Packages are registered in a database that tracks all package installations in the cluster. To update the database, use a package manager to remove the package: **yum** for Red Hat and CentOS, or **zypper** for SUSE.

1. Uninstall the package (use one of the following):
    sudo yum erase microsoft-r-server-mro-8.0
    sudo zypper rm microsoft-r-server-mro-8.0
2. Remove other files added by the installer. On the root node, verify the file location. You should see subfolders for hadoop, lib64, share, and stage:
        $ ls /usr/lib64/microsoft-r/8.0
3. Remove the entire directory:
        $ rm -fr /usr/lib64/microsoft-r

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## How to uninstall an individual package

If you remove Microsoft R Open (microsoft-r-server-mro-8.0-8.0.5-1.x86_64), you will also remove any dependent packages used only by R Open.

Uninstall order is important. Due to package dependencies, be sure to remove the packages in the order given below.

**For RPM**

        $ rpm -e microsoft-r-server-hadoop-8.0-8.0.5-1.x86_64
        $ rpm -e microsoft-r-server-packages-8.0-8.0.5-1.x86_64
        $ rpm -e microsoft-r-server-intel-mkl-8.0-8.0.5-1.x86_64
        $ rpm -e microsoft-r-server-mro-8.0-8.0.5-1.x86_64

**For RPM**

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
