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
> Python support is new in this release. Although you can add Python to Machine Learning Server for Windows, local script that calls our [proprietary Python functions](../python-reference/introducing-python-package-reference.md) must execute on [SQL Server 2017 Machine Learning Server with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-service), or on [Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md) in a Spark [remote compute context](../r/concept-what-is-compute-context.md). <br/><br/>On Windows, Python operations are limited to generic Python script, pushing  proprietary function calls to remote instances. You can also run Machine Learning Server [web services](../operationalize/concept-what-are-web-services.md) that contain  compiled Python script. More capability, including interactive Python sessions and direct execution of proprietary functions, is projected for future releases.

## System and setup requirements

+ Operating system must be a [supported version of Linux](r-server-install-supported-platforms.md) on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

+ Memory must be a minimum of 2 GB of RAM is required; 8 GB or more are recommended. Disk space must be a minimum of 500 MB.

+ An internet connection. If you do not have an internet connection, for the instructions for an [offline installation](machine-learning-server-linux-offline.md).

+ A package manager:

    + [yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html) for CentOS/RHEL systems
    + [apt](https://help.ubuntu.com/lts/serverguide/apt.html) for Ubuntu
    + [dpkg](https://help.ubuntu.com/lts/serverguide/dpkg.html) for Ubuntu offline
    + [zypper](https://www.suse.com/documentation/opensuse111/opensuse111_reference/data/sec_zypper.html) for SUSE systems
    + [rpm](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-rpm-using.html) for RHEL, CentOS, SUSE

+ Root or super user permissions


## Running setup on existing installations

The installation path for Machine Learning Server is new: /opt/microsoft/mlserver/${VERSION}. However, if R Server 9.x is present, Machine Learning Server 9.2.1 finds R Server at the old path (/usr/lib64/microsoft-r/#{VERSION}) and replaces it with the new version. 

There is no support for side-by-side installations of older and newer versions, nor is there support for hybrid versions (such as R Server 9.1 and Python 9.2.1). An installation is either entirely 9.2.1 or an earlier version.

## Installation paths

After installation completes, software can be found at the following paths:

+ Install root: /opt/microsoft/mlserver/9.2.1

+ Microsoft R Open root: /opt/microsoft/ropen/3.4.1

<a name="how-to-install"></a>

## How to install

Setup downloads packages from [packages.microsoft.com](https://packages.microsoft.com) for Linux installation.

> [!Tip]
> You can get Linux virtual machines in Azure if you want to use computing resources in the cloud: [Ubuntu virtual machine on Azure](https://docs.microsoft.com/sql/linux/quickstart-install-connect-ubuntu)

### Ubuntu 14.04 - 16.04

1. Enter `sudo su` to install as root.

2. Set the location of the packages using this example syntax for 16.04. Run each command separately. For more examples, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

    ```
     wget http://packages.microsoft.com/config/ubuntu/16.04/microsoft-mlserver-all-9.2.1.deb

     dpkg -i microsoft-mlserver-all-9.2.1.deb
    ```

3. Verify the mlserver.list configuration file exists: `ls -la /etc/apt/sources.list.d/`

4. Update packages on your system: `apt-get update` 

5. Optionally, if your system does not have the `https apt transport` option: `apt-get install apt-transport-https`

6. Install the server: `apt-get install microsoft-mlserver-all-9.2.1`  

7. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`     


### Red Hat and CentOS 6/7

1. Enter `sudo su` to install as root.

2. Set the location of the packages using this example syntax for 7.0. For more examples, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

   ```rpm -Uvh http://packages.microsoft.com/config/rhel/7/microsoft-mlserver-all-9.2.1.rpm```

3. Verify the mlserver.list cpnfiguration file exists: `ls -la /etc/apt/sources.list.d/`

4. Install the server: `yum install microsoft-mlserver-all-9.2.1` 

5. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`

### SUSE SLES11

1. Enter `sudo su` to install as root.

2. Set the location of the packages using this example syntax for SLES11. For more examples, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

   ```rpm -Uvh http://packages.microsoft.com/config/sles/11/microsoft-mlserver-all-9.2.1.rpm```

3. Verify the mlserver.list cpnfiguration file exists

4. Install the server: `zypper install microsoft-mlserver-all-9.2.1` 

5. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`


## Connect and validate

1. List installed packages:

  + On CentOS and RHEL: `rpm -qa | grep microsoft` 
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SLES11: `zypper se microsoft`

2. Once you have a package name, you can obtain verbose version information. For example:

   + On CentOS and RHEL: `$ rpm -qi microsoft-mlserver-packages-r-9.2.1`
   + On Ubuntu: `$ dpkg --status microsoft-mlserver-packages-r-9.2.1`  
   + On SLES: `zypper info microsoft-mlserver-packages-r-9.2.1`

Output on Ubuntu is as follows:

   ```
    Package: microsoft-mlserver-packages-r-9.2.1
    Status: install ok installed
    Priority: optional
    Section: devel
    Installed-Size: 195249
    Maintainer: revobuil@microsoft.com
    Architecture: amd64
    Version: 9.2.1.1287
    Depends: microsoft-r-open-mro-3.4.1, microsoft-r-open-mkl-3.4.1, microsoft-r-open-foreachiterators-3.4.1
    Description: Microsoft Machine Learning Server
	  . . .
   ```

### Start Revo64

As another verification step, run the Revo64 program. By default, Revo64 is installed in the /usr/bin directory, available to any user who can log in to the machine:

1. From /Home or any other working directory:

   `[<path>] $ Revo64`

2. Run a RevoScaleR function, such as **rxSummary** on a dataset. Many sample datasets, such as the iris dataset, are ready to use because they are installed with the software:

   `> rxSummary(~., iris)`

  Output from the iris dataset should look similar to the following:

~~~~
        Rows Read: 150, Total Rows Processed: 150, Total Chunk Time: 0.001 seconds
        Computation time: 0.005 seconds.
        Call:
        rxSummary(formula = ~., data = iris)

        Summary Statistics Results for: ~.
        Data: iris
        Number of valid observations: 150

         Name         Mean     StdDev    Min Max ValidObs MissingObs
         Sepal.Length 5.843333 0.8280661 4.3 7.9 150      0
         Sepal.Width  3.057333 0.4358663 2.0 4.4 150      0
         Petal.Length 3.758000 1.7652982 1.0 6.9 150      0
         Petal.Width  1.199333 0.7622377 0.1 2.5 150      0

        Category Counts for Species
        Number of categories: 3
        Number of valid observations: 150
        Number of missing observations: 0

         Species    Counts
         setosa     50
         versicolor 50
         virginica  50
~~~~

To quit the program, type `q()` at the command line with no arguments.

<a name="package-list"></a>

## Packages installed 

In addition to packages for Machine Learning Server, the following additional components are required for operationalization features and full language support.

* Microsoft R Open 3.4.1  
* Microsoft .NET Core 1.1 
* Anaconda 4.2 with Python 3.5

The following .NET and Microsoft packages are installed:

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

Other package dependencies are installed if the package is not found on the system. This list will vary for each installation.

## Enable operationalization

Machine Learning Server can be used as-is with an R IDE on the same box, but you can also [enable the server to host web services and to allow remote server connections](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization)

Configure the server to by running the [Administrator Utility](../operationalize/configure-use-admin-utility.md) to configure the server for remote access and execution, web service deployment, or cluster topologies. 

[Remote execution](../r/how-to-execute-code-remotely.md) makes the server accessible to client workstations running [R Client](../r-client/install-on-linux.md) on your network. Configuration steps are few and the benefit is big, so please take a few minutes to complete this task.

## What's installed

An installation of Machine Learning Server includes some or all of the following components.

| Component | Description |
|-----------|-------------|
| Microsoft R Open (MRO) | An open source distribution of the base R language, plus the Intel Math Kernel library (int-mkl). |
| R Server proprietary libraries and script engine | R Server packages provide libraries of functions. R Server libraries are co-located with R libraries in the `<install-directory>\library` folder. Libraries include RevoScaleR, MicrosoftML, mrsdeploy, olapR, RevoPemaR, and others listed in [R Package Reference](../r-reference/introducing-r-server-r-package-reference.md). <br/><br/>On Linux, the default R Server installation directory is `/opt/microsoft/mlserver/9.2.1`. <br/><br/>R Server is engineered for distributed and parallel processing for all multi-threaded functions, utilizing available cores and disk storage of the local machine. R Server also supports the ability to transfer computations to other R Server instances on other platforms through compute context instructions. |
| Python proprietary libraries | Propietary packages provide modules of class objects and static functions. Python libraries are in the `<install-directory>\lib\site-packages` folder. Libraries include revoscalepy, microsoftml, and azureml-model-management-sdk. <br/><br/>On Windows, the default installation directory is `C:\Program Files\Microsoft\ML Server\PYTHON_SERVER`.  |
| Anaconda 4.2 with Python 3.5.2 | An open source distribution of Python.|
| [Admin tool](../operationalize/configure-use-admin-utility.md) | Used for enabling remote execution and web service deployment, operationalizing analytics, and configuring web and compute nodes.| 
| [Pre-trained models](microsoftml-install-pretrained-models.md) | Used for sentiment analysis and image detection. |

Consider adding a development tool on the server to build script or solutions using R Server features:

+ [Visual Studio 2017](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://docs.microsoft.com/visualstudio/rtvs/installation) and the [Python for Visual Studio (PTVS)](https://www.visualstudio.com/vs/python/).

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

### See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)