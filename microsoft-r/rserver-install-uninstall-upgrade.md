---
# required metadata
title: "Uninstall R Server on Hadoop to upgrade to a newer version"
description: "Upgrade Microsoft R Server yy uninstalling the existing version and installing a newer one."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/01/2017"
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
# Uninstall R Server on Hadoop to upgrade to a newer version

This article explains how to uninstall Microsoft R Server on Hadoop. Unless you are upgrading from 9.0.1 to the [the latest version 9.1](install/r-server-install-hadoop.md), upgrade requires that you first uninstall the existing deployment before installing a new distribution.

For 9.0.1-to-9.1, the install script automatically removes previous versions of R Server or Microsoft R Open 3.3.2 if they are detected so that setup can install newer versions.

## Considerations for uninstall and reinstallation of R Server

You can uninstall and reinstall node by node across the cluster, but don’t try to submit any jobs until all nodes are at the same functional level.

If you are upgrading the Hadoop cluster, you must uninstall and reinstall Microsoft R Server on the cluster. You can either uninstall R Server before upgrading your cluster, or create and run a user-defined script that repairs the R Server links as a post-upgrade operation. The simpler approach is to uninstall R Server first, upgrade the cluster, and then reinstall R Server.

## Before you uninstall

Uninstall reverses the installation steps, including uninstalling any package dependencies used only by R Server, so the best place to start is by collecting some information about your installation:

- Version of R Server in the cluster
- Package location
- Which nodes in the cluster have R Server

## Program version and file locations

As a first step, use your package manager to list the currently installed R Server packages. Typically, CentOS and Red Hat systems use **yum**, Ubuntu systems use **apt-get**, and SLES systems use **zypper**:

1. List the packages from Microsoft.

  + On RHEL: `yum list \*microsoft\*`   
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SUSE: `zypper search \*microsoft-r\*`    


2. On a 9.1 installation, you will see about 9 packages. Since multiple major versions can coexist, the package list could be much longer. Given a list of packages, you can get verbose version information for particular packages in the list. The following examples are for Microsoft R Open version 3.3.3:

  + On RHEL: `rpm -qi microsoft-r-open-mro-3.3.x86_64`   
  + On Ubuntu: `dpkg --status microsoft-r-open-mro-3.3.x86_64` 
  + On SUSE: `zypper info microsoft-r-open-mro-3.3.x86_64`     


If R Server was installed on Cloudera using parcel installation, program information looks like this:

- `/opt/cloudera/parcels/MRO-3.3.3` and `/opt/cloudera/parcels/MRS-9.0.1` (applies to 9.0.1)    
- `/opt/cloudera/parcels/MRO-3.3.2` and `/opt/cloudera/parcels/MRS-8.0.5` (applies to 8.0.5)    
- `/opt/cloudera/parcels/MRO-3.2.2-1` and `/opt/cloudera/parcels/MRS-8.0.0-1` (applies to 8.0)  

## General instructions for all versions


Packages are registered in a database that tracks all package installations in the cluster. To update the database, use a package manager to remove the package: **yum** for Red Hat and CentOS, **apt** for Ubuntu, or **zypper** for SUSE.

Log in as root or a user with `sudo` privileges. If you are using `sudo`, precede commands requiring root privileges with `sudo`.

The Revo64 program runs on demand so stopping and disabling the server is not required.  

## How to uninstall 9.x

1. On any node, uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO. Most packages are uninstalled with MRO, including Microsoft R Server. 

  + On RHEL: `yum erase microsoft-r-open-mro-3.3.x86_64`     
  + On Ubuntu: `apt-get purge microsoft-r-open-mro-3.3`  
  + On SUSE: `zypper remove microsoft-r-open-mro-3.3`    

2. On edge nodes only, you could have additional packages if you installed features for operationalizing analytics. On a 9.0.1 installation, this is the mrsdeploy package, which you can uninstall using the same syntax in the previous step. In the 9.1 release, multiple packages provide the feature. Uninstall each one in the following order:

  + Microsoft-r-server-adminutil-9.1.x86_64
  + Microsoft-r-server-webnode-9.1.x86_64
  + Microsoft-r-server-computenode-9.1.x86_64
  + Microsoft-r-server-config-rserve-9.1.x86_64

3. On all nodes, after packages are removed, you should now remove remaining files. On the root node, verify the location of other files that need to be removed:

  + `$ ls /usr/lib64/microsoft-r`

4. Remove the entire directory:

  + `$ rm -fr /usr/lib64/microsoft-r`

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

Repeat on each node in the cluster.

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

[Install R on Hadoop overview](install/r-server-install-hadoop.md)

[Install Microsoft R Server on Linux](rserver-install-linux-server.md)

[Troubleshoot R Server installation problems on Hadoop](install/r-server-install-hadoop-troubleshoot.md)
