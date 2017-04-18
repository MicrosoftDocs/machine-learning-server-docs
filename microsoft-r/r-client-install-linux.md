---

# required metadata
title: "Install Microsoft R Client on Linux"
description: "Microsoft R Client Install Guide Linux"
keywords: "R Client, R IDE configuration, RTVS,  Microsoft R Client Linux"
author: "j-martens"
manager: "jhubbard"
ms.date: "4/19/2017"
ms.topic: "get-started-article"
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
ms.technology: "r-client"
ms.custom: ""

---

# Install Microsoft R Client on Linux

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](scaler-getting-started.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is capped at two threads for RevoScaleR functions. 

To benefit from disk scalability, performance and speed, you can push the compute context using rxSetComputeContext() to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility.](r-client-compatibility.md)  

You can also run your code remotely on R Server using [remoteLogin() or remoteLoginAAD()](/operationalize/remote-execution.md) from the `mrsdeploy` package to offload heavy processing on server or to test your analytics during their development. 


## System Requirements

|   |   |
| - | - |
|Operating Systems|Supported versions include:<br>- Red Hat Enterprise Linux (RHEL) and CentOS 6.x and 7.x<br>- Ubuntu 14.04 and 16.04<br>- SUSE Linux Enterprise Server 11 (SLES11)<br>Must be a supported version of Linux on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.|
|Available RAM|2 GB of RAM is required; 8 GB or more are recommended|
|Free disk space|600 MB recommended, after installation of all prerequisites<br>1.2 GB recommended if pretrained models are installed|
|Internet access|Needed to download R Client and any dependencies. If you do not have an internet connection, for the instructions for an [offline installation](#offline)|


The following additional components are included in Setup and required for R Client.
* Microsoft R Open 3.3.3

>[!WARNING]
>Microsoft R Open is a requirement of both Microsoft R Client and R Server. Only install the version of Microsoft R Open required for your product. Typically, the installation of R Open is handled for you by the setup process. However, in offline scenarios when no internet connection is available on the target machine, you must manually download the R Open version using only the link specified in the installer or installation guide. Do NOT go to MRAN and download it from there or you may inadvertently get the wrong version for your Microsoft R product. 

## Setup Requirements

+ A package manager (yum for RHEL systems, zypper for SLES systems)

+ Root or super user permissions

+ You must install Microsoft R Client to a local drive on your computer.

+ You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.

## How to install (with internet access)

This section walks you through an R Client 3.3.3 deployment using the `install.sh` script. Under these instructions, your installation includes the ability to use the RevoScalerR and MicrosoftML packages.

1. Get the zipped installer file from http://aka.ms/rclientlinux. <br>
   Download the software to a writable directory, such as **/tmp**.

1. Unpack the distribution and run the installation script.

   The distribution includes one installer for Microsoft R Client. For a gzipped TAR file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as **/tmp**):

1. Log in as root or a user with super user privileges (`sudo su`). The following instructions assume user privileges with the sudo override.

