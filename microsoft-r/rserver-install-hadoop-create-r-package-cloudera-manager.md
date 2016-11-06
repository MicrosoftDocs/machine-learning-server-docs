---
# required metadata
title: "Create an R Package Parcel for Cloudera Manager"
description: "Create an R Package Parcel for Cloudera Manager to distribute R across a Hadoop cluster."
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/14/2016"
ms.topic: ""
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

# Create an R Package Parcel for Cloudera Manager

If you are using Cloudera Manager to manage your Cloudera Hadoop cluster, you can use the Microsoft R Server Parcel Generator to create a Cloudera Manager parcel containing additional R packages, and use the resulting parcel to distribute those packages across all the nodes of your cluster.

The Microsoft R Server Parcel Generator is a Python script that takes a library of R packages and creates a Cloudera Manager parcel that **excludes any base or recommended packages, or packages included with the standard Microsoft R Server distribution**. Make sure to consider any dependencies your packages might have and be sure to include those in your library. If you installed Microsoft R Server with Cloudera Manager parcels, you will find the Parcel Generator in the *Revo.home()/scripts* directory. (You may need to ensure that the script has execute permission using the chmod command, or you can call it as “python generate\_r\_parcel.py”.)

When you call the script, you must provide a name and a version number for the resulting parcel, together with the path to the library you would like to package. When choosing a name for your parcel, be sure to pick a name that is unique in your parcel repository (typically /opt/cloudera/parcel-repo). For example, to package the library /home/RevoUser/R/library, you might call the script as follows:

	generate_r_parcel.py –p "RevoUserPkgs" –v "0.1" –l /home/RevoUser/R/library

By default, the path to the library you package should be the same as the path to the library on the Hadoop cluster. You can specify a different destination using the –d flag:

	generate_r_parcel.py –p "RevoUserPkgs" –v "0.1" -l /home/RevoUser/R/library –d /var/RevoShare/RevoUser/library

To distribute and activate your parcel perform the following steps:

1.  Copy or move your .parcel and .sha files to the parcel repository on your Cloudera cluster (typically, /opt/cloudera/parcel-repo)
2.  Ensure that the .parcel and .sha files are owned by root and have 755 permissions (that is, read, write, and execute permission for root, and read and execute permissions for group and others).
3.  In your browser, open Cloudera Manager.
4.  Click **Hosts** in the upper navigation bar to bring up the All Hosts page.
5.  Click **Parcels** to bring up the Parcels page.
6.  Click **Check for New Parcels**. Your new parcel should appear with a **Distribute** button. After clicking Check for New Parcels you may need to click on “All Clusters” under the “Location” section on the left to see the new parcel.
7.  Click the **Distribute** button for your parcel. The parcel will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.
8.  Click **Activate**. Activation prepares your parcel to be used by the cluster.

After your parcel is distributed and activated your R packages should be present in the libraries on each node and can be loaded into your next R session.

## See Also

[Install R on Hadoop overview](rserver-install-hadoop.md)

[Install R Server 2016 on Hadoop](rserver-install-hadoop-805.md)

[Install R Server 8.0 on Hadoop](rserver-install-hadoop-800.md)

[Install Microsoft R Server on Linux](rserver-install-linux-server.md)

[Uninstall Microsoft R Server to upgrade to a newer version](rserver-install-uninstall-upgrade.md)

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)
