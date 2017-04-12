---
# required metadata
title: "Uninstall R Server on Hadoop to upgrade to a newer version"
description: "Upgrade Microsoft R Server yy uninstalling the existing version and installing a newer one."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/08/2017"
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
# Uninstall R Server to upgrade to a newer version

This article explains how to uninstall Microsoft R Server on Hadoop. Upgrading to any new version of R Server, regardless of whether it's a major or minor release, requires that you first uninstall the existing deployment so that you can install the new distribution.

When you do reinstall R Server, we recommend the latest version of [R Server for Hadoop](rserver-install-hadoop.md).

## Considerations for uninstall and reinstallation of R Server

You can uninstall and reinstall node by node across the cluster, but don’t try to submit any jobs until all nodes are at the same functional level.

If you are upgrading the Hadoop cluster, you must uninstall and reinstall Microsoft R Server on the cluster. You can either uninstall R Server before upgrading your cluster, or create and run a user-defined script that repairs the R Server links as a post-upgrade operation. The simpler approach is to uninstall R Server first, upgrade the cluster, and then reinstall R Server.

## Before you uninstall

Uninstall reverses the installation steps, including uninstalling any package dependencies used only by R Server, so the best place to start is by collecting some information about your installation:

- Version of R Server in the cluster
- Package location
- Which nodes in the cluster have R Server

## Program version and file locations

There are several approaches for identifying a program version, including `rpm -qi` as noted in the installation instructions.

List installed packages and get package names:

- `yum list \*microsoft\*`

You might see a list like this:

- `microsoft-r-open-foreachiterators-3.3.x86_64`
- `microsoft-r-open-mkl-3.3.x86_64`
- `microsoft-r-open-mro-3.3.x86_64`

You can get verbose version information using this syntax:

- `rpm -qi <package-name>`

Alternatively, you might see these paths, given a Cloudera Manager parcel installation.

- `/opt/cloudera/parcels/MRO-8.0.5` and `/opt/cloudera/parcels/MRS-8.0.5` (applies to 8.0.5)
- `/opt/cloudera/parcels/MRO-3.2.2-1` and `/opt/cloudera/parcels/MRS-8.0.0-1` (applies to 8.0)

## General instructions for all versions

Packages are registered in a database that tracks all package installations in the cluster. To update the database, use a package manager to remove the package: **yum** for Red Hat and CentOS, or **zypper** for SUSE.

Log in as root or a user with super user privileges. If you are using `sudo`, precede commands requiring root privileges with `sudo` (for example, `sudo yum erase microsoft-r-open-mro-3.3.x86_64`).

## How to uninstall 9.x

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

        sudo yum erase microsoft-r-open-mro-3.3.x86_64

2. Repeat for Microsoft R Server.

3. On the root node, verify the location of other files that need to be removed. You might see 3.3 for MRO and 9.0.1 (or 9.1.0) for R Server:

        ls /usr/lib64/microsoft-r/

4. Remove the entire directory:

        rm -fr /usr/lib64/microsoft-r

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## How to uninstall 8.0.5

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

        yum erase microsoft-r-server-mro-8.0

2. On the root node, verify the location of other files that need to be removed: `

        ls /usr/lib64/microsoft-r/8.0

3. Remove the entire directory:

        rm -fr /usr/lib64/microsoft-r

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## How to uninstall 8.0.0

1. Uninstall the Microsoft R Open for Microsoft R Server (MRO-for-MRS) package:

        yum erase MRO-for-MRS-8.0.0

2. Remove directories of MRS and MRO, in this order:

        rm -rf /usr/lib64/MRS-8.0
        rm -rf /usr/lib64/MRO-for-MRS-8.0.0

3. Remove symlinks:

        rm -f /usr/bin/Revo64 /usr/bin/Revoscript

## How to uninstall individual packages

If you remove Microsoft R Open (microsoft-r-server-mro-8.0-8.0.5-1.x86_64), you will also remove any dependent packages used only by R Open.

Uninstall order is important. Due to package dependencies, be sure to remove the packages in the order given below.

        rpm -e microsoft-r-server-hadoop-8.0-8.0.5-1.x86_64
        rpm -e microsoft-r-server-packages-8.0-8.0.5-1.x86_64
        rpm -e microsoft-r-server-intel-mkl-8.0-8.0.5-1.x86_64
        rpm -e microsoft-r-server-mro-8.0-8.0.5-1.x86_64


## How to uninstall the Hadoop component

If you are uninstalling R Server 9.0.1 or 8.0.5, there is a Hadoop component that can be uninstalled independently of other packages in the distribution:

- `microsoft-r-server-hadoop-9.0.1.x86_64`
- `microsoft-r-server-hadoop-8.0-8.0.5-1.x86_64`

For versions prior to 8.0.5, do the following:

- Remove symlinks to libhfs.so and libjvm.so
- Remove scaleR JARs (or symlinks to those JARs) from HADOOP_HOME/lib

## See Also

[Install R on Hadoop overview](rserver-install-hadoop.md)

[Install Microsoft R Server on Linux](rserver-install-linux-server.md)

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)
