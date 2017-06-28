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

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](../scaler-getting-started-data-import-exploration.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is capped at two threads for RevoScaleR functions. 

To benefit from disk scalability, performance and speed, you can push the compute context using rxSetComputeContext() to a production instance of Microsoft R Server such as [SQL Server Machine Learning Services](https://docs.microsoft.com/en-us/sql/advanced-analytics/r/sql-server-r-services) and R Server for Hadoop. [Learn more about its compatibility.](compatibility-with-server.md)  

You can offload heavy processing to R Server or test your analytics during their developmentYou by running your code remotely using [remoteLogin() or remoteLoginAAD()](../operationalize/remote-execution.md) from the `mrsdeploy` package.  


## System Requirements

|   |   |
| - | - |
|Operating Systems|Supported versions include:<br>- Red Hat Enterprise Linux (RHEL) and CentOS 6.x and 7.x<br>- Ubuntu 14.04 and 16.04<br>- SUSE Linux Enterprise Server 11 (SLES11)<br><br>Must be a supported version of Linux on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.|
|Available RAM|2 GB of RAM is required; 8 GB or more are recommended|
|Free disk space|600 MB recommended, after installation of all prerequisites<br>1.2 GB recommended if pretrained models are installed|
|Internet access|Needed to download R Client and any dependencies. If you do not have an internet connection, for the instructions for an [offline installation](#offline)|


Also included and required for R Client setup is Microsoft R Open 3.3.3.  Microsoft R Open is a requirement of Microsoft R Client. In offline scenarios when no internet connection is available on the target machine, you must manually download the R Open installer. Use only the link specified in the installer or installation guide. Do NOT go to MRAN and download it from there or you may inadvertently get the wrong version for your Microsoft R product. 

## Setup Requirements

+ A package manager (yum for RHEL systems, zypper for SLES systems)

+ Root or super user permissions

+ You must install Microsoft R Client to a local drive on your computer.

+ You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.

## How to install (with internet access)

This section walks you through an R Client 3.3.3 deployment using the `install.sh` script. Under these instructions, your installation includes the ability to use the RevoScalerR and MicrosoftML packages.


1. Log in as root or a user with super user privileges (`sudo su`). The following instructions assume root install.

1. Get the zipped installer file from http://aka.ms/rclientlinux. <br>
   Download the software to a writable directory, such as **/tmp**.

1. Switch to the **/tmp** directory (assuming it's the download location).

1. Unpack the distribution and run the installation script.

1. Unpack the file:

   ```
   [tmp] $ tar zxvf microsoft-r-client-3.3.3.tar.gz
   ```

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

   ```
   [root@localhost tmp] $ yum clean all
   ```

1. Change to the `MRC_Linux` directory containing the installation script:

   ```
   [root@localhost tmp] $ cd /tmp/MRC_Linux
   ```

1. Run the script. 

   To include the [**pretrained machine learning models for MicrosoftML**](../install/microsoftml-install-pretrained-models.md) when you install, then specify the switch `-m` for install.sh. 

   ```
   [root@localhost MRC_Linux] $ bash install.sh
   ```

1. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

1. Repeat for the R Client license agreement: click Enter, click **q** when finished reading, click **y** to accept the terms.

   Installation begins immediately. Installer output shows the packages and location of the log file. 
   
   You can now [set up your IDE and try out some sample code](../r-client-get-started.md).

<br>

## How to install offline (without internet access) 

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or limits on internet access prevent the installer from reaching these sites, you can download individual components on a computer that has internet access, copy the files to another computer behind the firewall, manually install prerequisites and packages, and then run setup.

If you previously installed version 3.3.2, it will be replaced with the 3.3.3 version. 

### Download R Client and its dependencies

From an internet-connected computer, download the following:

1. Download Microsoft R Client for Linux from http://aka.ms/rclientlinux.

1. Download the Microsoft R Open for this version of R Client. Microsoft R Open provides the R distribution used by R Server. [Direct link to microsoft-r-open-3.3.3.tar.gz](https://go.microsoft.com/fwlink/?linkid=845297). Use the link provided to get the required component. Do NOT go to MRAN and download the latest or you may end up with the wrong version. 

1. On certain platforms, also download package dependencies. The list of [required packages are the same as those for Microsoft R Server 9.1.0](../install/r-server-install-linux-hadoop-packages.md). If the **target system** is missing any, download the ones you will need.

   If you will be building and installing packages, including miniCRAN, we recommend that you also install the following binary packages: `gcc-c++` and `gcc-gfortran`.
   
   It's possible your Linux machine already has package dependencies installed. By way of illustration, on a few systems, only libpng12 had to be installed.

   To see what is currently installed, list the existing packages in /usr/lib64. It's common to have a very large number of packages. You can do a partial string search to filter on specific filenames (such as lib* for files starting with lib.)

   ```
   ls -l /usr/lib64/lib*
   ```

1. Transfer these files to the machine without any internet connection  to a writable directory, such as **/tmp**, on your internet-restricted server. Use a a tool like [SmarTTY](http://smartty.sysprogs.com/download/) or [PuTTY](http://www.putty.org) or another mechanism.

   Files to be transferred include the following:
   + `microsoft-r-open-3.3.3.tar.gz`
   + `microsoft-r-client-3.3.3.tar.gz`
   + any missing packages from the dependency list

### Prepare the downloaded files for installation

On the target system which is disconnected from the internet:

1. Log in as root or as a user with super user privileges (sudo -s). The following instructions assume root install.

1. Be sure you have put the downloaded files into a writable directory, such as **/tmp**.

1. Execute `ls` to list the files as a verification step. You should see the tar.gz files you transferred earlier.

1. Install any missing package dependencies. run `rpm -i <package-name>` to install any packages that are missing from your system (for example, `rpm -i libpng12-1-2-50-10.el7.x86_64.rpm`).

   >[!Important] 
   >Do not unpack Microsoft R Open. Instead, copy the gzipped tar file to the MRC_Linux directory after you've unpacked Microsoft R Client.

1. Unpack the Microsoft R Client gzipped file:
   ```
   [root@localhost tmp] $ tar zxvf microsoft-r-client-3.3.3.tar.gz
   ```

   A new folder called MRC_Linux is created under /tmp. This folder contains files and packages used during setup. 

1. Copy the gzipped tar file for Microsoft R Open to the MRC_Linux folder containing the installation script (install.sh).  The install.sh script file for R Client looks for the gzipped tar file for Microsoft R Open. Assuming root permissions, copy the gzipped Microsoft R Open tar file to the same folder containing the installation script.

  ```
  [root@localhost tmp] $ cp microsoft-r-open-3.3.3.tar.gz /tmp/MRC_Linux
  ```

### Run the Microsoft R Client install script

R Client for Linux is deployed by running the install script with no parameters.

1. Switch to the `MRC_Linux` directory containing the installation script:

   ```
   [root@localhost tmp] $ cd MRC_Linux
   ```

1. Run the script. 

   To include the [**pretrained machine learning models for MicrosoftML**](../install/microsoftml-install-pretrained-models.md) when you install, then specify the switch `-m` for install.sh. 

   ```
   [root@localhost MRC_Linux] $ bash install.sh
   ```

1. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

1. When prompted to accept the license terms for Microsoft R Client, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

   Installer output shows the packages and location of the log file. 
   
   You can now [set up your IDE and try out some sample code](../r-client-get-started.md). Also, consider package management as described in the next section.

### Offline Package Management

Review the recommendations in [Package Management](../operationalize/configure-manage-r-packages.md#offline) for instructions on how to set up a local package repository using MRAN or miniCRAN. As we mentioned earlier, you must install the `gcc-c++` and `gcc-gfortran` binary packages to be able to build and install packages, including miniCRAN.

## Unattended installs

You can perform a silent install to bypass prompts during setup. In /tmp/MRC_Linux, run the install script with the following parameters:

```
[root@localhost MRC_LINUX] $ install.sh -a -s -m
```

Additional flags are available, as follows:

flag | Option | Description
-----|--------|------------
 -a | --accept-eula | Accept all end user license agreements.
 -d | --download-mro |  Download microsoft r open for distribution to an offline system.
 -m | --models | Install Microsoft ML pretrained models.
 -s | --silent | Perform a silent, unattended install.
 -u | --unattended | Perform an unattended install.
 -h | --help | Print this help text.

## How to uninstall R Client 3.3.3

1. Log in as root or a user with `sudo` privileges. If you are using `sudo`, precede commands requiring root privileges with `sudo` .

1. Use your package manager to list the currently installed R Server packages. 
   + If your package manager is **yum** (common for CentOS/Red Hat systems):

	   ```
     yum list \*microsoft-r\*
     ```

   + If your package manager is **apt-get** (common for Ubuntu systems):

	   ```
     apt list \*microsoft-r\*
     ```

   + If your package manager is **zypper** (common for SLES systems):

	   ```
     zypper search \*microsoft-r\*
     ```

  Packages are registered in a database that tracks all package installations in the cluster. To update the database, use your yum, apt-get, or zypper package manager to remove the package (for example, `sudo yum erase microsoft-r-server-mro-8.0`).

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

   ```
   yum erase microsoft-r-open-mro-3.3         #For CentOS/RHEL systems
   apt-get remove microsoft-r-open-mro-3.3    #For Ubuntu systems
   zypper remove microsoft-r-open-mro-3.3     #For SLES systems
   ```

2. On the root node, verify the location of other files that need to be removed: `

   ```
   ls /usr/lib64/microsoft-r
   ```

3. Remove the entire directory:

   ```
   rm -fr /usr/lib64/microsoft-r
   ```

   The **rm** command removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## Learn More

You can learn more with these guides:

+ [Overview of Microsoft R](../index.md) 

+ [Overview of Microsoft R Client](../r-client-get-started.md) 

+ [Quickstart: Running R code in Microsoft R](../quickstart-r-code.md) (example)

+ [Diving into data analysis with Microsoft R](../data-analysis-in-microsoft-r.md)

+ [RevoScaleR R package reference](../microsoft-r-tutorials.md)

+ [MicrosoftML R package reference](../r/concept-what-is-the-microsoftml-package.md)

+ [mrsdeploy R package reference](../r-reference/mrsdeploy/mrsdeploy-package.md)

+ [Execute code on remote R Server](../operationalize/remote-execution.md)
