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

Machine Learning Server for Linux runs machine learning and data mining solutions written in R or Python in standalone and clustered topologies. 

This article explains how to install Machine Learning Server 9.2.1 on a standalone Linux server that has an internet connection. If your server has restrictions on internet access, see [offline installation](machine-learning-server-linux-offline.md). 

> [!Note]
> Python support is new and there are a few limitations in remote computing scenarios. 1) Remote execution is not supported on Windows or Linux. 2) Remote compute contexts must be Spark or SQL Server. In computing that is local to the machine, there are no limitations.

## System and setup requirements

+ Operating system must be a [supported version of 64-bit Linux](r-server-install-supported-platforms.md).

+ Minimum RAM is 2 GB. Minimum disk space is 500 MB (8 GB or more is recommended).

+ An internet connection. If you do not have an internet connection, use the [offline installation instructions](machine-learning-server-linux-offline.md).

+ Root or super user permissions

<a name="package-manager"></a>

## Package managers

Installation is through package managers. Unlike previous releases, there is no install.sh script. 

| Package manager | Platform |
|-----------------|----------|
|[yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html) | RHEL, CentOS|
|[apt](https://help.ubuntu.com/lts/serverguide/apt.html) | Ubuntu online |
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
+ Executables like Revo64 and mlserver-python are at `/usr/bin`

<a name="how-to-install"></a>

## How to install

The package manager downloads packages from the [packages.microsoft.com](https://packages.microsoft.com) repo, determines dependencies, retrieves additional packages, sets the installation order, and installs the software. For example syntax on setting the repo, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

Activation is a separate step. If you forget to activate, the server works, but the following error appears when you call an API: "Express Edition will continue to be enforced."

### Red Hat and CentOS 6/7

1. Install as root: `sudo su`

2. Set the location of the package repo at the **prod** directory, which contains the Machine Learning Server distribution. This example specifies 7.0: `rpm -Uvh http://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm`

3. As a verification step, check whether the **microsoft-prod.repo** configuration file exists: `ls -la /etc/yum.repos.d/` 

4. Update packages on your system: `yum update` 

5. Install the server: `yum install microsoft-mlserver-all-9.2.1` 

6. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`

### Ubuntu 14.04 - 16.04

1. Install as root: `sudo su`

2. Optionally, if your system does not have the https apt transport option: `apt-get install apt-transport-https`

3. Set the location of the package repo the **prod** directory, which contains the Machine Learning Server distribution. This example specifies 16.04: `wget http://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb`

4. Register the repo: `dpkg -i packages-microsoft-prod.deb`

5. As a verification step, check whether the **microsoft-prod.list** configuration file exists: `ls -la /etc/apt/sources.list.d/`

6. Update packages on your system: `apt-get update` 

7. Install the server: `apt-get install microsoft-mlserver-all-9.2.1`  

8. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`     


### SUSE (SLES11 only)

1. Install as root: `sudo su`

2. Set the location of the package repo at the **prod** directory, which contains the Machine Learning Server distribution. This example is for SLES11, the only supported version of SUSE in Machine Learning Server: `zypper -ar -f http://packages.microsoft.com/sles/11/prod packages-microsoft-com`

3. Update packages on your system: `zypper update` 

4. Install the server: `zypper install microsoft-mlserver-sles11-9.2.1` 

5. You might get a message stating that PackageKit is blocking zypper. Enter `y` to quit PackageKit and allow zypper to continue.

6. You are prompted whether to trust the repository signing key. You can choose `t` to temporarily trust the key for the purposes of downloading and installing Machine Learning Server. 

7. You might get a "Digest verification failed" message (this is a temporary issue that will be resolved soon). Enter `y` to continue.

8. When asked to confirm the list of packages to install, enter `y` to continue. 

9. Review and accept the license agreements for MRO, Anaconda, and Machine Learning Server.

10. Activate the server: `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`


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

As another verification step, run the **Revo64** program. By default, **Revo64** is installed in the /usr/bin directory, available to any user who can log in to the machine:

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

### Start Python

1. From Home or any other user directory:

   `[<path>] $ mlserver-python`

2. Run a revoscalepy function, such as **rx_Summary** on a dataset. Many sample datasets are built in. At the Python command prompt, paste the following script:

    ~~~~
    import os
    import revoscalepy 
    sample_data_path = revoscalepy.RxOptions.get_option("sampleDataDir")
    ds = revoscalepy.RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
    summary = revoscalepy.rx_summary("ArrDelay+DayOfWeek", ds)  
    print(summary)
    ~~~~

  Output from the sample dataset should look similar to the following:

    ~~~~ 
    Summary Statistics Results for: ArrDelay+DayOfWeek
    File name: /opt/microsoft/mlserver/9.2.1/libraries/PythonServer/revoscalepy/data/sample_data/AirlineDemoSmall.xdf
    Number of valid observations: 600000.0
    
            Name       Mean     StdDev   Min     Max  ValidObs  MissingObs
    0  ArrDelay  11.317935  40.688536 -86.0  1490.0  582628.0     17372.0
    
    Category Counts for DayOfWeek
    Number of categories: 7
    
                Counts
    DayOfWeek         
    1          97975.0
    2          77725.0
    3          78875.0
    4          81304.0
    5          82987.0
    6          86159.0
    7          94975.0
    ~~~~

To quit the program, type `quit()` at the command line with no arguments.

## Enable server to host analytic web services and accept remote connections

To benefit from [hosting your Python and R script as a web service](../operationalize/concept-what-are-web-services.md) or [remote R code execution](../r/how-to-execute-code-remotely.md), [configure the server for operationalization](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization). Remote execution makes the server accessible to client workstations running [R Client](../r-client/install-on-linux.md) on your network. 

To configure the server, use the [Administrator Utility](../operationalize/configure-use-admin-utility.md). The configuration steps are few and the benefit is substantial, so please take a few minutes to complete this task.

## What's installed

An installation of Machine Learning Server includes some or all of the following components.

| Component | Description |
|-----------|-------------|
| Microsoft R Open (MRO) | An open source distribution of the base R language, plus the Intel Math Kernel library (int-mkl). |
| R proprietary libraries and script engine | Properietary R libraries are co-located with R base libraries. Libraries include RevoScaleR, MicrosoftML, mrsdeploy, olapR, RevoPemaR, and others listed in [R Package Reference](../r-reference/introducing-r-server-r-package-reference.md). <br/><br/>On Linux, the default R installation directory is `/opt/microsoft/mlserver/9.2.1`. <br/><br/>RevoScaleR is engineered for distributed and parallel processing for all multi-threaded functions, utilizing available cores and disk storage of the local machine. RevoScaleR also supports the ability to transfer computations to other RevoScaleRr instances on other computers and platforms through compute context instructions. |
| Python proprietary libraries | Proprietary packages provide modules of class objects and static functions. Libraries include revoscalepy, microsoftml, and azureml-model-management-sdk. |
| Anaconda 4.2 with Python 3.5.2 | An open source distribution of Python.|
| [Admin tool](../operationalize/configure-use-admin-utility.md) | Used for enabling remote execution and web service deployment, operationalizing analytics, and configuring web and compute nodes.| 
| [Pre-trained models](microsoftml-install-pretrained-models.md) | Used for sentiment analysis and image detection. |

Consider adding a development tool on the server to build script or solutions using R Server features:

+ [Visual Studio 2017](https://www.visualstudio.com/downloads/) followed by the [R Tools for Visual Studio (RTVS) add-in](https://docs.microsoft.com/visualstudio/rtvs/installation) and the [Python for Visual Studio (PTVS)](https://www.visualstudio.com/vs/python/).

<a name="package-list"></a>

## Package list 

The following packages comprise a full Machine Learning Server installation:

```
 microsoft-mlserver-packages-r-9.2.1        ** core
 microsoft-mlserver-python-9.2.1            ** core
 microsoft-mlserver-packages-py-9.2.1       ** core
 microsoft-mlserver-mml-r-9.2.1             ** microsoftml for R (optional)
 microsoft-mlserver-mml-py-9.2.1            ** microsoftml for Python (optional)
 microsoft-mlserver-mlm-r-9.2.1             ** pre-trained models (requires mml)
 microsoft-mlserver-mlm-py-9.2.1            ** pre-trained models (requires mml)
 microsoft-mlserver-hadoop-9.2.1            ** hadoop (required for hadoop)
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

Additional open source packages are installed if a package is required but not found on the system. This list varies for each installation. Refer to [offline installation](machine-learning-server-linux-offline.md) for an example list.

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)