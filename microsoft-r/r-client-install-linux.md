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

To benefit from disk scalability, performance and speed, you can push the compute context using rxSetComputeContext() to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility.](r-client-compatibility.md)  You can also run your code remotely on R Server using [remoteLogin() or remoteLoginAAD()](/operationalize/remote-execution.md) from the mrsdeploy package to offload heavy processing on server or to test your analytics during their development. You can also run your code remotely on R Server using [remoteLogin() or remoteLoginAAD()](/operationalize/remote-execution.md) from the mrsdeploy package to offload heavy processing on server or to test your analytics during their development.


## System Requirements

|   |   |
| - | - |
|Operating Systems|Supported versions include:<br>- Red Hat Enterprise Linux (RHEL) and CentOS 6.x and 7.x<br>- Ubuntu 14.04 and 16.04<br>- SUSE Linux Enterprise Server 11 (SLES11)<br>Must be a supported version of Linux on a 64-bit with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.|
|Free disk space|600 MB recommended, after installation of all prerequisites<br>1.2 GB recommended if pretrained models are installed|
|Available RAM|2 GB of RAM is required; 8 GB or more are recommended|
|Internet access|Needed to download R Client and any dependencies. If you do not have an internet connection, for the instructions for an [offline installation](#offline)|


The following additional components are included in Setup and required for R Client.

* Microsoft R Open 3.3.3
@@@* Microsoft .NET Core 1.1 for Linux (required for mrsdeploy and MicrosoftML use cases)

## @@Setup Requirements

+ A package manager (yum for RHEL systems, zypper for SLES systems)

+ Root or super user permissions

+ You must install Microsoft R Client to a local drive on your computer.

+ You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.


## What's Installed with R Client

The Microsoft R Client setup installs the R base packages and a set of enhanced and proprietary R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop. The R libraries are installed under the R Client installation directory, @@`C:\Program Files\Microsoft\R Client\R_SERVER`. Additionally, in this directory you will find documentation for the R base packages, sample data, and the R library.

All of tools for the standard base R (RTerm, Rgui.exe, and RScript) are also included with Microsoft R Client under `<install-directory>\bin`. Documentation for these tools can be found in the setup folder: `<install-directory>\doc` and in `<install-directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options.

> [!NOTE]
> By default, telemetry data is collected during your usage of R Client. To turn this feature off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.


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

This version cannot co-exist with the previous major minor version. The install script automatically replaces previous minor versions, including the preview version of .NET Core, if they are detected so that setup can proceed.

### Download R Client and its dependencies

From an internet-connected computer, download the following:

1. Download Microsoft R Client for Linux from http://aka.ms/rclientlinux.

1. Download Microsoft R Open, the R distribution used by R Client, as well as .NET Core for Linux. The .NET Core component is required for MicrosoftML (machine learning) and mrsdeploy packages.

   | Component | Version | Download Link |
   |-----------|---------|---------------|
   | Microsoft R Open | 3.3.3 | https://go.microsoft.com/fwlink/?linkid=845297 |
   | Microsoft .NET Core | 1.1 | [.NET Core download site](https://www.microsoft.com/net/download/linux) |

   The file name for Microsoft R Open is `microsoft-r-open-3.3.3.tar.gz`. 

   The .NET Core download page for Linux provides gzipped tar files for supported platforms. In the Runtime column, click **x64** to download a tar.gz file for the operating system you are using. 

   > [!Note]
   > Multiple versions of .NET Core are available. Be sure to choose from the 1.1.1 (Current) list.

1. @@ DOES THIS APPLY??? On certain platforms, also download package dependencies. The list of [required packages are the same as those for Microsoft R Server 9.1.0](rserver-install-linux-hadoop-packages.md). If the target system is missing any, download the ones you will need.

   > [!Note]
   > It's possible your Linux machine already has package dependencies installed. By way of illustration, on a few systems, only libpng12 had to be installed.

   To see what is currently installed, list the existing packages in /usr/lib64. It's common to have a very large number of packages. You can do a partial string search to filter on specific filenames (such as lib* for files starting with lib.)

   `ls -l /usr/lib64/lib*`

1. Transfer these files to the machine without any internet connection.

   You can use a tool like [SmarTTY](http://smartty.sysprogs.com/download/) or [PuTTY](http://www.putty.org) or another mechanism to transfer downloaded files to a writable directory, such as **/tmp**, on your internet-restricted server. 
   
   Files to be transferred include the following:

   + `dotnet-<linux-os-name>-x64.1.1.1.tar.gz`
   + `microsoft-r-open-3.3.3.tar.gz`
   + `microsoft-r-client-3.3.3.tar.gz`
   + any missing packages from the dependency list

### Prepare the downloaded files for installation

On the target system which is disconnected from the internet:

1. Be sure you have put the downloaded files into a writable directory, such as **/tmp**.

1. Install any missing package dependencies. Run the following command to install any packages that are missing from your system.
   `rpm -qi <package-name>`

1. Do not unpack Microsoft R Open. Instead, copy the gzipped tar file to the MRC_Linux directory after you've unpacked Microsoft R Client. 

1. Unpack the distributions for .NET Core and Microsoft R Client.
   You should unpack the file as follows:

   1. Log in as root or a user with super user privileges (`sudo su`). The following instructions assume user privileges with the sudo override.

   1. Switch to the **/tmp** directory (assuming it's the download location).

   1. Unpack the .NET Core redistribution, and set the symbolic link for .NET Core to make it accessible to R Client as follows. .NET Core is unpacked into **/shared/Microsoft.NETCore.App/1.1.1**.

      `[root@localhost tmp] $ tar zxvf dotnet-<linux-os-name>-x64.1.1.tar.gz`

      `[root@localhost tmp] $ ln -s /path/to/dotnet-core/dotnet /usr/local/bin/dotnet`

   1. Unpack the Microsoft R Client gzipped file as follows. Files are unpacked into  **MRC_Linux**. If you list the contents, you will see licensing documents, subfolders, and scripts.

      `[root@localhost tmp] $ tar zxvf microsoft-r-client-3.3.3.tar.gz`

1. Copy the gzipped tar file for Microsoft R Open to the MRC_Linux directory.  The install.sh script file for R Client looks for the gzipped tar file for Microsoft R Open. Assuming root permissions, copy the gzipped Microsoft R Open tar file to the same folder containing the installation script.

  `[root@localhost tmp] $ cp microsoft-r-open-3.3.3.tar.gz /tmp/MRC_Linux`

### Run the Microsoft R Client install script

R Client for Linux is deployed by running the install script with no parameters.

1. Switch to the `MRC_Linux` directory containing the installation script:

  `[root@localhost tmp] $ cd MRC_Linux`

2. Run the script.

   `[root@localhost MRC_Linux] $ bash install.sh`

3. When prompted to accept the license terms for Microsoft R Client, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

4. Installer output shows the packages and location of the log file. You can now [verify the installation](#verify).


<a name="verify"></a>

## Verify the installation

1. List installed packages and get package names:

   `[tmp MRC_Linux] $ yum list \*microsoft\*`

1. Check the version of Microsoft R Open using `rpm -qi`:

   `[tmp MRC_Linux] $ rpm -qi microsoft-r-open-mro-3.3.x86_64`

1. Check the version of Microsoft R Client:
   @@
   `[tmp MRC_Linux] $ rpm -qi microsoft-r-server-packages-9.1.x86_64`

1. Partial output is as follows (note version 9.1.0):
   @@
   ```
	 Name        : microsoft-r-server-packages-9.1     Relocations: /usr/lib64
	 Version     : 9.1.0                               Vendor: Microsoft
	 . . .
   ```

1. Verify that Revo64 can start.

   `[tmp MRC_Linux] $ Revo64`

3. Run an R function, such as **rxSummary** on a dataset. Many sample datasets, such as the Iris Dataset, are ready to use because they are installed with the software:

   ```
   > rxSummary(~., iris)`
   ```

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

## Unattended installs

You can bypass the interactive install steps of the Microsoft R Client install script with the `-y` flag. Additional flags are available, as follows:

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



<a name="offline"></a>

## How to offline install (without internet access)

1. On a machine with _**unrestricted**_ internet access:

   + Download Microsoft R Client from the following link: http://aka.ms/rclient/

   + Download the Microsoft R Open ( *.cab) needed to install R Client from the following link: https://go.microsoft.com/fwlink/?LinkId=834568&clcid=1033

   + Download the prerequisites, including the .NET Framework and other components previously listed.

   + Copy the .cab file, component executables, and R Client installer to a network share or portable drive.

1. On the machine with _**restricted**_ internet access:

   + Log in with administrator privileges.

   + Copy the .cab file, component executables, and R Client installer from the network share/portable drive on the first machine to the machine that has restricted internet access. Put those files under `%temp%`. 

   + Install the prerequisites first. 
   
   + Restart your computer is you installed the .NET Framework.

   + Run `RClientSetup.exe`, which then finds the cab file in the temp folder.
   
   + Follow the onscreen prompts.

1. Without internet access, we recommend disabling the _auto-update check_ feature so that R Client can launch more quickly. Do so in the `Rprofile.site` file by adding a comment character (#) at the start of the line: `mrupdate::mrCheckForUpdates()`
 


## Remove Microsoft R Client

This section explains how to uninstall Microsoft R Client on Linux. 

## Program version and file locations

As a first step, use **yum** and **rpm** to check for program version and file locations.

List installed packages and get package names:

- `yum list \*microsoft\*`

Get version information:

- `rpm -qi microsoft-r-open-mro-3.3.x86_64`
- `rpm -qi microsoft-r-server-packages-9.1.x86_64`



## Learn More

You can learn more with these guides:

+ [Get Started with Microsoft R Client](r-client-get-started.md) 

+ [Microsoft R Getting Started](microsoft-r-getting-started.md) 

+ [Diving into Data Analysis with Microsoft R](data-analysis-in-microsoft-r.md)

+ [Get started with RevoScaleR](microsoft-r-get-started-node.md)

+ [Get started with MicrosoftML](microsoftml-get-started.md)

+ [Get started with mrsdeploy](mrsdeploy/mrsdeploy.md)
