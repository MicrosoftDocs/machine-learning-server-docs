---
# required metadata
title: "Uninstall Machine Learning Server for Linux | Microsoft Docs"
description: "How to uninstall Machine Learning Server for Linux."
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

# Uninstall Machine Learning Server for Linux

This article explains how to uninstall Machine Learning Server on Hadoop or Linux. 

For upgrades from 9.0.1-to-9.1, uninstall is unnecessary. Setup automatically replaces previous versions of R Server or Microsoft R Open 3.3.2 if they are detected so that setup can install newer versions.

## Collect information

Uninstall reverses the installation steps, including uninstalling any package dependencies used only by R Server. Start is by collecting information about your installation.

1. List the packages from Microsoft.

  + On RHEL: `yum list \*microsoft\*`   
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SUSE: `zypper search \*microsoft-r\*`    


2. Get package version information. On a 9.2.1 installation, you should see about 16 packages. Since multiple major versions can coexist, the package list could be much longer. Given a list of packages, you can get verbose version information for particular packages in the list. The following examples are for Microsoft R Open version 3.4.1:

  + On RHEL: `rpm -qi microsoft-r-open-mro-3.4.1`   
  + On Ubuntu: `dpkg --status microsoft-r-open-mro-3.4.1` 
  + On SUSE: `zypper info microsoft-r-open-mro-3.4.1`     

## How to uninstall 9.x

1. On any node, uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO. Most packages are uninstalled with MRO, including Microsoft R Server. 

  + On RHEL: `yum erase microsoft-r-open-mro-3.4.1`     
  + On Ubuntu: `apt-get purge microsoft-r-open-mro-3.4.1`  
  + On SUSE: `zypper remove microsoft-r-open-mro-3.4.1`    

2. On edge nodes only, you could have additional packages if you installed features for operationalizing analytics. On a 9.x installation, this is the mrsdeploy package, which you can uninstall using the same syntax in the previous step. In the 9.1 release, multiple packages provide the feature. Uninstall each one in the following order:

  + microsoft-mlserver-adminutil-9.2
  + microsoft-mlserver-computenode-9.2
  + microsoft-mlserver-config-rserve-9.2 
  + microsoft-mlserver-webnode-9.2

3. After packages are removed, remove remaining files. On root@, determine whether additional files still exist:

  + `$ ls /opt/microsoft/mlserver/9.2.1/`

4. Remove the entire directory:

  + `$ rm -fr ls /opt/microsoft/mlserver/9.2.1/`

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
