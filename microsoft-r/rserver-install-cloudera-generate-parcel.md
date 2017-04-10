---

# required metadata
title: "Generate a parcel for R Server 9.1.0 on CDH"
description: "Generate a parcel to install Microsoft R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)."
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

# Generate a parcel

**Applies to:** R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)

In previous releases, parcel installation required downloading two pre-build parcel files. The 9.1.0 release improves upon this experience by providing a parcel generator script. The script guides you through a series of steps and produces a single parcel file in the end.

The script name is `generate_mrs_parcel.sh`.

Requirements and limitations on using the script include the following:

+ CDH must be running on RHEL 7.x or later.
+ You can include MicrosoftML in the parcel.
+ Excluded and unsupported by the parcel generator the operationalization features: mrsdeploy (remote execution, web service deployment), web node and compute node configurations. If you require these features, you can install the packages manually. For instructions, see [Manual package installation](rserver-install-hadoop-manual-package.md).

## Script syntax

~~~~
$ bash generate_mrs_parcel.sh -n
~~~~

## Script options

flag | Option | Description
-----|--------|------------
 -m | --distro-name [DISTRO]| Target Linux distribution for this parcel, one of: el6 el7 sles11
 -l | --add-mml  | Add Microsoft ML to the Parcel regardless of the target system.
 -a | --accept-eula | Accept all end user license agreements.
 -d | --download-mro | Download microsoft r open for distribution to an offline system.
 -s | --silent | Perform a silent, unattended install.
 -u | --unattended | Perform an unattended install.
 -n | --dry-run | Don't do anything, just show what would be done.
 -h | --help | Print this help text.

## Next steps

After you generate a parcel, the next step is to [deploy the parcel using Cloudera Manager and activate r Server instance](rserver-install-cloudera-deploy-activate.md).

## See Also

[Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](rserver-install-cloudera.md)
