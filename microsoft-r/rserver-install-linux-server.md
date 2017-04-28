---

# required metadata
title: "R Server 9.1 installation for Linux systems"
description: "Install Microsoft R Server 9.1.0 on Linux."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/28/2017"
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

# R Server 9.1 Installation for Linux Systems

Microsoft R Server is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers and clusters. The server runs on a wide range of computing platforms, including Linux. 

This article explains how to install Microsoft R Server 9.1.0 on a standalone Linux server that has an internet connection. If your server has restrictions on internet access, see the instructions for an [offline installation](rserver-install-windows-offline.md).

If you previously installed version 9.0.1, it will be replaced with the 9.1.0 version. An 8.x version can run side-by-side 9.x, unaffected by the new installation.

## System requirements

+ Operating system must be a [supported version of Linux](rserver-install-supported-platforms.md) on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

+ Memory must be a minimum of 2 GB of RAM is required; 8 GB or more are recommended.

+ Disk space must be a minimum of 500 MB.

+ An internet connection. If you do not have an internet connection, for the instructions for an [offline installation](rserver-install-linux-offline.md).

+ A package manager (yum for RHEL systems, zypper for SLES systems)

+ Root or super user permissions

The following additional components are included in Setup and required for R Server.

* Microsoft R Open 3.3.3
* Microsoft .NET Core 1.1 for Linux (required for mrsdeploy and MicrosoftML use cases)

<a name="howtoinstall"></a>
## How to install

This section walks you through an R Server 9.1.0 deployment using the `install.sh` script. Under these instructions, your installation will be serviced under the [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) and includes the ability to [operationalize your analytics](operationalize/about.md) and use the MicrosoftML package.

> [!Tip]
> Review [recommendations and best practices](rserver-install-linux-manage-install.md) for deployments in locked down environments.
>

<a name="download"><a/>
### Download R Server installer

You can get the gzipped installation file from one of the following download sites.  

| Site | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios. <br/><br/>1. Click **Join or Access Now** and enter your account information.<br/>2. Make sure you're in the right place: *my.visualstudio.com*.<br/>3. Click **Downloads**, and then search for *Microsoft R*. |
|[Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for R Server for Linux. A selection for **R Server 9.1.0 for Linux** is provided on this site. |
| [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) | Developer or Enterprise | Subscribers can download software at given subscription levels. Depending on your subscription, you can get either edition. |

### Unpack the distribution

Download the software to a writable directory, such as **/tmp**, unpack the distribution and then run the installation script.

The distribution includes one installer for Microsoft R Server. For a gzipped TAR file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as **/tmp**):

1. Log in as root or a user with super user privileges (`sudo su`).

