---
# required metadata
title: "Linux offline installation for Machine Learning Server 9.2.1 | Microsoft Docs"
description: "How to install Machine Learning Server for Linux with no internet connection."
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

# Offline installation of Machine Learning Server for Linux 

By default, installers connect to Microsoft download sites to get required and updated components for Machine Learning Server 9.2.1 for Linux. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

Before you start, review the following article for requirements and restrictions:

+ [Install Machine Learning Server 9.2.1 on Linux](machine-learning-server-linux-install.md) for an internet-connected installation.

> [!Note] 
> In contrast with previous releases, there is no install.sh script in this release.

<a name="file-list"></a>

## Package location

Packages for all supported versions of Linux can be found at [packages.microsoft.com](https://packages.microsoft.com). The base file name is microsoft-mlserver-all-9.2.1.<ext> with different file extensions based on the package manager of the target platform.

## Step 1: Download

Download the necessary packages to a computer connected to the internet. If your system provides a graphical user interface, you can click the file to download it. Otherwise, use the follwoing commands:

+ Download to CentOS or RHEL:
+ Download to Ubuntu:
+ Download to SUSE:

For more information, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

## Step 2: Verify

You should find the files at the following locations.

TBD
TBD
TBD

## Step 3: Get dependencies

Run the following commands to produce a list of dependent packages.

rpm -qpR {.rpm-file}
rpm -qR {package}

**On Ubuntu:** `dpkg -I microsoft-mlserver-all-9.2.1.deb | grep Depends`

**On CentOS, RHEL SUSE:**  `rpm -qpR microsoft-mlserver-all-9.2.1.rpm`

Download thw additional packages reported by the above commands.

## Step 4: Transfer

Use a tool or device to transfer the files to the offline server. Place files in the following locations:

TBD
TBD

## Step 5: Install

After files are placed, run setup following the same procedure as articulated for internet-connected setup:

+ [Install Machine Learning Server > How to install](machine-learning-server-linux-install.md#how-to-install)

## Step 6: Connect and validate

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

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

### See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)