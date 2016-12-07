---
# required metadata
title: "Troubleshoot Microsoft R installation problems on Hadoop"
description: "Troubleshoot Microsoft R installation on a Hadoop cluster."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/05/2016"
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
# Troubleshoot Microsoft R installation problems on Hadoop

No two Hadoop installations are exactly alike, but most are quite similar. This article brings together a number of common errors seen in attempting to run Microsoft R Server commands on Hadoop clusters, and the most likely causes of such errors from our experience.

### Missing dependencies

Linux and Hadoop servers that are locked down will prevent the installation script from downloading any required dependencies that are missing on your system. For a list of all dependencies required by Microsoft R, see [Package Dependencies for Microsoft R Server installations on Linux and Hadoop](rserver-install-linux-hadoop-packages.md).

### No Valid Credentials

If you see a message such as “No valid credentials provided”, this means you do not have a valid Kerberos ticket. Quit Microsoft R Server, obtain a Kerberos ticket using kinit, and then restart Microsoft R Server.

### Unable to Load Class RevoScaleR

If you see a message about being unable to find or load main class RevoScaleR, this means that the jar file scaleR-hadoop-0.1-SNAPSHOT.jar could not be found. This jar file must be in a location where it can be found by the getHadoopEnvVars.py script, or its location must be explicitly added to the CLASSPATH.

### Classpath Errors

If you see other errors related to Java classes, these are likely related to the settings of the following environment variables:

- PATH
- CLASSPATH
- JAVA\_LIBRARY\_PATH

Of these, the most commonly misconfigured is the CLASSPATH.

### Unable to Load Shared Library

If you see a message about being unable to load libhdfs.so, you may need to create a symbolic link from your installed version of libhdfs.so to the system library, such as the following:

	ln -s /path/to/libhdfs.so /usr/lib64/libhdfs.so

Or, update your LD\_LIBRARY\_PATH environment variable to include the path to the libjvm shared object:

	export LD\_LIBRARY\_PATH=$LD_LIBRARY_PATH:/path/to/libhdfs.so

(This step is normally performed automatically during the RRE install. If you continue to see errors about libhdfs.so, you may need to both create the symbolic link as above and set LD_LIBRARY_PATH.)

Similarly, if you see a message about being unable to load libjvm.so, you may need to create a symbolic link from your installed version of libjvm.so to the system library, such as the following:

	ln -s /path/to/libjvm.so /usr/lib64/libjvm.so

Or, update your LD\_LIBRARY\_PATH environment variable to include the path to the libjvm shared object:

export LD\_LIBRARY\_PATH=$LD\_LIBRARY\_PATH:/path/to/libjvm.so