2. Switch to the **/tmp** directory (assuming it's the download location).

3. Unpack the file:

  `[tmp] $ tar zxvf en_microsoft_r_server_910_for_linux_x64_10323878.tar.gz`

The distribution is unpacked into an `MRS91Linux` folder at the download location. The distribution includes the following files:

| File | Description |
|------|-------------|
|`install.sh` | Script for installing R Server. |
|`generate_mrs_parcel.sh` | Script for generating a parcel used for [installing R Server on CDH](rserver-install-cloudera.md). |
| `EULA.txt` | End user license agreements for each separately licensed component. |
| DEB folder | Contains Microsoft R packages for deployment on Ubuntu. |
| RPM folder | Contains Microsoft R packages for deployment on CentOS/RHEL and SUSE |
| Parcel folder | Contains files used to generate a parcel for installation on CDH. |

MRS packages include an admin utility, core engine and function libraries, compute node, web node, platform packages, and machine learning.

> [!Important]
> Package names in the R Server distribution have changed in the 9.1 release. Instead of DeployrR-themed package names, the new names are aligned to base packages. If you have script or tooling for manual R Server package installation, be sure to note the name change.
>

### Run the MRS install script

R Server for Linux is deployed by running the install script with no parameters.

1. Log in as root or as a user with super user privileges (`sudo -s`). The following instructions assume root install.

2. Clean cached data so that the installer can get updated packages. 

  + On RHEL use [yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Working_with_Yum_Cache.html): `[root@localhost tmp] $ yum expire-cache`  
  + On Ubuntu use [apt-get](https://help.ubuntu.com/community/AptGet/Howto): `[root@localhost tmp] $ apt-get autoclean` 

3. Change to the `MRS91Linux` directory containing the installation script:

  `[root@localhost tmp] $ cd /tmp/MRS91Linux`

4. Run the script. To include the [pretrained machine learning models for MicrosoftML](deploy-pretrained-microsoftml-models.md), append the `-m` switch. 

   `[root@localhost MRS91Linux] $ bash install.sh -m`

5. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

6. Repeat for the R Server license agreement: click Enter, click **q** when finished reading, click **y** to accept the terms.

  Installation begins immediately. Installer output shows the packages and location of the log file.

### Verify installation

1. List installed MRS packages:

  + On RHEL: `rpm -qa | grep microsoft` 
  + On Ubuntu: `apt list --installed | grep microsoft`  

2. Once you have a package name, you can obtain verbose version information. For example:

   + On RHEL: `$ rpm -qi microsoft-r-server-packages-9.1.x86_64`
   + On Ubuntu: `$ dpkg --status microsoft-r-server-packages-9.1.x86_64`  

Partial output is as follows (note version 9.1.0):

   ```
	  Name        : microsoft-r-server-packages-9.1     Relocations: /usr/lib64
	  Version     : 9.1.0                               Vendor: Microsoft
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

## Enable Remote Connections and Analytic Deployment

The server can be used as-is if you install and use an R IDE on the same box, but to benefit from the deployment and consumption of web services with Microsoft R Server, then you must configure R Server after installation to act as a deployment server and host analytic web services. Possible configurations are a [one-box setup](operationalize/configuration-initial.md) or an [enterprise setup](operationalize/configure-enterprise.md). Doing so also enables remote execution, allowing you to connect to R Server from an R Client workstation and execute code on the server.

<a name="unattended"></a>

## Unattended install options

You can perform a silent install to bypass prompts during setup. In /tmp/MRS91Linux, run the install script with the following parameters:

   `[root@localhost MRS91Linux] $ install.sh -a -s`

Additional flags are available, as follows:

flag | Option | Description
-----|--------|------------
 -a | --accept-eula | Accept all end user license agreements.
 -d | --download-mro |  Download microsoft r open for distribution to an offline system.
 -m | --models | Install Microsoft ML models.
 -r | --no-dotnet-core | Opt out of installing .NET Core (required for mrsdeploy and MicrosoftML)
 -s | --silent | Perform a silent, unattended install.
 -u | --unattended | Perform an unattended install.
 -h | --help | Print this help text.

## What's Installed with R Server

The Microsoft R Server setup installs the R base packages and a set of enhanced and proprietary R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop. In contrast with R Client, R Server supports much larger data sets and distributed workloads.

> [!NOTE]
> By default, telemetry data is collected during your usage of R Server. To turn this feature off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.

## Next Steps

Review the best practices in [Manage your R Server for Linux installation](rserver-install-linux-manage-install.md) for instructions on how to set up a local package repository using MRAN or miniCRAN, change file ownership or permissions, set Revo64 as the de facto R script engine on your server.

## See Also

 [Introduction to R Server](rserver.md) 
 [What's New in R Server](rserver-whats-new.md)
 [Supported platforms](rserver-install-supported-platforms.md)  
 [Known Issues](rserver-known-issues.md)  
 [Install R on Hadoop overview](rserver-install-hadoop.md)  
 [Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-linux-uninstall.md) 
 [Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)  
 [Configure R Server to operationalize analytics](operationalize/configuration-initial.md)