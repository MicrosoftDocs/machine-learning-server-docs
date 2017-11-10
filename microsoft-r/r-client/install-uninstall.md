---

# required metadata
title: "Uninstall Microsoft R Client to upgrade to a newer version - Machine Learning Server "
description: "Explains how to uninstall Microsoft R Client. You do not have to uninstall R Client before installing a more recent version."
keywords: "R Client, Microsoft R Client, remove, uninstall, uninstallation"
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

# Uninstall Microsoft R Client 

This article explains how to uninstall Microsoft R Client. **You do not have to uninstall R Client before installing a more recent version.**

## Uninstall on Windows

+ Remove Microsoft R Client like other applications using the Add/Remove dialog on Windows.

+ Alternately, you can use the same setup file used to install R Client to remove the program by specifying the /uninstall option on the command line such as:
  ```RClientSetup.exe /uninstall```


## Uninstall on Linux

## Program version and file locations

As a first step, use your package manager to list the currently installed Machine Learning Server packages. (Typically, CentOS and Red Hat systems use **yum**, Ubuntu systems use **apt-get**, and SLES systems use **zypper**):

If your package manager is **yum**:

	yum list \*microsoft-r\*

If your package manager is **apt-get**:

	apt list \*microsoft-r\*

If your package manager is **zypper**:

	zypper search \*microsoft-r\*

## General instructions for all versions

Packages are registered in a database that tracks all package installations in the cluster. To update the database, use a package manager to remove the package: **yum** for Red Hat and CentOS, **zypper** for SUSE, or **apt-get** for Ubuntu.

Log in as root or a user with `sudo` privileges. If you are using `sudo`, precede commands requiring root privileges with `sudo` (for example, `sudo yum erase microsoft-r-server-mro-8.0`).

## How to uninstall

1. Uninstall Microsoft R Open (MRO) and remove any dependent packages used only by MRO:

        yum erase microsoft-r-server-mro-x.x 		#(CentOS/RHEL systems))
		apt-get remove microsoft-r-server-mro-x.x	# (Ubuntu systems)
		zypper remove microsoft-r-server-mro-x.x	# (SLES systems)

   where x.x is 3.3 for R Server 9.0.1 or 9.1.0, and 3.4 for Machine Learning Server 9.2.

2. On the root node, verify the location of other files that need to be removed: `

        ls /usr/lib64/microsoft-r

3. Remove the entire directory:

        rm -fr /usr/lib64/microsoft-r

The **rm** command removes the folder. Parameter "f" is for force and "r" for recursive, deleting everything under microsoft-r. This command is destructive and irrevocable, so be sure you have the correct directory before you press Enter.


## Learn More

You can learn more with these guides:

+ [Overview of Microsoft R Client](../r-client-get-started.md) 

+ [Quickstart: Running R code in Microsoft R](../r/quickstart-run-r-code.md) (example)

+ [How-to guides in Machine Learning Server](../r/how-to-introduction.md)

+ [RevoScaleR R package reference](../r/tutorial-introduction.md)

+ [MicrosoftML R package reference](../r-reference/microsoftml/microsoftml-package.md)

+ [mrsdeploy R package reference](../r-reference/mrsdeploy/mrsdeploy-package.md)