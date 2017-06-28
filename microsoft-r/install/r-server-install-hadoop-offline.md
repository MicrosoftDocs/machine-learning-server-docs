---

# required metadata
title: "Offline install for Microsoft R Server 9.1.0 for Hadoop"
description: "How to install R Server on Hadoop without an internet connection"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/10/2017"
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

# Offline installation of R Server 9.1 for Hadoop

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or limits on internet access prevent the installer from reaching these sites, you can download individual components on a computer that has internet access, copy the files to another computer behind the firewall, manually install prerequisites and packages, and then run setup.

If you previously installed version 9.0.1, it will be replaced with the 9.1.0 version. An 8.x version can run side-by-side 9.x, unaffected by the new installation.

<a name="download"><a/>

## Download R Server dependencies

From an internet-connected computer, download Microsoft R Open (MRO) and .NET Core for Linux. 

MRO provides the R distribution (base R language and script support) used by R Server. R Server setup checks for this specific distribution and won't use alternative distributions. MRO can co-exist with other distributions of R on your machine, but additional configuration could be required to make a particular version the default. For more information, see [Manage an R Server installation on Linux](../rserver-install-linux-manage-install.md).

The .NET Core component is required for MicrosoftML (machine learning). It is also required for mrsdeploy, used for remote execution, web service deployment, and configuration of R Server as web node and compute node instances.

