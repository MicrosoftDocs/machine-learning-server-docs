---

# required metadata
title: "Install Microsoft R Client on Linux - Machine Learning Server "
description: "Guide to installing Microsoft R Client on Linux. R Client is a free, data science tool for high performance analytics."
keywords: "R Client, R IDE configuration, RTVS,  Microsoft R Client Linux"
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "9/25/2017"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-client"
#ms.custom: ""

---

# Install Microsoft R Client on Linux

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](../r/tutorial-revoscaler-data-import-transform.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of RevoScaleR functions, but there are some constraints. On its own, the data to be processed must fit in local memory, and processing is capped at two threads for RevoScaleR functions. 

To benefit from disk scalability, performance and speed, you can push the compute context using rxSetComputeContext() to a production instance of Machine Learning Server (or Microsoft R Server) such as [SQL Server Machine Learning Services](https://docs.microsoft.com/en-us/sql/advanced-analytics/r/sql-server-r-services) and Machine Learning Server for Hadoop. [Learn more about its compatibility.](compatibility-with-server.md)  

You can offload heavy processing to Machine Learning Server or test your analytics during their development. You by running your code remotely using [remoteLogin() or remoteLoginAAD()](../r/how-to-execute-code-remotely.md) from the mrsdeploy package.  

For a What's New for Microsoft R Client, see [here](what-is-microsoft-r-client.md#r-client-whats-new).

## System Requirements

|   |   |
| - | - |
|Operating Systems|Supported versions include:<br>- Red Hat Enterprise Linux (RHEL) and CentOS 6.x and 7.x<br>- Ubuntu 14.04 and 16.04<br>- SUSE Linux Enterprise Server 11 (SLES11)<br><br>Must be a supported version of Linux on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.|
|Available RAM|2 GB of RAM is required; 8 GB or more are recommended|
|Free disk space|600 MB recommended, after installation of all prerequisites<br>1.2 GB recommended if pre-trained models are installed|
|Internet access|Needed to download R Client and any dependencies. If you do not have an internet connection, for the instructions for an [offline installation](#offline)|


Also included and required for R Client setup is Microsoft R Open 3.4.1.  Microsoft R Open is a requirement of Microsoft R Client. In offline scenarios when no internet connection is available on the target machine, you must manually download the R Open installer. Use only the link specified in the installer or installation guide. Do NOT go to MRAN and download it from there or you may inadvertently get the wrong version for your Microsoft R product. 

## Setup Requirements

+ A package manager from this list:
  | Package manager | Platform |
  |-----------------|----------|
  |[yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html) | RHEL, CentOS|
  |[apt](https://help.ubuntu.com/lts/serverguide/apt.html) | Ubuntu online |
  |[dpkg](https://help.ubuntu.com/lts/serverguide/dpkg.html) | Ubuntu offline |
  |[zypper](https://www.suse.com/documentation/opensuse111/opensuse111_reference/data/sec_zypper.html) | SUSE |
  |[rpm](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-rpm-using.html) | RHEL, CentOS, SUSE |

+ Root or super user permissions

+ You must install Microsoft R Client to a local drive on your computer.

+ You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.

## Installation paths
After installation completes, software can be found at the following paths:
+ Install root: /opt/microsoft/rclient/3.4.1
+ Microsoft R Open root: /opt/microsoft/ropen/3.4.1
+ Executables like Revo64 are under /usr/bin

There is no support for side-by-side installations of older and newer versions. 

## How to install (with internet access)

This section walks you through an R Client 3.4.1 deployment. Under these instructions, your installation includes the ability to use the RevoScaleR, MicrosoftML packages, and mrsdeploy.

The package manager downloads packages from the packages.microsoft.com repo, determines dependencies, retrieves additional packages, sets the installation order, and installs the software. For example syntax on setting the repo, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/en-us/windows-server/administration/linux-package-repository-for-microsoft-software).

> [!Note]
> If the repository configuration file is not present in the /etc directory, try [manual configuration](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software#manual-configuration) for repository registration.

**On Ubuntu 14.04 - 16.04**

With root or sudo permissions, run the following commands:
```
# Install as root or sudo
sudo su

# If your system does not have the https apt transport option, add it now
apt-get install apt-transport-https

# Set the package repository location containing the R Client distribution. 
# On Ubuntu 14.04.
# wget http://packages.microsoft.com/config/ubuntu/14.04/prod/packages-microsoft-prod.deb 
# On Ubuntu 16.04.
wget http://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb 

# Register the repo.
dpkg -i packages-microsoft-prod.deb

# Check for microsoft-prod.list configuration file to verify registration.
ls -la /etc/apt/sources.list.d/

# Update packages on your system
apt-get update

# Install the packages
apt-get install microsoft-r-client-packages-3.4.1

# List the packages
ls /opt/microsoft/r-client/
```     

**Red Hat and CentOS 6/7**

With root or sudo permissions, run the following commands:
```
# Install as root or sudo
sudo su

# Set the package repository location containing the R Client distribution. 
# On RHEL 6:
# rpm -Uvh http://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
# On RHEL 7:
rpm -Uvh http://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Check for microsoft-prod.repo configuration file to verify registration.
ls -la /etc/yum.repos.d/ 

# Update packages on your system
yum update

# Install the packages
yum install microsoft-r-client-packages-3.4.1
``` 

**SUSE Linux Enterprise Server 11**

With root or sudo permissions, run the following commands:
```
# Install as root or sudo
sudo su

# Set the package repository location containing the R Client distribution. 
zypper -ar -f http://packages.microsoft.com/sles/11/prod packages-microsoft-com

# Update packages on your system
zypper update

# Install the packages
zypper install microsoft-r-client-packages-3.4.1
``` 


You can now [set up your IDE and try out some sample code](../r-client-get-started.md).

<br>
<a name="offline"></a>

## How to install offline (without internet access) 

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or limits on internet access prevent the installer from reaching these sites, you can download individual components on a computer that has internet access, copy the files to another computer behind the firewall, manually install prerequisites and packages, and then run setup.

If you previously installed version 3.3.3, it will be replaced with the 3.4.1 version. 

Packages for all supported versions of Linux can be found at [packages.microsoft.com](https://packages.microsoft.com). 

+ Redhat 6:  https://packages.microsoft.com/rhel/6/prod/
+ Redhat 7:  https://packages.microsoft.com/rhel/7/prod/ 
+ SLES 11: https://packages.microsoft.com/sles/11/prod/ 
+ Ubuntu 14.04: https://packages.microsoft.com/ubuntu/14.04/prod/
+ Ubuntu 16.04: https://packages.microsoft.com/ubuntu/16.04/prod/ 

<a name="package-list"></a>

### Package list

The following packages comprise a full R Client installation:

```
 microsoft-r-client-packages-3.4.1     ** core
 microsoft-r-client-mml-3.4.1          ** microsoftml for R (optional)
 microsoft-r-client-mlm-3.4.1          ** pre-trained models (requires microsoftml)
```

Microsoft R Open is required for R execution:

```
 microsoft-r-open-foreachiterators-3.4.1 
 microsoft-r-open-mkl-3.4.1
 microsoft-r-open-mro-3.4.1 
```

Additional open source packages must be installed if a package is required but not found on the system. This list varies for each installation. Here is one example of the additional packages that were added to a clean RHEL image during a connected (online) setup:

```
 cairo
 fontconfig 
 fontpackages-filesystem
 graphite2 
 harfbuzz 
 libICE  
 libSM   
 libXdamage  
 libXext  
 libXfixes  
 libXft  
 libXrender  
 libXt    
 libXxf86vm  
 libicu      
 libpng12   
 libthai
 libunwind     
 libxshmfence   
 mesa-libEGL
 mesa-libGL     
 mesa-libgbm    
 mesa-libglapi 
 pango   
 paratype-pt-sans-caption-fonts  
 pixman  
```

### Download packages

If your system provides a graphical user interface, you can click a file to download it. Otherwise, use `wget`. We recommend downloading all packages to a single directory so that you can install all of them in a single command. By default, `wget` uses the working directory, but you can specify an alternative path using the `-outfile` parameter.

The following example is for the first package. Each command references the version number of the platform. Remember to change the number if your version is different. For more information, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

+ Download to CentOS or RHEL 6: `wget https://packages.microsoft.com/rhel/6/prod/microsoft-r-client-packages-3.4.1.rpm` 
+ Download to CentOS or RHEL 7: `wget https://packages.microsoft.com/rhel/7/prod/microsoft-r-client-packages-3.4.1.rpm` 
+ Download to SUSE: `wget https://packages.microsoft.com/sles/11/prod/microsoft-r-client-packages-3.4.1.rpm`
+ Download to Ubuntu 14.04: `wget https://packages.microsoft.com/prod/ubuntu/14.04/microsoft-r-client-packages-3.4.1.deb`
+ Download to Ubuntu 16.04: `wget https://packages.microsoft.com/prod/ubuntu/16.04/microsoft-r-client-packages-3.4.1.deb`

Repeat for each package.

### Install packages

The package manager determines the installation order. Assuming all packages are in the same folder:

+ Install on CentOS or RHEL: `yum install *.rpm` 
+ Install on Ubuntu: `dpkg -i *.deb`
+ Install on SUSE: `zypper install *.rpm`

This step completes installation.

### Connect and validate

1. List installed packages:

  + On CentOS and RHEL: `rpm -qa | grep microsoft` 
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SLES11: `zypper se microsoft`

2. Once you have a package name, you can obtain verbose version information. For example:

   + On CentOS and RHEL: `$ rpm -qi microsoft-r-client-packages-3.4.1`
   + On Ubuntu: `$ dpkg --status microsoft-r-client-packages-3.4.1`  
   + On SLES: `zypper info microsoft-r-client-packages-3.4.1`

  Output on Ubuntu is as follows:

   ```
    Package: microsoft-r-client-packages-3.4.1
    Status: install ok installed
    Priority: optional
    Section: devel
    Installed-Size: 195249
    Maintainer: revobuil@microsoft.com
    Architecture: amd64
    Version: 3.4.1
    Depends: microsoft-r-open-mro-3.4.1, microsoft-r-open-mkl-3.4.1, microsoft-r-open-foreachiterators-3.4.1
    Description: Microsoft R Client 3.4.1
	  . . .
   ```

You can now [set up your IDE and try out some sample code](../r-client-get-started.md). Also, consider package management as described in the next section.

### Offline Package Management

Review the recommendations in [Package Management](../operationalize/configure-manage-r-packages.md#offline) for instructions on how to set up a local package repository using MRAN or miniCRAN. As we mentioned earlier, you must install the `gcc-c++` and `gcc-gfortran` binary packages to be able to build and install packages, including miniCRAN.


## How to uninstall R Client 3.4.1

1. On root@, uninstall Microsoft R Open (MRO) first. This action removes any dependent packages used only by MRO, which includes packages like microsoft-mlserver-packages-r. 

  + On RHEL: `yum erase microsoft-r-open-mro-3.4.1`     
  + On Ubuntu: `apt-get purge microsoft-r-open-mro-3.4.1`  
  + On SUSE: `zypper remove microsoft-r-open-mro-3.4.1`    

2. Re-list the packages from Microsoft to check for remaining files:

  + On RHEL: `yum list \*microsoft\*`   
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SUSE: `zypper search \*microsoft-r\*`  

  On Ubuntu, you might have `dotnet-sharedframework-microsoft.netcore.app-1.1.2`. [NET Core](https://docs.microsoft.com/dotnet/core/index) is a cross-platform, general purpose development platform maintained by Microsoft and the .NET community on GitHub. This package could be providing infrastructure to other applications on your computer. If R Client is the only Microsoft software you have, you can remove it now.

6. After packages are uninstalled, remove remaining files. On root@, determine whether additional files still exist:

  + `$ ls /opt/microsoft/rclient/3.4.1/`

7. Remove the entire directory:

  + `$ rm -fr ls /opt/microsoft/rclient/3.4.1/`

RM removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft/rclient. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.


## Learn More

You can learn more with these guides:

+ [Overview of Microsoft R Client](../r-client-get-started.md) 

+ [Quickstart: Running R code in Microsoft R](../r/quickstart-run-r-code.md) (example)

+ [How-to guides in Machine Learning Server](../r/how-to-introduction.md)

+ [RevoScaleR R package reference](../r/tutorial-introduction.md)

+ [MicrosoftML R package reference](../r-reference/microsoftml/microsoftml-package.md)

+ [mrsdeploy R package reference](../r-reference/mrsdeploy/mrsdeploy-package.md)

+ [Execute code on remote Machine Learning Server](../r/how-to-execute-code-remotely.md)
