---

# required metadata
title: "Manage an R Server installation on Linux"
description: "Manage an R Server installation on Linux"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/09/2017"
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

# Manage your R Server installation on Linux

Covers file management (ownership and permissions), creating a local package repository for production servers deployed behind a firewall, and how to make RevoScaleR the default R script engine.

## File permissions

Normally, ordinary Microsoft R Server files are installed with read/write permission for owner and read-only permission for group and world. Directories are installed with execute permission as well, to permit them to be traversed. You can modify these permissions using the **chmod** command. (For files owned by root, this command requires root privileges.)

## File ownership

By default, the installed R Server files are all owned by root. For single-user workstations where the user has either sudo privileges or access to the root password, this is normally fine. In enterprise environments, however, it's common to have third-party applications such as Microsoft R Server installed into an account owned by a non-root user to reduce security concerns. In such an environment, you might want to create an "RUser" account, and change ownership of the files to that user. You can do that as follows:

1. Install Microsoft R Server as root, as usual.
2. Create the "RUser" account if it does not already exist. Assign this user to a suitable group, if desired.
3. Use the **chown** command to change ownership of the files (in the example below, we assume RUser has been made a member of the dev group; this command requires root privileges):

		chown -R RUser:dev /usr/lib64/MRS90LINUX

## Use Revo64 as the default R script engine

If you installed base R followed by Microsoft R Server, the automated installer detects that R exists and does not prompt you to link R to Revo64. In this case, you can use both versions of R simply by calling them by their respective script names, “R” and “Revo64”. 

To have Microsoft R Server become your default R, do the following:

1.  Rename the R script by appending the R version, for example 3.3.3:

		mv /usr/bin/R /usr/bin/R-3.3.3

2.  Create a symbolic link from the Revo64 script to R:

		ln -s /usr/bin/Revo64 /usr/bin/R

## Set up a local package repository behind a firewall

One of the strengths of the R language is the thousands of third-party packages that have been made publicly available via CRAN, the Comprehensive R Archive Network. R includes a number of functions that make it easy to download and install these packages. However, in many enterprise environments, access to the Internet is limited or non-existent. In such environments, it is useful to create a local package repository that users can access from within the corporate firewall.

A local repository may contain source packages, binary packages, or both. If some or all of your users will be working on Windows systems, you should include Windows binaries in your repository. Windows binaries are R-version-specific; if you are running R 3.3.3, you need Windows binaries built under R 3.3.3. These versioned binaries are available from CRAN and other public repositories. If some or all of your users will be working on Linux systems, you must include source packages in your repository.

The Microsoft Managed R Archive Network (MRAN) provides daily snapshots of all of CRAN, and together with the Microsoft-developed open-source package miniCRAN can be used to easily build a local package repository containing source packages, binary packages, or both.

There are two ways to create the package repository: either copy all the packages from a given MRAN snapshot, or create a new repository and populate it with just those packages you want to be available to your users. We will describe both procedures using MRAN and miniCRAN.

> [!Tip]
> The miniCRAN package itself is dependent on 18 other CRAN packages, among which is the RCurl package, which has a system dependency on the curl-devel package. Similarly, package XML has a dependency on libxml2-devel. We recommend, therefore, that you build your local repository initially on a machine with full Internet access, so that you can easily satisfy all these dependencies. After created, you can either move the repository to a different location within your firewall, or simply disable the machine’s Internet access.
>

### Approach 1: use miniCRAN

On a system with Internet access, the easiest way to install the miniCRAN package (or any R package) is to start R and use the install.packages function:

	install.packages("miniCRAN", dependencies=TRUE)

If your system already contains all the system prerequisites, this will normally download and install all of miniCRAN’s R package dependencies as well as miniCRAN itself. If a system dependency is missing, compilation of the first package that needs that dependency will fail, typically with a specific but not particularly helpful message. In our testing, we have found that an error about curl-config not being found indicates that the curl-devel package is missing, and an error about libxml2 indicates the libxml2-devel package is missing. If you get such an error, exit R, use yum or zypper to install the missing package, then restart R and retry the install.packages command.

### Approach 2: use an MRAN snapshot

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

## Create a custom repository

You can customize a package repository to include specific packages rather than all-inclusive snapshots providing more packages than you need. A custom repository gives you complete control over which packages are available to your users. Here, too, you have two basic choices in terms of populating your repository: you can either use the previously described miniCRAN makeRepo function to select specific packages from an existing MRAN snapshot

Alternatively, you can combine your own locally developed packages with packages from other sources. The second option offers the greatest control, but typically means you need to manage the contents using home-grown tools.

#### Recommendations for Windows binary packages

If using your own tools to create a custom package directory, your Windows binary packages should have a path of the form:

	/local/repos/bin/windows/contrib/3.3

while source packages should have a path of the form:

	local/repos/src/contrib

Windows binary packages should be built with your current R version. Windows binary files will be zip files. Package source files will be tar.gz files. 

Move the desired packages to the appropriate directories, then run the R function write\_PACKAGES in the tools package to create the PACKAGES and PACKAGES.gz index files:

	tools:::write_PACKAGES("/local/repos")

## Configure R to use a local repository

To make your local repository available to your R users:

1.  Open the file Rprofile.site in the /etc directory of your installed R (if you installed to the default /usr prefix, the path is /usr/lib64/MRO-for-MRS-9.0.1/R-3.3.3/lib64/R/etc/Rprofile.site).

2.  Find the following line in the Rprofile.site file:

		r <- c(REVO=RevoUtils::getRevoRepos())

3.  Add your repository as follows:

		r <- c(REVO=RevoUtils::getRevoRepos(),
			LOCAL="file:///local/repos")

	 If you added more than one repository and used R version numbers, add multiple lines of the form LOCAL\_VER="file:///local/repos/VER", for example:

		r <- c(REVO=RevoUtils::getRevoRepos(),
		       LOCAL_3.3="file:///local/repos/3.3",
		       LOCAL_3.2="file:///local/repos/3.2",
		       LOCAL_3.1="file:///local/repos/3.1")

4.  Save the file and your repository is ready for use.

If you are in a locked-down environment without access to the standard Microsoft repository, *replace* the Revo repository with your local repository (or repositories).


## See Also

[Install R Server 8.0.5 for Linux](rserver-install-linux-server-805.md)

[Install R Server 9.0.1 for Linux](rserver-install-linux-server-901.md)

[Install R on Hadoop overview](rserver-install-hadoop.md)

[Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-linux-uninstall.md)

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)

[Configure R Server to operationalize analytics](operationalize/configuration-initial.md)