| Component | Version | Download Link | Notes |
|-----------|---------|---------------|-------|
| Microsoft R Open | 3.3.3 | [Direct link to microsoft-r-open-3.3.3.tar.gz](https://go.microsoft.com/fwlink/?linkid=845297) | Use the link provided to get the required component. Do NOT go to MRAN and download the latest or you could end up with the wrong version. |
| Microsoft .NET Core | 1.1 | [.NET Core download site](https://www.microsoft.com/net/download/linux) | Multiple versions of .NET Core are available. Be sure to choose from the 1.1.1 (Current) list. |

The .NET Core download page for Linux provides gzipped tar files for supported platforms. In the Runtime column, click **x64** to download a tar.gz file for the operating system you are using. The file name for .NET Core is `dotnet-<linux-os-name>-x64.1.1.1.tar.gz`.

## Download R Server installer

You can get Microsoft R Server (MRS) 9.1.0 for Hadoop from one of the following download sites. 

| Site | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios. <br/><br/>1. Click **Join or Access Now** and enter your account information.<br/>2. Make sure you're in the right place: *my.visualstudio.com*.<br/>3. Click **Downloads**, and then search for *Microsoft R*. |
|[Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for "SQL Server 2016 Enterprise edition", and then choose a per-core or CAL licensing option. A selection for **R Server for Hadoop 9.1.0** is provided on this site. |
| [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) | Developer or Enterprise | Subscribers can download software at given subscription levels. Depending on your subscription, you can get either edition. |

## Download package dependencies

R Server has package dependencies for various platforms. The list of required packages can be found at [Package dependencies for Microsoft R Server](r-server-install-linux-hadoop-packages.md). If the target system is missing any, download the ones you will need.

You can list existing packages in /usr/lib64 to see what is currently installed. It's common to have a very large number of packages. You can do a partial string search to filter on specific filenames (such as lib* for files starting with lib.):  `ls -l /usr/lib64/lib*`

> [!Note]
> It's possible your Linux machine already has package dependencies installed. By way of illustration, on a few systems, only libpng12 had to be installed.

## Transfer files

Use a tool like [SmarTTY](http://smartty.sysprogs.com/download/) or [PuTTY](http://www.putty.org) or another mechanism to upload or transfer files to a writable directory, such as **/tmp**, on your internet-restricted server. Files to be transferred include the following:

+ `dotnet-<linux-os-name>-x64.1.1.1.tar.gz`
+ `microsoft-r-open-3.3.3.tar.gz`
+ `en_microsoft_r_server_910_for_hadoop_x64_10323951.tar.gz`
+ any missing packages from the dependency list

## Install package dependencies

On the target system which is disconnected from the internet, run `rpm -i <package-name>` to install any packages that are missing from your system (for example, `rpm -i libpng12-1-2-50-10.el7.x86_64.rpm`).

## Unpack .NET Core and set symbolic link

For an offline installation of .NET Core, manually create its directory path, unpack the distribution, and then set references so that the component can be discovered by other applications.

1. Log in as root or as a user with super user privileges (`sudo -s`).

2. Switch to the **/tmp** directory (assuming it's the download location) and execute `ls` to list the files as a verification step. You should see the tar.gz files you transferred earlier.

3. Make a directory for .NET Core:

  `[root@localhost tmp] $ mkdir /opt/dotnet`

4. Unpack the .NET Core redistribution into the /opt/dotnet directory:

  `[root@localhost tmp] $ tar zxvf dotnet-<linux-os-name>-x64.1.1.tar.gz -C /opt/dotnet`

5. Set the symbolic link for .NET Core to user directories:

  `[root@localhost tmp] $ ln -s /opt/dotnet/dotnet /usr/bin/dotnet`

## Unpack MRS distribution and copy MRO

Next, unpack the R Server distribution and copy the gzipped MRO distribution to the MRS91Hadoop folder.

> [!Important]
> Do not unpack MRO yourself. The installer looks for a gzipped tar file for MRO. 

1. Unpack the MRS gzipped file. 

  `[root@localhost tmp] $ tar zxvf en_microsoft_r_server_910_for_hadoop_x64_10323951.tar.gz`

2. A new folder called MRS91Hadoop is created under /tmp. This folder contains files and packages used during setup. Copy the gzipped MRO tar file to the new MRS91Hadoop folder containing the installation script (install.sh).

  `[root@localhost tmp] $ cp microsoft-r-open-3.3.3.tar.gz /tmp/MRS91Hadoop`

## Run the MRS install script

R Server for Hadoop is deployed by running the install script with no parameters. At this point, you could opt for [unattended install](#unattended) to bypass EULA prompts.

1. Switch to the `MRS91Hadoop` directory containing the installation script:

  `[root@localhost tmp] $ cd MRS91Hadoop`

2. Run the script with the **-p** parameter, specifying the Hadoop component. Optionally, add the pretrained machine learning models.

   `[root@localhost MRS91Hadoop] $ bash install.sh -h -m`

3. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

4. Repeat EULA acceptance for Microsoft R Server.

Installer output shows the packages and location of the log file.

## Verify installation

1. List installed packages and get package names:

   `[MRS91Hadoop] $ yum list \*microsoft\*`

2. Check the version of Microsoft R Open using `rpm -qi`:

   `[MRS91Hadoop] $ rpm -qi microsoft-r-open-mro-3.3.3.x86_64`

3. Check the version of Microsoft R Server:

   `[MRS91Hadoop] $ rpm -qi microsoft-r-server-packages-9.1.0.x86_64`

4. Partial output is as follows (note version 9.1.0):

	 Name        : microsoft-r-server-packages-9.0     Relocations: /usr/lib64
	 Version     : 9.1.0                               Vendor: Microsoft
	 . . .

## Start Revo64

As a verification step, run the Revo64 program.

1. Switch to the directory containing the executable:

   `$ cd MRS91Hadoop`

2. Start the program:

   `$ Revo64`

3. Run an R function, such as **rxSummary** on a dataset. Many sample datasets, such as the iris dataset, are ready to use because they are installed with the software:

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

The server can be used as-is if you install and use an R IDE on the same box, but to benefit from the deployment and consumption of web services with Microsoft R Server, then you must configure R Server after installation to act as a deployment server and host analytic web services. Possible configurations are a [one-box setup](operationalize-r-server-one-box-config.md) or an [enterprise setup](operationalize-r-server-enterprise-config.md). Doing so also enables remote execution, allowing you to connect to R Server from an R Client workstation and execute code on the server.

## Next Steps

Review the following walkthroughs to move forward with using R Server and the RevoScaleR package in Spark and MapReduce processing models.

+ [Practice data import and exploration on Spark](../scaler-spark-getting-started.md)
+ [Practice data import and exploration on MapReduce](../scaler-hadoop-getting-started.md)

Review the best practices in [Manage your R Server for Linux installation](../rserver-install-linux-manage-install.md) for instructions on how to set up a local package repository using MRAN or miniCRAN, change file ownership or permissions, set Revo64 as the de facto R script engine on your server.

## See Also

[Supported platforms](../rserver-install-supported-platforms.md)

[What's new in R Server](../notes/r-server-notes.md)


