---
# required metadata
title: "Install Machine Learning Server for Linux"
description: "How to install, connect to, and use Machine Learning Server on computers running a Linux operating system."
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

# Install Machine Learning Server for Linux


Machine Learning Server for Linux runs machine learning and data mining solutions written in R in standalone and clustered topologies. 

This article explains how to install Machine Learning Server 9.2.1 on a standalone Linux server that has an internet connection. If your server has restrictions on internet access, see [offline installation](machine-learning-server-linux-offline.md). 

> [!Note]
> Although you can add Python support during Setup, script that calls functions from Python libraries must execute on [SQL Server 2017 Machine Learning Server with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-service), or [Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md) in a Spark compute context. On Linux, you can run a web service that contains Python script, but web service execution is the only methodology we offer for Python on Linux in the 9.2.1 release. Additional capability for Python sessions and direct execution on Machine Learning Server for Linux is coming in subsequent releases.

## System requirements

+ Operating system must be a [supported version of Linux](r-server-install-supported-platforms.md) on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

+ Memory must be a minimum of 2 GB of RAM is required; 8 GB or more are recommended.

+ Disk space must be a minimum of 500 MB.

+ An internet connection. If you do not have an internet connection, for the instructions for an [offline installation](machine-learning-linux-offline.md).

+ A package manager (yum for CentOS/RHEL systems, apt for Ubuntu, dpkg for Ubuntu offline, zypper for SLES systems, )

+ Root or super user permissions

The following additional components are included in Setup and required for full language support in Machine Learning Server.

* Microsoft R Open 3.4.1  
* Microsoft .NET Core 1.1 
* Anaconda 4.2 with Python 3.5

## Running setup on existing installations

The installation path for Machine Learning Server is new: /opt/microsoft/mlserver/${VERSION}. However, if R Server 9.x is present, Machine Learning Server 9.2.1 finds R Server at the old path (/usr/lib64/microsoft-r/#{VERSION}) and replaces it with the new version. 

There is no support for side-by-side installations of older and newer versions, nor is there support for hybrid versions (such as R Server 9.1 and Python 9.2.1). An installation is either entirely 9.2.1 or an earlier version.

## Installation paths

Installed software can be found at the following paths:

+ Install root: /opt/microsoft/mlserver/9.2.1

+ Microsoft R Open root: /opt/microsoft/ropen/3.4.1

## How to install

Setup downloads packages from [packages.microsoft.com](https://packages.microsoft.com) for Linux installation.

> [!Tip]
> You can get Linux virtual machines in Azure if you want to use computing resources in the cloud: [Ubuntu virtual machine on Azure](https://docs.microsoft.com/sql/linux/quickstart-install-connect-ubuntu)

### Ubuntu 14.04 - 16.04

1. Enter `sudo su` to install as root.

2. Set the location of the packages using this example syntax for 16.04. For more examples, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

    ```
     wget http://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
     dpkg -i packages-microsoft-prod.deb
    ```

3. Update packages on your system: `apt-get update` 

4. Optionally, if your system does not have the `https apt transport` option: `apt-get install apt-transport-https`

5. Install the server: `apt-get install microsoft-mlserver-all-9.2.1`  

6. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`     

> [!Note]
> The configuration files for Machine Learning Server can be found at `/etc/apt/sources.list.d/mlserver.list`

### Red Hat and CentOS 6/7

1. Enter `sudo su` to install as root.

2. Set the location of the packages using this example syntax for 7.0. For more examples, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

   ```rpm -Uvh http://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm```

3. Install the server: `yum install microsoft-mlserver-all-9.2.1` 

4. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`

> [!Note]
> The configuration files for Machine Learning Server can be found at `/etc/apt/sources.list.d/mlserver.list`

### SUSE SLES11

1. Enter `sudo su` to install as root.

2. Set the location of the packages using this example syntax for SLES11. For more examples, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

   ```rpm -Uvh http://packages.microsoft.com/config/sles/11/packages-microsoft-prod.rpm```

3. Install the server: `zypper install microsoft-mlserver-all-9.2.1` 

4. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`

> [!Note]
> The configuration files for Machine Learning Server can be found at `/etc/zypp/repos.d/mlserver.repo`

## Next steps

## See also