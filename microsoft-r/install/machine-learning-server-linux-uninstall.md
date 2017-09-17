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

This article explains how to uninstall Machine Learning Server 9.2.1 for Linux. 

> [!Note]
> To upgrade from either a 9.0.1 or 9.1 installation, you can skip uninstall. Setup automatically replaces previous versions of R Server or Microsoft R Open 3.3.2 with newer versions. However, if you have 8.x, you must uninstall first.

## Collect information

Uninstall reverses the installation steps, including uninstalling any package dependencies used only by the server. Start by collecting information about your installation.

1. List the packages from Microsoft.

  + On RHEL: `yum list \*microsoft\*`   
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SUSE: `zypper search \*microsoft-r\*`    


2. Get package version information. On a 9.2.1 installation, you should see about [16 packages](#installed-packages). Since multiple major versions can coexist, the package list could be much longer. Given a list of packages, you can get verbose version information for particular packages in the list. The following examples are for Microsoft R Open version 3.4.1:

  + On RHEL: `rpm -qi microsoft-r-open-mro-3.4.1`   
  + On Ubuntu: `dpkg --status microsoft-r-open-mro-3.4.1` 
  + On SUSE: `zypper info microsoft-r-open-mro-3.4.1`     

## How to uninstall 9.2.1

1. On root@, uninstall Microsoft R Open (MRO) first. This action removes any dependent packages used only by MRO, which includes packages like microsoft-mlserver-packages-r. 

  + On RHEL: `yum erase microsoft-r-open-mro-3.4.1`     
  + On Ubuntu: `apt-get purge microsoft-r-open-mro-3.4.1`  
  + On SUSE: `zypper remove microsoft-r-open-mro-3.4.1`    

2. Remove the Machine Learning Server Python packages:

  + On RHEL: `yum erase microsoft-mlserver-python-9.2.1`     
  + On Ubuntu: `apt-get purge microsoft-mlserver-python-9.2.1`  
  + On SUSE: `zypper remove microsoft-mlserver-python-9.2.1`

3. Remove the Hadoop package:

  + On RHEL: `yum erase microsoft-mlserver-hadoop-9.2.1`     
  + On Ubuntu: `apt-get purge microsoft-mlserver-hadoop-9.2.1`  
  + On SUSE: `zypper remove microsoft-mlserver-hadoop-9.2.1`

4. You have additional packages if you installed the operationalization feature. On a 9.2.1 installation, this is the azureml-model-management library, which you can uninstall using the syntax from the previous step. Multiple packages provide the feature. Uninstall each one in the following order:

  + microsoft-mlserver-adminutil-9.2
  + microsoft-mlserver-webnode-9.2
  + microsoft-mlserver-computenode-9.2

5. Re-list the packages from Microsoft to check for remaining files:

  + On RHEL: `yum list \*microsoft\*`   
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SUSE: `zypper search \*microsoft-r\*`  

  On Ubuntu, you have `dotnet-sharedframework-microsoft.netcore.app-1.1.2`. [NET Core](https://docs.microsoft.com/dotnet/core/index) is a cross-platform, general purpose development platform maintained by Microsoft and the .NET community on GitHub. This package could be providing infrastructure to other applications on your computer. If Machine learning Server is the only Microsoft software you have, you can remove it now.

6. After packages are uninstalled, remove remaining files. On root@, determine whether additional files still exist:

  + `$ ls /opt/microsoft/mlserver/9.2.1/`

7. Remove the entire directory:

  + `$ rm -fr ls /opt/microsoft/mlserver/9.2.1/`

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft/mlserver. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

<a name="installed-packages"></a>

## Package list

Machine Learning Server for Linux adds the following packages at a minimum. When uninstalling software, refer to this list when searching for packages to remove.

    dotnet-host
    dotnet-hostfxr-1.1.0
    dotnet-sharedframework-microsoft.netcore.app-1.1.2 
    
    microsoft-mlserver-adminutil-9.2
    microsoft-mlserver-all-9.2.1 
    microsoft-mlserver-computenode-9.2
    microsoft-mlserver-config-rserve-9.2 
    microsoft-mlserver-hadoop-9.2.1
    microsoft-mlserver-mlm-py-9.2.1 
    microsoft-mlserver-mlm-r-9.2.1
    microsoft-mlserver-mml-py-9.2.1
    microsoft-mlserver-mml-r-9.2.1
    microsoft-mlserver-packages-py-9.2.1 
    microsoft-mlserver-packages-r-9.2.1
    microsoft-mlserver-python-9.2.1 
    microsoft-mlserver-webnode-9.2
    microsoft-r-open-foreachiterators-3.4.1 
    microsoft-r-open-mkl-3.4.1
    microsoft-r-open-mro-3.4.1 

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
