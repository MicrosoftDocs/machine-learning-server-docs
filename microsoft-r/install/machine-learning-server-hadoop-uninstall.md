---
# required metadata
title: "Uninstall Machine Learning Server for Hadoop"
description: "How to uninstall Machine Learning Server on a Hadoop cluster."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "02/16/2018"
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

# Uninstall Machine Learning Server for Hadoop

**Applies to:  Machine Learning Server 9.2.1 | 9.3**

This article explains how to uninstall Machine Learning Server running in a Spark cluster. 

+ Remove components on edge nodes
+ Remove components on data nodes

You can uninstall existing software and upgrade to newer versions node by node across the cluster, but don’t try to submit any jobs until all nodes are at the same functional level.

## Uninstall edge nodes

1. List the packages from Microsoft.

 + On RHEL: `yum list \*microsoft\*`   
 + On Ubuntu: `apt list --installed | grep microsoft`  
 + On SUSE: `zypper search \*microsoft-r\*`    

2. Operationalization features run on edge nodes. On a 9.2.1 installation, this is the [azureml-model-management library](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) or [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md), which you can uninstall using the syntax from the previous step. Multiple packages provide the feature. Uninstall each one in the following order:

 **On CentOS and RHEL**

 + `yum erase microsoft-mlserver-adminutil-9.2`
 + `yum erase microsoft-mlserver-webnode-9.2`
 + `yum erase microsoft-mlserver-computenode-9.2`

 **On Ubunute**

 + `apt-get purge microsoft-mlserver-adminutil-9.2` 
 + `apt-get purge microsoft-mlserver-webnode-9.2` 
 + `apt-get purge microsoft-mlserver-computenode-9.2`  
 
 **On SUSE**

 + `zypper remove microsoft-mlserver-adminutil-9.2`   
 + `zypper remove microsoft-mlserver-webnode-9.2`   
 + `zypper remove microsoft-mlserver-computenode-9.2`   

## Uninstall data nodes

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

Machine Learning Server for Hadoop adds the following packages at a minimum. When uninstalling software, refer to this list when searching for packages to remove.

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