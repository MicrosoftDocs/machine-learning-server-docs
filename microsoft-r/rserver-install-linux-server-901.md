---

# required metadata
title: "R Server 9.0.1 installation for Linux systems"
description: "Install Microsoft R Server 9.0.1 on Linux."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/07/2016"
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

# R Server 9.0.1 Installation for Linux Systems

Older versions of R Server for Linux are no longer available on the Microsoft download sites, but if you already have an older distribution, you can follow these instructions to deploy version 8.0.5. For the current release, see [Install R Server for Linux](rserver-install-linux-server.md).

**Side-by-side Installation**

You can install major versions of R Server (such as an 8.x and 9.x) side-by-side on Linux, but not minor versions. For example, if you already installed Microsoft R Server 8.0, you must uninstall it before you can install 8.0.5.

**Upgrade Versions**

If you want to replace an older version rather than run side-by-side, you can uninstall the older distribution before installing the new version (there is no in-place upgrade). See [Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md) for instructions.

**Requirements**

Installer requirements include the following:

-   An internet connection
-   A package manager (yum for RHEL systems, zypper for SLES systems)
-   Root or as super user permissions

If these requirements cannot be met, you can install R Server manually. First, verify that your system meets system requirements and satisfies the [package prerequisites](rserver-install-linux-hadoop-packages.md). You can then follow the more detailed installation instructions described in [Managing Your Microsoft R Server Installation](#manage-installation).


## System Requirements

**Processor:** 64-bit processor with x86-compatible architecture (variously known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips). Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

**Operating System:** Microsoft R for Linux can be installed on Red Hat Enterprise Linux (RHEL) or a fully compatible operating system like CentOS, or SUSE Linux Enterprise Server. Microsoft R Server has different operating system requirements depending on the version you install. See [Supported platforms](rserver-install-supported-platforms.md) for specifics. Only 64-bit operating systems are supported.

**Memory:** A minimum of 2 GB of RAM is required; 8 GB or more are recommended.

**Disk Space:** A minimum of 500 MB of disk space is required.

## Unpack the distribution

Download the software to a writable directory, such as **/tmp**, unpack the distribution and then run the installation script.

The distribution includes one installer for Microsoft R Server. For a gzipped TAR file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as **/tmp**):

1. Log in as root or a user with sudo privileges.

