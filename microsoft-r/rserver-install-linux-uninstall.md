---
# required metadata
title: "Uninstall R Server on Linux to upgrade to a newer version"
description: "Upgrade Microsoft R Server and the RevoScaleR package by uninstalling the existing version and installing a newer version."
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
# Uninstall R Server on Linux to upgrade to a newer version

This article explains how to uninstall Microsoft R Server on Linux. Unless you are upgrading from 9.0.1 to the [the latest version 9.1](rserver-install-linux-server.md), upgrade requires that you first uninstall the existing deployment before installing a new distribution.

For 9.0.1-to-9.1, the install script automatically removes previous versions of R Server or Microsoft R Open 3.3.2 if they are detected so that setup can install newer versions.

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

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

  + On RHEL: `yum erase microsoft-r-open-mro-3.3.x86_64`     
  + On Ubuntu: `apt-get purge microsoft-r-open-mro-3.3`  
  + On SUSE: `zypper remove microsoft-r-open-mro-3.3`    

2. Most packages are uninstalled, including Microsoft R Server. List the remaining packages to see what's left. On a 9.1. installation, you should see only those packages used for operationalizing R Server analytics. On a 9.0.1 install, you might see just mrsdeploy. Using the syntax from the previous step, uninstall remaining packages. For 9.1, uninstall packages in the following order:

  + Microsoft-r-server-adminutil-9.1.x86_64
  + Microsoft-r-server-webnode-9.1.x86_64
  + Microsoft-r-server-computenode-9.1.x86_64
  + Microsoft-r-server-config-rserve-9.1.x86_64

2. After packages are removed, you can remove remaining files. On the root node, verify the location of other files that need to be removed:

  + `$ ls /usr/lib64/microsoft-r`

3. Remove the entire directory:

  + `$ rm -fr /usr/lib64/microsoft-r`

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## How to uninstall 8.0.5

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

        yum erase microsoft-r-server-mro-3.0

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

 [Install R on Hadoop overview](install/r-server-install-hadoop.md)      
 [Install R Server 8.0.5 on Hadoop](install/r-server-install-hadoop-805.md)      
 [Install Microsoft R Server on Linux](rserver-install-linux-server.md) 
 [Troubleshoot R Server installation problems on Hadoop](install/r-server-install-hadoop-troubleshoot.md)