1. Switch to the **/tmp** directory (assuming it's the download location).

1. Unpack the file:
   `[tmp] $ tar zxvf microsoft-r-client-3.3.3.tar.gz`

   The distribution is unpacked into an `MRC_Linux` folder at the download location. The distribution includes the following files:

   | File | Description |
   |------|-------------|
   |`install.sh` | Script for installing R Client. |
   | `EULA.txt` | End user license agreements for each separately licensed component. |
   | DEB folder | Contains Microsoft R packages for deployment on Ubuntu. |
   | RPM folder | Contains Microsoft R packages for deployment on CentOS/RHEL and SUSE |

   Microsoft R Client packages include a core engine and function libraries, platform packages, and machine learning.

1. Install R Client for Linux, which is deployed by running the install script with no parameters.

1. Verify system repositories are up to date:

   `[username] $ sudo yum clean all`

1. Change to the `MRC_Linux` directory containing the installation script:

   `[username] $ cd /tmp/MRC_Linux`

1. Run the script.

   `[tmp MRC_Linux] $ sudo bash install.sh`

1. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

1. Repeat for the R Client license agreement: click Enter, click **q** when finished reading, click **y** to accept the terms.

   Installation begins immediately. Installer output shows the packages and location of the log file. You can now [verify the installation](#verify).

## How to install offline (without internet access) 

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or limits on internet access prevent the installer from reaching these sites, you can download individual components on a computer that has internet access, copy the files to another computer behind the firewall, manually install prerequisites and packages, and then run setup.

If you previously installed version 3.3.2, it will be replaced with the 3.3.3 version. 

### Download R Client and its dependencies

From an internet-connected computer, download the following:

1. Download Microsoft R Client for Linux from http://aka.ms/rclientlinux.

1. Download Microsoft R Open, the R distribution used by R Client from  https://go.microsoft.com/fwlink/?linkid=845297 

   The file name for Microsoft R Open is `microsoft-r-open-3.3.3.tar.gz`. 

1. On certain platforms, also download package dependencies. The list of [required packages are the same as those for Microsoft R Server 9.1.0](rserver-install-linux-hadoop-packages.md). If the **target system** is missing any, download the ones you will need.

   > [!Note]
   > It's possible your Linux machine already has package dependencies installed. By way of illustration, on a few systems, only libpng12 had to be installed.

   To see what is currently installed, list the existing packages in /usr/lib64. It's common to have a very large number of packages. You can do a partial string search to filter on specific filenames (such as lib* for files starting with lib.)

   `ls -l /usr/lib64/lib*`

1. Transfer these files to the machine without any internet connection.

   Use a tool like [SmarTTY](http://smartty.sysprogs.com/download/) or [PuTTY](http://www.putty.org) or another mechanism to upload or transfer files to a writable directory, such as **/tmp**, on your internet-restricted server. Files to be transferred include the following:
   + `dotnet-<linux-os-name>-x64.1.1.1.tar.gz`
   + `microsoft-r-open-3.3.3.tar.gz`
   + `microsoft-r-client-3.3.3.tar.gz`
   + any missing packages from the dependency list

### Prepare the downloaded files for installation

On the target system which is disconnected from the internet:

1. Be sure you have put the downloaded files into a writable directory, such as **/tmp**.

1. Install any missing package dependencies. Run the following command to install any packages that are missing from your system.
   `rpm -qi <package-name>`

1. Unpack the distributions for .NET Core and Microsoft R Client.

   >[!Important] 
   >Do not unpack Microsoft R Open. Instead, copy the gzipped tar file to the MRC_Linux directory after you've unpacked Microsoft R Client.

   1. Log in as root or as a user with super user privileges (sudo -s). The following instructions assume user privileges with the sudo override.

   1. Switch to the **/tmp** directory (assuming it's the download location).

   1. Unpack the Microsoft R Client gzipped file:
      `[root@localhost tmp] $ tar zxvf microsoft-r-client-3.3.3.tar.gz`

1. Copy the gzipped tar file for Microsoft R Open to the MRC_Linux directory.  The install.sh script file for R Client looks for the gzipped tar file for Microsoft R Open. Assuming root permissions, copy the gzipped Microsoft R Open tar file to the same folder containing the installation script.

  `[root@localhost tmp] $ cp microsoft-r-open-3.3.3.tar.gz /tmp/MRC_Linux`

### Run the Microsoft R Client install script

R Client for Linux is deployed by running the install script with no parameters.

1. Switch to the `MRC_Linux` directory containing the installation script:

  `[root@localhost tmp] $ cd MRC_Linux`

1. Run the script.

   `[root@localhost MRC_Linux] $ bash install.sh`

1. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

1. When prompted to accept the license terms for Microsoft R Client, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

Installer output shows the packages and location of the log file. You can now [verify the installation](#verify).

## Unattended installs

You can bypass the interactive install steps of the Microsoft R Client install script with the `-u` flag. Additional flags are available, as follows:

flag | Option | Description
-----|--------|------------
 -a | --accept-eula | Accept all end user license agreements.
 -d | --download-mro |  Download microsoft r open for distribution to an offline system.
 -m | --models | Install Microsoft ML models.
 -r | --no-dotnet-core | Opt out of installing .NET Core (required for mrsdeploy and MicrosoftML)
 -s | --silent | Perform a silent, unattended install.
 -u | --unattended | Perform an unattended install.
 -h | --help | Print this help text.

For a standard unattended install, run the following script:

	./install.sh –a –s



## Learn More

You can learn more with these guides:

+ [Get Started with Microsoft R Client](r-client-get-started.md) 

+ [Microsoft R Getting Started](microsoft-r-getting-started.md) 

+ [Diving into Data Analysis with Microsoft R](data-analysis-in-microsoft-r.md)

+ [Get started with RevoScaleR](microsoft-r-get-started-node.md)

+ [Get started with MicrosoftML](microsoftml-get-started.md)

+ [Get started with mrsdeploy](mrsdeploy/mrsdeploy.md)