2. Switch to the **/tmp** directory (assuming it's the download location)

3. Unpack the file:

  `[tmp] $ tar zxvf en_r_server_901_for_linux_x64_9648602.gz`

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

## Configure R Server to operationalize your analytics

The server can be used as-is if you install and use an R IDE on the same box, but to benefit from the deployment and consumption of web services with Microsoft R Server, then you must configure R Server after installation to act as a deployment server and host analytic web services. Possible configurations are a [one-box setup](install/operationalize-r-server-one-box-config.md) or an [enterprise setup](install/operationalize-r-server-enterprise-config.md). Doing so also enables remote execution, allowing you to connect to R Server from an R Client workstation and execute code on the server.

<a name="manage-installation"></a>
## Manage your installation

In this section, we discuss file management for your Microsoft R Server installation, including file ownership, file permissions, and so on.

### File ownership

If followed the instructions provided, the installed files are all owned by root. For single-user workstations where the user has either sudo privileges or access to the root password, this is normally fine. In enterprise environments, however, it's common to have third-party applications such as Microsoft R Server installed into an account owned by a non-root user; this can make maintenance easier and reduce security concerns. In such an environment, you may wish to create an "RUser" account, and change ownership of the files to that user. You can do that as follows:

1. Install Microsoft R Server as root, as usual.
2. Create the "RUser" account if it does not already exist. Assign this user to a suitable group, if desired.
3. Use the **chown** command to change ownership of the files (in the example below, we assume RUser has been made a member of the dev group; this command requires root privileges):

		chown -R RUser:dev /usr/lib64/MRS90LINUX

Here we show the default path /usr/lib64/MRS90LINUX; if you have specified an alternate installation path, use that in this command as well.

### Unattended installs

You can bypass the interactive install steps of the Microsoft R Server install script with the -y flag ("yes" or "accept default" to all prompts except that you also agree to the license agreement). Additional flags can be used to specify which of the usual install options you want, as follows:

flag | Option | Description
-----|--------|------------
 -a | --accept-eula | Accept all end user license agreements.
 -d | --download-mro |  Download microsoft r open for distribution to an offline system.
 -p | --hadoop-components | Install Hadoop components.
 -s | --silent | Perform a silent, unattended install.
 -u | --unattended | Perform an unattended install.
 -h | --help | Print this help text.

For a standard unattended install, run the following script:

	./install.sh –a –s

### File permissions

Normally, ordinary Microsoft R Server files are installed with read/write permission for owner and read-only permission for group and world. Directories are installed with execute permission as well, to permit them to be traversed. You can modify these permissions using the **chmod** command. (For files owned by root, this command requires root privileges.)

### Installing to a read-only file system

In enterprise environments, it is common for enterprise utilities to be mounted as read-only file systems, so that ordinary operations cannot corrupt the tools. Obviously, new applications can be added to such systems only by first unmounting them, then re-mounting them for read/write, installing the software, and then re-mounting as read-only. This must be done by a system administrator.

## Setting up a package repository

One of the strengths of the R language is the thousands of third-party packages that have been made publicly available via CRAN, the Comprehensive R Archive Network. R includes a number of functions that make it easy to download and install these packages. However, in many enterprise environments, access to the Internet is limited or non-existent. In such environments, it is useful to create a local package repository that users can access from within the corporate firewall.

Your local repository may contain source packages, binary packages, or both. If some or all of your users will be working on Windows systems, you should include Windows binaries in your repository. Windows binaries are R-version-specific; if you are running R 3.2.2, you need Windows binaries built under R 3.2. These versioned binaries are available from CRAN and other public repositories. If some or all of your users will be working on Linux systems, you must include source packages in your repository.

The Microsoft Managed R Archive Network (MRAN) provides daily snapshots of all of CRAN, and together with the Microsoft-developed open-source package miniCRAN can be used to easily build a local package repository containing source packages, binary packages, or both.

There are two ways to create the package repository: either copy all the packages from a given MRAN snapshot, or create a new repository and populate it with just those packages you want to be available to your users. We will describe both procedures using MRAN and miniCRAN.

**Note:** The miniCRAN package itself is dependent on 18 other CRAN packages, among which is the RCurl package, which has a system dependency on the curl-devel package. Similarly, package XML has a dependency on libxml2-devel. We recommend, therefore, that you build your local repository initially on a machine with full Internet access, so that you can easily satisfy all these dependencies. After created, you can either move the repository to a different location within your firewall, or simply disable the machine’s Internet access.

### Installing miniCRAN

On a system with Internet access, the easiest way to install the miniCRAN package (or any R package) is to start R and use the install.packages function:

	install.packages("miniCRAN", dependencies=TRUE)

If your system already contains all the system prerequisites, this will normally download and install all of miniCRAN’s R package dependencies as well as miniCRAN itself. If a system dependency is missing, compilation of the first package that needs that dependency will fail, typically with a specific but not particularly helpful message. In our testing, we have found that an error about curl-config not being found indicates that the curl-devel package is missing, and an error about libxml2 indicates the libxml2-devel package is missing. If you get such an error, exit R, use yum or zypper to install the missing package, then restart R and retry the install.packages command.

### Create a repository from an MRAN snapshot

Creating a repository from an MRAN snapshot is very straightforward:

1. Choose a location for your repository somewhere on your local network. If you have a corporate intranet, this is usually a good choice, provided URLs have the prefix http:// and not https://. Any file system that is mounted for all users can be used; file-based URLs of the form file:// are supported by the R functions. In this example, we suppose the file system /local is mounted on all systems and we will create our repository in the directory /local/repos.

2. Start R and load the miniCRAN package:

		library(miniCRAN)

3. Specify an MRAN snaphot:

		CRAN <- "http://mran.revolutionanalytics.com/snapshot/2015-11-30"

4. Set your MRAN snapshot as your CRAN repo:

		r <- getOption("repos")
		r["CRAN"] <- CRAN
		options(repos=r)

5. Use miniCRAN’s pkgAvail function to obtain a list of (source) packages in your MRAN snapshot:

		pkgs <- pkgAvail()[,1]

6. Use miniCRAN’s makeRepo function to create a repository of these packages in your /local/repos directory:

		makeRepo(pkgs, "/local/repos", type="source")

### Create a custom repository

As mentioned above, a custom repository gives you complete control over which packages are available to your users. Here, too, you have two basic choices in terms of populating your repository: you can either use makeRepo to select specific packages from an existing MRAN snapshot, or you can combine your own locally developed packages with packages from other sources. The latter option gives you the greatest control, but typically means you need to manage the contents using home-grown tools.

#### Create a custom package directory

Your custom package directory can contain source packages, binary packages, or both. Windows binary packages should have a path of the form

	/local/repos/bin/windows/contrib/3.2

while source packages should have a path of the form:

	local/repos/src/contrib

Windows binary packages should be built with your current R version. Windows binary files will be zip files, package source files will be tar.gz files. Move the desired packages to the appropriate directories, then run the R function write\_PACKAGES in the tools package to create the PACKAGES and PACKAGES.gz index files:

	tools:::write_PACKAGES("/local/repos")

### Configure R to use a local repository

To make your local repository available to your R users:

1.  Open the file Rprofile.site in the /etc directory of your installed R (if you installed to the default /usr prefix, the path is /usr/lib64/MRO-for-MRS-8.0.0/R-3.2.2/lib64/R/etc/Rprofile.site).

2.  Find the following line in the Rprofile.site file:

		r <- c(REVO=RevoUtils::getRevoRepos())

3.  Add your repository as follows:

		r <- c(REVO=RevoUtils::getRevoRepos(),
			LOCAL="file:///local/repos")

	 If you added more than one repository and used R version numbers, add multiple lines of the form LOCAL\_VER="file:///local/repos/VER", for example:

		r <- c(REVO=RevoUtils::getRevoRepos(),
		       LOCAL_3.2="file:///local/repos/3.2",
		       LOCAL_3.1="file:///local/repos/3.1",
		       LOCAL_3.0="file:///local/repos/3.0")

4.  Save the file and your repository is ready for use.

If you are in a locked-down environment without access to the standard Microsoft repository, *replace* the Revo repository with your local repository (or repositories).

## Remove Microsoft R Server

For instructions on rolling back your installation, see
[Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md).

## Manage multiple R installations

You can install the latest Microsoft R Server side-by-side with an existing Microsoft R Server. In this section, we discuss maintaining multiple versions of Microsoft R Server, together with instructions for maintaining Microsoft R Server alongside versions of R obtained from CRAN or third-party repositories such as <http://dl.fedoraproject.org/pub/epel>. (Note, however, that patch releases such as Microsoft R Server 7.4.1 are intended to replace their associated main release, not live side by side with it. You must uninstall and reinstall such releases.)

### Using Microsoft R Server with other R installations

If you have an installed R script in your /usr/bin directory and then install Microsoft R Server, the automated installer detects that R exists and does not prompt you to link R to Revo64. In this case, you can use both versions of R simply by calling them by their respective script names, “R” and “Revo64”. To have Microsoft R Server become your default R, do the following:

1.  Rename the R script by appending the R version, for example 3.2.2:

		mv /usr/bin/R /usr/bin/R-3.2.2

2.  Create a symbolic link from the Revo64 script to R:

		ln -s /usr/bin/Revo64 /usr/bin/R

If you have installed Microsoft R Server and then install base R, again there should be no conflict; R and Revo64 point to their respective installations, as usual. You can use the above procedure to make Microsoft R Server your default version of R.

## See Also

[Install R Server 8.0.5 for Linux](rserver-install-linux-server-805.md)

[Install R Server 8.0 for Linux](rserver-install-linux-server-800.md)

[Install R on Hadoop overview](rserver-install-hadoop.md)

[Install R Server 8.0.5 for Hadoop](rserver-install-hadoop-805.md)

[Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md)

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)

[Configure R Server to operationalize analytics](install/operationalize-r-server-one-box-config.md)
