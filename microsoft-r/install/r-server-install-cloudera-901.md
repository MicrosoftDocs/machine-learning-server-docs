---
# required metadata
title: "Install R Server 9.0.1 on CDH"
description: "Install R Server 9.0.1 on Cloudera Distribution Including Apache Hadoop (CDH)."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/08/2017"
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

# Install R Server 9.0.1 on Cloudera Distribution Including Apache Hadoop (CDH)

We recommend the latest version of [R Server for Hadoop](r-server-install-cloudera.md) for the most features and for simplicity of installation, but if you have an older download of R Server for Hadoop 9.0.1, you can use the following instructions to create a parcel in Cloudera Manager. Multi-node installation is also covered in this article.

## Create an R Package Parcel for Cloudera Manager

If you are using Cloudera Manager to manage your Cloudera Hadoop cluster, you can use the Microsoft R Server Parcel Generator to create a Cloudera Manager parcel containing additional R packages, and use the resulting parcel to distribute those packages across all the nodes of your cluster.

The Microsoft R Server Parcel Generator is a Python script that takes a library of R packages and creates a Cloudera Manager parcel that **excludes any base or recommended packages, or packages included with the standard Microsoft R Server distribution**. Make sure to consider any dependencies your packages might have and be sure to include those in your library. 

You can find the generate_r_parcel.py script at the following link: https://aka.ms/generate_r_parcel.py

You can find the Parcel Generator used to run the script in the *MRS-x.y.z/bin* directory. You might need to ensure that the script has execute permission using the `chmod` command, or you can call it as `python generate\_r\_parcel.py`.)

When calling the script, yprovide a name and a version number for the resulting parcel, together with the path to the library you would like to package. When choosing a name for your parcel, be sure to pick a name that is unique in your parcel repository (typically /opt/cloudera/parcel-repo). For example, to package the library /home/RevoUser/R/library, you might call the script as follows:

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

## Multinode installation using Cloudera Manager

The following steps walk you through a multinode installation using Cloudera Manager to create a Cloudera Manager parcel for an R Server installation.

Two parcels are required:

- *Microsoft R Open* parcel installs open source R and additional open-source components on the nodes of your Cloudera cluster. This distribution of MRO must be downloaded using the link below. Please do not use MRO from MRAN or the MRO package from another installation of R Server.
- *Microsoft R Server* parcel installs proprietary components on the nodes of your Cloudera cluster.

Install the Cloudera Manager parcels as follows:

1. [Download the Microsoft R Open for Microsoft R Server Cloudera Manager parcel](https://rserverdistribution.azureedge.net/production/MRO/3.3.2/485/1033/f9644b5c602b4479bcdaa88d55cdd977/MRO-3.3.2-Cloudera.tar.gz). Note that the parcel consists of two files, the parcel itself and its associated .sha file. They may be packaged as a single .tar.gz file for convenience in downloading, but that must be unpacked and the two files copied to the parcel-repo for Cloudera Manager to recognize them as a parcel.

2. Download and unpack the R Server distribution, which will either be a DVD img file (if you obtained Microsoft R Server via Microsoft Volume Licensing) or a gzipped tar file (if you obtained Microsoft R Server via MSDN or Dev Essentials). The distribution file includes the required Cloudera Parcel files.

  If you have an img file, you must first mount the file. The following commands create a mount point and mount the file to that mount point:

		mkdir /mnt/mrsimage
		mount –o loop MRS90HADOOP.img /mnt/mrsimage

  If you have a gzipped tar file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as /tmp):

		tar zxvf MRS90HADOOP.tar.gz

3. Copy the parcel files to your local parcel-repo, typically /opt/cloudera/parcel-repo:

  From the mounted img file:
		cp /mnt/mrsimage/MRS_Parcel/MRS-9.0.1-* /opt/cloudera/parcel-repo

  From the unpacked tar file:
		cp /tmp/MRS90HADOOP/MRS_Parcel/MRS-9.0.1-* /opt/cloudera/parcel-repo
 
4. You should have the following files in your parcel repo:

		MRO-3.3.2-el6.parcel
		MRO-3.3.2-el6.parcel.sha
		MRS-9.0.1-el6.parcel
		MRS-9.0.1-el6.parcel.sha

  Be sure all the files are owned by root and have 644 permissions (read, write, permission for root, and read permission for groups and others). Parcels should not have a file extension. If any parcels have a .sha file extension, please rename the file to remove the extension.

5. In your browser, open Cloudera Manager.

6. Click **Hosts** in the upper navigation bar to bring up the All Hosts page.

7. Click **Parcels** to bring up the Parcels page.

8. Click **Check for New Parcels**. MRO and MRS parcels should each appear with a **Distribute** button. After clicking **Check for New Parcels** you may need to click on **Location > All Clusters** on the left to see the new parcels.

9. Click the MRO **Distribute** button. Microsoft R Open will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.

10. Click **Activate**. Activation prepares Microsoft R Open to be used by the cluster.

11. Click the MRS **Distribute** button. Microsoft R Server will be distributed to all the nodes of your cluster. When the distribution is complete, the **Distribute** button is replaced with an **Activate** button.



## See Also

[Install R on Hadoop overview](../rserver-install-hadoop.md)

[Install R Server 8.0.5 on Hadoop](r-server-install-hadoop-805.md)

[Install R Server 8.0 on Hadoop](r-server-install-hadoop-800.md)

[Install Microsoft R Server on Linux](../rserver-install-linux-server.md)

[Uninstall Microsoft R Server to upgrade to a newer version](../rserver-install-uninstall-upgrade.md)

[Troubleshoot R Server installation problems on Hadoop](r-server-install-hadoop-troubleshoot.md)
