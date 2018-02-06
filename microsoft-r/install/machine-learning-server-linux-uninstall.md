---
# required metadata
title: "Uninstall Machine Learning Server for Linux "
description: "How to uninstall Machine Learning Server for Linux."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "02/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Uninstall Machine Learning Server for Linux

**Applies to:  Machine Learning Server 9.2.1 | 9.3**

This article explains how to uninstall Machine Learning Server for Linux. 

> [!Note]
> For upgrade purposes, uninstall is not necessary unless the existing version is 8.x. Upgrading from 9.x is an in-place upgrade that overwrites the previous version. Upgrading from 8.x requires that you uninstall R Server 8.x first. 

## Gather information

Uninstall reverses the installation steps, including uninstalling any package dependencies used only by the server. Start by collecting information about your installation.

1. List the packages from Microsoft.

  + On RHEL: `yum list \*microsoft\*`   
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SUSE: `zypper search \*microsoft-r\*`    


2. Get package version information. On a 9.3.0 installation, you should see about [16 packages](#installed-packages). Since multiple major versions can coexist, the package list could be much longer. Given a list of packages, you can get verbose version information for particular packages in the list. The following examples are for Microsoft R Open version 3.4.3:

  + On RHEL: `rpm -qi microsoft-r-open-mro-3.4.3`   
  + On Ubuntu: `dpkg --status microsoft-r-open-mro-3.4.3` 
  + On SUSE: `zypper info microsoft-r-open-mro-3.4.3`     

## Uninstall 9.3.0

1. On root@, uninstall Microsoft R Open (MRO) first. This action removes any dependent packages used only by MRO, which includes packages like microsoft-mlserver-packages-r. 

  + On RHEL: `yum erase microsoft-r-open-mro-3.4.3`     
  + On Ubuntu: `apt-get purge microsoft-r-open-mro-3.4.3`  
  + On SUSE: `zypper remove microsoft-r-open-mro-3.4.3`    

2. Remove the Machine Learning Server Python packages:

  + On RHEL: `yum erase microsoft-mlserver-python-9.3.0`     
  + On Ubuntu: `apt-get purge microsoft-mlserver-python-9.3.0`  
  + On SUSE: `zypper remove microsoft-mlserver-python-9.3.0`

3. Remove the Hadoop package:

  + On RHEL: `yum erase microsoft-mlserver-hadoop-9.3.0`     
  + On Ubuntu: `apt-get purge microsoft-mlserver-hadoop-9.3.0`  
  + On SUSE: `zypper remove microsoft-mlserver-hadoop-9.3.0`

4. You have additional packages if you installed the operationalization feature. On a 9.3.0 installation, this is the On a 9.3.0 installation, this is the [azureml-model-management library](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) or [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md), which you can uninstall using the syntax from the previous step. Multiple packages provide the feature. Uninstall each one in the following order:

  + microsoft-mlserver-adminutil-9.3
  + microsoft-mlserver-webnode-9.3
  + microsoft-mlserver-computenode-9.3

5. Re-list the packages from Microsoft to check for remaining files:

  + On RHEL: `yum list \*microsoft\*`   
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SUSE: `zypper search \*microsoft-r\*`  

  On Ubuntu, you have `ddotnet-runtime-2.0.0`. [NET Core](https://docs.microsoft.com/dotnet/core/index) is a cross-platform, general purpose development platform maintained by Microsoft and the .NET community on GitHub. This package could be providing infrastructure to other applications on your computer. If Machine learning Server is the only Microsoft software you have, you can remove it now.

6. After packages are uninstalled, remove remaining files. On root@, determine whether additional files still exist:

  + `$ ls /opt/microsoft/mlserver/9.3.0/`

7. Remove the entire directory:

  + `$ rm -fr ls /opt/microsoft/mlserver/9.3.0/`

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft/mlserver. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

<a name="installed-packages"></a>

## Package list

Machine Learning Server for Linux adds the following packages at a minimum. When uninstalling software, refer to this list when searching for packages to remove.

    dotnet-host-2.0.0
    dotnet-hostfxr-2.0.0
    dotnet-runtime-2.0.0 
    
    microsoft-mlserver-adminutil-9.3
    microsoft-mlserver-all-9.3.0 
    microsoft-mlserver-computenode-9.3
    microsoft-mlserver-config-rserve-9.3 
    microsoft-mlserver-hadoop-9.3.0
    microsoft-mlserver-mlm-py-9.3.0 
    microsoft-mlserver-mlm-r-9.3.0
    microsoft-mlserver-mml-py-9.3.0
    microsoft-mlserver-mml-r-9.3.0
    microsoft-mlserver-packages-py-9.3.0 
    microsoft-mlserver-packages-r-9.3.0
    microsoft-mlserver-python-9.3.0 
    microsoft-mlserver-webnode-9.3
    microsoft-r-open-foreachiterators-3.4.3 
    microsoft-r-open-mkl-3.4.3
    microsoft-r-open-mro-3.4.3 
    azure-cli-2.0.25-1.el7.x86_64     


## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
