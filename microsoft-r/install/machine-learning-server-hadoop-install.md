---
# required metadata
title: "Install Machine Learning Server for Hadoop"
description: "How to install, connect to, and use Machine Learning Server on a Hadoop cluster with Spark or MapReduce"
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

# Install Machine Learning Server for Hadoop

On a Hadoop cluster, Machine Learning Server must be installed on the edge node and all data nodes on a commercial distribution of Hadoop: Cloudera, HortonWorks, MapR. Optionally, you can install [operationalization features](../operationalize/concept-operationalize-deploy-consume.md) on edge nodes only.

Machine Learning Server is engineered for the following architecture:

+ Hadoop Distributed File System (HDFS)
+ Apache YARN
+ MapReduce or Spark 2.0-2.4

## System and setup requirements

+ Native operating system must be a [supported version of Hadoop on 64-bit Linux](r-server-install-supported-platforms.md).

+ Minimum RAM is 8 GB (16 GB or more is recommended). Minimum disk space is 500 MB per node.

+ An internet connection. If you do not have an internet connection, use the [offline installation instructions](machine-learning-server-linux-offline.md).

+ Root or super user permissions

<a name="package-manager"></a>

## Package managers

Installation is through package managers. Unlike previous releases, there is no install.sh script. 

| Package manager | Platform |
|-----------------|----------|
|[yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html) | RHEL, CentOS|
|[apt](https://help.ubuntu.com/lts/serverguide/apt.html) | Ubuntu onlne |
| [dpkg](https://help.ubuntu.com/lts/serverguide/dpkg.html) | Ubuntu offline |
|[zypper](https://www.suse.com/documentation/opensuse111/opensuse111_reference/data/sec_zypper.html) | SUSE |
|[rpm](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-rpm-using.html) | RHEL, CentOS, SUSE |

## Running setup on existing installations

The installation path for Machine Learning Server is new: `/opt/microsoft/mlserver/9.2.1`. However, if R Server 9.x is present, Machine Learning Server 9.2.1 finds R Server at the old path (`/usr/lib64/microsoft-r/9.1.0`) and replaces it with the new version. 

There is no support for side-by-side installations of older and newer versions, nor is there support for hybrid versions (such as R Server 9.1 and Python 9.2.1). An installation is either entirely 9.2.1 or an earlier version.

## Installation paths

After installation completes, software can be found at the following paths:

+ Install root: `/opt/microsoft/mlserver/9.2.1`
+ Microsoft R Open root: `/opt/microsoft/ropen/3.4.1`
+ Executables such as Revo64 and mlserver-python are at `/usr/bin`

## Edge node installation

Machine Learning Server is required on the edge node. You should run [full setup](machine-learning-server-linux-install.md) to install on this node.

Full setup gives you core components for both R and Python, machine learning algorthims and pretrained models, and [operationalization](../operationalize/concept-operationalize-deploy-consume.md). Operationalization features run on edge nodes, enabling additional ways of deploying and consuming script. For example, you can build and deploy web services providing compiled code, or running script in real time. 

> [!Note]
> You cannot use operationalization on data nodes. Operationalization does not support Yarn queues and cannot run in a distributed manner.

## Data node installation

You can start on any data node, installing sequentially, or install on multiple data nodes concurrently. There are two approaches for installing Machine Learning Server on data nodes. 

**Approach 1: Package managers for full installation** 

We recommend running the [full setup package](machine-learning-server-linux-install.md) on every node. This approach is fast because package managers do most of the work, including adding the Hadoop package (microsoft-mlserver-hadoop-9.2.1) and setting it up for activation.

**Approach 2: Manual steps for partial installation**

Alternatively, you can install a subset of packages. You might do this if you do not want operationalization on your data nodes. Be prepared for more testing if you choose this approach. The packages are not specifically designed to run as standalone modules. Hence, unexpected problems are more likely if you leave some packages out. 

1. Install as root: `sudo su`

2. Refer to the [annotated package list](#package-list) and download individual packages from the package repo corresponding to your platform:

   + [https://packages.microsoft.com/ubuntu/14.04/prod/](https://packages.microsoft.com/ubuntu/14.04/prod/)
   + [https://packages.microsoft.com/ubuntu/16.04/prod/](https://packages.microsoft.com/ubuntu/16.04/prod/)
   + [https://packages.microsoft.com/rhel/7.3/prod/](https://packages.microsoft.com/rhel/7.3/prod/)
   + [https://packages.microsoft.com/sles/11/prod/](https://packages.microsoft.com/sles/11/prod/)

3. Make a directory to contain your packages: `hadoop fs -mkdir /tmp/mlsdatanode`
 
4. Copy the packages: `hadoop fs -copyFromLocal /tmp/mlserver /tmp/mlsdatanode`

5. Switch to the directory: `cd /tmp/mlsdatanode`

6. Install the packages using the tool and syntax for your platform:

    + On Ubunutu online: `apt-get install *.rpm`
    + On Ubunutu offline: `dpkg -I *.deb`
    + On CentOS and RHEL: `yum install *.rpm` 

7. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`

Repeat this procedure on remaining nodes.

<a name="package-list"></a>

## Packages list 

The following packages comprise a full Machine Learning Server installation:

```
 microsoft-mlserver-packages-r-9.2.1        ** core
 microsoft-mlserver-python-9.2.1            ** core
 microsoft-mlserver-packages-py-9.2.1       ** core
 microsoft-mlserver-hadoop-9.2.1            ** hadoop (required for hadoop)
 microsoft-mlserver-mml-r-9.2.1             ** microsoftml for R (optional)
 microsoft-mlserver-mml-py-9.2.1            ** microsoftml for Python (optional)
 microsoft-mlserver-mlm-r-9.2.1             ** pre-trained models (requires mml)
 microsoft-mlserver-mlm-py-9.2.1            ** pre-trained models (requires mml)
 microsoft-mlserver-adminutil-9.2           ** operationalization (optional)
 microsoft-mlserver-computenode-9.2         ** operationalization (optional)
 microsoft-mlserver-config-rserve-9.2       ** operationalization (optional) 
 microsoft-mlserver-dotnet-9.2              ** operationalization (optional)
 microsoft-mlserver-webnode-9.2             ** operationalization (optional)
```
The microsoft-mlserver-python-9.2.1 package provides Anaconda 4.2 with Python 3.5, executing as mlserver-python, found in `/opt/microsoft/mlserver/9.2.1/bin/python/python`

Microsoft R Open is required for R execution:

```
 microsoft-r-open-foreachiterators-3.4.1 
 microsoft-r-open-mkl-3.4.1
 microsoft-r-open-mro-3.4.1 
```

Microsoft .NET Core 1.1, used for operationalization, must be added to Ubuntu:

```
 dotnet-host
 dotnet-hostfxr-1.1.0
 dotnet-sharedframework-microsoft.netcore.app-1.1.2 
```

Additional open source packages could be required. The potential list of packages varies for each computer. Refer to [offline installation](machine-learning-server-linux-offline.md) for an example list.

## Next steps

We recommend starting with [How to use RevoScaleR with Spark](../r/how-to-revoscaler-spark.md) or [How to use RevoScaleR with Hadoop MapReduce](../r/how-to-revoscaler-hadoop.md). 

For a list of functions that utilize Yarn and Hadoop infrastructure to process in parallel across the cluster, see [Distributed computing > Function list](../r/how-to-revoscaler-distributed-computing.md#distributed-computing-overview).

## See also

+ [Install on Linux](machine-learning-server-linux-install.md)
+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)

