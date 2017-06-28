---

# required metadata
title: "Installation Guide for R Server 8.0 for Linux systems"
description: "Install R Server 8.0 on Linux."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/18/2016"
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

# R Server 8.0 Installation for Linux Systems

Older versions of R Server for Linux are no longer available on the Microsoft download sites, but if you already have an older distribution, you can follow these instructions to deploy version 8.0. For the current release, see [Install R Server for Linux](rserver-install-linux-server.md).

Installation of Microsoft R Server 8.0 consists of two distinct steps:

- Install R Open
- Install R Server

To install R Open and R Server:

1. Log in as root, or as a user with super user (sudo) privileges. Optionally, you can install as a non-root user. See [Non-Root Installs](#non-root-installs) for instructions.

2. Install Microsoft R Open according to the [online instructions](http://go.microsoft.com/fwlink/?LinkID=699383&clcid=0x409) for your platform.

3. Find the download for the 8.0 version of Microsoft R Server for Linux on your system. If you have an img file, you must first mount the file. The following commands create a mount point and mount the file to that mount point:

	    mkdir /mnt/mrsimage
	    mount –o loop <filename> /mnt/mrsimage

  For RHEL/CENTOS systems;
		tar zxvf MRS80RHEL.tar.gz

  For SLES systems;
		tar zxvf MRS80SLES.tar.gz

6.  In either case, you will then want to copy the installer gzipped tar file to a writable directory, such as /tmp:

  From the mounted img file:
		cp /mnt/mrsimage/Microsoft-R-Server-`*`.tar.gz /tmp

  From the unpacked tar file:
		cp /tmp/MRS80*/Microsoft-R-Server-`*`.tar.gz /tmp

7. Unpack the tar file and run the installer script, as follows:

		cd /tmp
		tar xvzf Microsoft-R-Server-8.0.0-<OS>.tar.gz
		pushd rrent
		./install.sh

The installer prompts you for the following:

a. Location of your R installation. If you installed Microsoft R Open for Microsoft R Server to the default location, you can simply press Enter and continue.

b. Location for installing additional Microsoft R Server files. These are files that are not part of specific R packages, such as documentation, licenses, and scripts.

c. Whether you would like to load the rpart and lattice packages by default; these packages are recommended as they enhance some RevoScaleR operations.

d. Agreement to the Microsoft R Server software license.

8.  Once you have agreed to the license, the installation completes. Type the following to return to your original directory: `popd`

On Linux systems with Hadoop installed, the install.sh script also tries to configure Microsoft R Server for use with Hadoop. On such systems, the install.sh script runs a Python script that queries the Hadoop environment for certain environment variables and searches the Hadoop installation for certain files, writing a set of Hadoop environment variables required by Microsoft R Server to a file in the R Server installation directories. For complete details on Hadoop configuration, including troubleshooting when the automated configuration is incomplete or inaccurate, see the [Microsoft R Server Hadoop Configuration Guide](install/r-server-install-hadoop.md).

If you receive messages about uninstalled dependencies, see [Package Dependencies for Microsoft R Server installations on Linux and Hadoop](rserver-install-linux-hadoop-packages.md).

<a name="non-root-installs"></a>
### Non-Root installs of R Server 8.0

If you have an older distribution of Microsoft R Server 8.0 for Linux, you can run the installer as a non-root user without sudo privileges. You can run either a complete install or simply extract the files to a directory for subsequent installation using your own install scripts. For a complete install to succeed, however, the following conditions must be met:

-   You must have all the prerequisite packages installed on your computer.
-   You must have write permission to the specified base installation directory.

The installer will check that the prerequisite packages are installed and inform you of any that are missing. The prerequisite packages typically do require superuser priviliges to install, so if you do not have sudo privileges, you will probably need to consult your system administrator in order to install them.

If all the prerequisites are installed, you complete the installation as follows:

1. You must already have the 8.0 download because it is no longer available on the download sites. Newer versions of the installers do not support non-root installs.

2. Run the following commands to install the rpm file:

		mkdir $HOME/localmrsrpmdb
		rpm --initdb --dbpath $HOME/localmrsrpmdb
		export MRS_RPM_DBPATH=$HOME/localmrsrpmdb && \
		    rpm --dbpath $MRS_RPM_DBPATH --prefix $HOME \
		        --nodeps -i MRO-for-MRS-`*`.rpm

3. Unpack the Microsoft R Server tarball, then run the installer script, as follows (the tarball name may include an operating system ID; the complete name of the tarball will be in your download letter):

		tar xvzf Microsoft-R-Server-8.0.0-`*`.tar.gz
		pushd rrent
		./install.sh

At this point you are asked for the location of your R installation—be sure to use the same location you provided as the --prefix argument in step 2. You are also asked where you would like to install additional Microsoft R Server files; these are files that are not part of specific R packages, such as documentation, licenses, and scripts.

You are then asked if you would like to load the rpart and lattice packages by default; these packages are recommended as they enhance some RevoScaleR operations.

Finally, you are asked to agree to the Microsoft R Server software license. Once you have agreed to the license, the installation completes. Type the following to return to your original directory: `popd`

The R installation is owned by the user who completed the installation; subsequent updates and uninstalls must be performed by the same user. In particular, multiple users can perform non-root installations–as long as the installations specify separate base installation directories, they will be completely independent. Non-root installations can be performed on systems that have had Microsoft R Server installed as root, and vice versa.

In general, non-root installs require you to specify the complete path to the Revo64 script once installation is complete. To avoid this, add the path to the directory containing your Revo64 script to your PATH environment variable, for example, by adding the following to the end of your .bash\_profile file: `export PATH=/home/$USER/Revo-8.0/R-3.1.3/bin:$PATH`

## See Also

[Install R on Hadoop overview](install/r-server-install-hadoop.md)

[Install R Server 2016 on Hadoop](install/r-server-install-hadoop-805.md)

[Install R Server 8.0 on Hadoop](install/r-server-install-hadoop-800.md)

[Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md)

[Troubleshoot R Server installation problems on Hadoop](install/r-server-install-hadoop-troubleshoot.md)
