---

# required metadata
title: "Offline install for Microsoft R Server for Linux"
description: "How to install R Server on Linux without an internet connection"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/02/2017"
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

# Offline installation instructions for R Server for Linux

By default, installers connect to Microsoft download sites to get required and updated components. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can download individual components on a computer that has internet access, copy the files to another computer behind the firewall, manually install each component, and then run setup.

## Download packages

Using an internet-connected computer, download the packages listed in [Package dependencies for Microsoft R Server 9.0.1](rserver-install-linux-hadoop-packages.md). 

## Download R Open installer

Get Microsoft R Open (MRO) 3.3.2 from the [MRAN web site](https://mran.microsoft.com/download/), choosing the distribution for your Linux operating system. The filename is `microsoft-r-open-3.3.2.tar.gz`.

## Download R Server installer

Get Microsoft R Server (MRS) 9.0.1 for Linux from one of the following locations. The filename is `en_r_server_901_for_linux_x64_9648602.gz`. 

**Option 1: [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx)**

Subscribers can download software at given subscription levels. Depending on your subscription, you can get the developer or enterprise edition.

**Option 2: [Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409)** 

This option provides the enterprise edition. Sign in, search for "SQL Server 2016 Enterprise edition", and then choose a per-core or CAL licensing option. A selection for **R Server for Windows 9.0.1** is provided on this site.**

**Option 3: [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409)** 

This option provides a zipped file, free to developers who sign up for Visual Studio Dev Essentials. This is the Developer edition of Microsoft R Server; it has the same features as Enterprise except it is licensed for development scenarios.

1. Click **Join or Access Now** and enter your account information.
2. Make sure you're in the right place. The URL should start with *my.visualstudio.com*.
3. Click **Downloads**, and then search for *Microsoft R*.

## Transfer files to the target server

Use a flash drive or another mechanism to transfer downloaded packages from the dependency list, `microsoft-r-open-3.3.2.tar.gz`, and `en_r_server_901_for_linux_x64_9648602.gz` to your disconnected server. Copy the files to a writable directory, such as **/tmp**.

## Manually install package dependencies

Use `rpm -qi <package-name>` to install any packages that are missing from your system.

## Unpack the distribution

Next, unpack the Microsoft R distributions, starting with MRO, followed by MRS.

1. Log in as root or a user with sudo privileges.

2. Switch to the **/tmp** directory (assuming it's the download location).

3. Unpack the MRO gzipped file:

  `[tmp] $ tar zxvf microsoft-r-open-3.3.2.tar.gz`

4. Unpack the MRS gzipped file:

  `[tmp] $ tar zxvf en_r_server_901_for_linux_x64_9648602.gz`

## Check files

Files are unpacked into child folders: **microsoft-r-open** and  **MRS90Linux**. If you list the contents, you will see licensing documents, subfolders, and an install script (install.sh)

## Run the MRO install script

R Open is deployed by running the install script with no parameters.

1. Log in as root or a user with sudo privileges (`sudo su`). The following instructions assume user privileges with the sudo override.

2. Verify system repositories are up to date:

  `[username] $ sudo yum clean all`

3. Change to the directory to which you downloaded the rpm (for example, **/tmp**):

  `[username] $ cd /tmp`

4. Change to the `microsoft-r-open` directory containing the installation script:

  `[tmp] $ cd microsoft-r-open`

5. Run the script.

  `[microsoft-r-open] $ sudo bash install.sh`

6. When prompted to accept the license terms for Microsoft R Open, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

7. Repeat to accept license terms for the Intel MKL libraries, and to start the installation.

When finished, the installer output shows the packages and location of the log file.

## Run the MRS install script

R Server for Linux is deployed by running the install script with no parameters.

1. Change to the `MRS90LINUX` directory containing the installation script:

  `[tmp] $ cd ..\MRS90LINUX`

2. Run the script.

   `[MRS90LINUX] $ sudo bash install.sh`

3. When prompted to accept the license terms for Microsoft R Server, click Enter to read the EULA, click **q** when you are finished reading, and then click **y** to accept the terms.

4. Installer output shows the packages and location of the log file.

## Verify installation

1. List installed packages and get package names:

   `[MRS90LINUX] $ yum list \*microsoft\*`

   `[MRS90LINUX] $ yum list \*deployr\*`

2. Check the version of Microsoft R Open using `rpm -qi`:

   `[MRS90LINUX] $ rpm -qi microsoft-r-open-mro-3.3.x86_64`

3. Check the version of Microsoft R Server:

   `[MRS90LINUX] $ rpm -qi microsoft-r-server-packages-9.0.x86_64`

4. Partial output is as follows (note version 9.0.1):

	 Name        : microsoft-r-server-packages-9.0     Relocations: /usr/lib64
	 Version     : 9.0.1                               Vendor: Microsoft
	 . . .

## Start Revo64

As a verification step, run the Revo64 program.

1. Switch to the directory containing the executable:

   `$ cd MRS90LINUX`

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

## Configure R Server for Operationalization

To benefit from Microsoft R Server’s deployment and operationalization features, you can [configure R Server for operationalization](operationalize/configuration-initial.md) after installation to act as a deployment server and host analytic web services. Doing so will enable you to operationalize your R code.

## See Also

[Run R Server for Windows](rserver-install-windows.md)

[Supported platforms](rserver-install-supported-platforms.md)

[What's new in R Server](notes/r-server-notes.md)

[Microsoft R Getting Started Guide](microsoft-r-getting-started.md)

[Configure R Server for Operationalization](operationalize/configuration-initial.md)
