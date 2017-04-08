---
# required metadata
title: "Uninstall R Server on Linux to upgrade to a newer version"
description: "Upgrade Microsoft R Server and the RevoScaleR package by uninstalling the existing version and installing a newer version."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "08/09/2016"
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

This article explains how to uninstall Microsoft R Server on Linux. Upgrading to any new version of R Server, regardless of whether it's a major or minor release, requires that you first uninstall the existing deployment so that you can install the new distribution.

When you do reinstall R Server, we recommend [the latest version](rserver-install-linux-server.md) because it provides the newest features, including significant enhancements to the installers.

## Program version and file locations

There are several approaches for identifying a program version, including `rpm -qi` as noted in the installation instructions.

List installed packages and get package names:

- `yum list \*microsoft\*`

Get version information:

- `rpm -qi microsoft-r-open-mro-3.3.x86_64`
- `rpm -qi microsoft-r-server-packages-9.1.x86_64`


Alternatively, you might see these paths, given a Cloudera Manager parcel installation.

- `/opt/cloudera/parcels/MRO-8.0.5` and `/opt/cloudera/parcels/MRS-8.0.5` (applies to 8.0.5)
- `/opt/cloudera/parcels/MRO-3.2.2-1` and `/opt/cloudera/parcels/MRS-8.0.0-1` (applies to 8.0)


## General instructions for all versions

Packages are registered in a database that tracks all package installations in the cluster. To update the database, use a package manager to remove the package: **yum** for Red Hat and CentOS, or **zypper** for SUSE.

Log in as root or a user with `sudo` privileges. If you are using `sudo`, precede commands requiring root privileges with `sudo` (for example, `sudo yum erase microsoft-r-server-mro-8.0`).

## How to uninstall 9.0.1

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

        yum erase microsoft-r-server-mro--3.2.x86_64

2. On the root node, verify the location of other files that need to be removed: `

        ls /usr/lib64/microsoft-r/9.0

3. Remove the entire directory:

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

        rpm -e microsoft-r-server-packages-8.0-8.0.5-1.x86_64
        rpm -e microsoft-r-server-intel-mkl-8.0-8.0.5-1.x86_64
        rpm -e microsoft-r-server-mro-8.0-8.0.5-1.x86_64

## See Also

[Install R on Hadoop overview](rserver-install-hadoop.md)

[Install R Server 8.0.5 on Hadoop](rserver-install-hadoop-805.md)

[Install Microsoft R Server on Linux](rserver-install-linux-server.md)

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)
