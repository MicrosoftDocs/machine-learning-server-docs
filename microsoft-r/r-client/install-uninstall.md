---

# required metadata
title: "Uninstall R client to upgrade to a newer version"
description: ""
keywords: "R Client, Microsoft R Client, remove, uninstall, uninstallation"
author: "j-martens"
manager: "jhubbard"
ms.date: "4/30/2017"
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

# Uninstall Microsoft R Client 

This article explains how to uninstall Microsoft R Client. **You do not have to uninstall R Client before installing a more recent version.**

## Uninstall on Windows

+ Remove Microsoft R Client like other applications using the Add/Remove dialog on Windows.

+ Alternately, you can use the same setup file used to install R Client to remove the program by specifying the /uninstall option on the command line such as:
  ```RClientSetup.exe /uninstall```


## Uninstall on Linux

## Program version and file locations

As a first step, use your package manager to list the currently installed R Server packages. (Typically, CentOS and Red Hat systems use **yum**, Ubuntu systems use **apt-get**, and SLES systems use **zypper**):

If your package manager is **yum**:

	yum list \*microsoft-r\*

If your package manager is **apt-get**:

	apt list \*microsoft-r\*

If your package manager is **zypper**:

	zypper search \*microsoft-r\*

## General instructions for all versions

Packages are registered in a database that tracks all package installations in the cluster. To update the database, use a package manager to remove the package: **yum** for Red Hat and CentOS, **zypper** for SUSE, or **apt-get** for Ubuntu.

Log in as root or a user with `sudo` privileges. If you are using `sudo`, precede commands requiring root privileges with `sudo` (for example, `sudo yum erase microsoft-r-server-mro-8.0`).

## How to uninstall 9.0.1 or 9.1.0

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

        yum erase microsoft-r-server-mro-3.3 		#(CentOS/RHEL systems))
		apt-get remove microsoft-r-server-mro-3.3	# (Ubuntu systems)
		zypper remove microsoft-r-server-mro-3.3	# (SLES systems)

2. On the root node, verify the location of other files that need to be removed: `

        ls /usr/lib64/microsoft-r

3. Remove the entire directory:

        rm -fr /usr/lib64/microsoft-r

The **rm** command removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.

## See Also

 [Install R on Hadoop overview](../install/r-server-install-hadoop.md)      
 [Install R Server 8.0.5 on Hadoop](../install/r-server-install-hadoop-805.md)      
 [Install Microsoft R Server on Linux](../install/r-server-install-linux-server.md) 
 [Troubleshoot R Server installation problems on Hadoop](../install/r-server-install-hadoop-troubleshoot.md)


## Learn More

You can learn more with these guides:

+ [Overview of Microsoft R](../index.md) 

+ [Overview of Microsoft R Client](../r-client-get-started.md) 

+ [Quickstart: Running R code in Microsoft R](../r/quickstart-run-r-code.md) (example)

+ [Diving into data analysis with Microsoft R](../r/how-to-introduction.md)

+ [RevoScaleR R package reference](../r/tutorial-introduction.md)

+ [MicrosoftML R package reference](../r/concept-what-is-the-microsoftml-package.md)

+ [mrsdeploy R package reference](../r-reference/mrsdeploy/mrsdeploy-package.md)