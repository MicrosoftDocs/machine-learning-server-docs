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

In previous releases, parcel installation required downloading two pre-built parcel files. The 9.1.0 release improves upon this experience by providing a parcel generator script. The script guides you through a series of steps and produces a single parcel file in the end.

The script name is `generate_mrs_parcel.sh`.  Output files include the following:

+ a parcel
+ a checksum
+ a Custom Service Descriptor

Requirements and limitations on using the script include:
 
+ You can include MicrosoftML in the parcel if CDH is running on RHEL 7.x or later.
+ Excluded and unsupported by the parcel generator are operationalization features: mrsdeploy (remote execution, web service deployment), web node and compute node configurations. If you require these features, you can install the packages manually. For instructions, see [Manual package installation](rserver-install-hadoop-manual-package.md).

## Script syntax and parameters

~~~~
$ sudo bash generate_mrs_parcel.sh -n
~~~~

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

## Run the generate_mrs_parcel.sh script

1. Download the gzipped tar file for R Server 9.1.0 for Hadoop to the master node in your CDH cluster. For download instructions, see [Install R Server on Hadoop](rserver-install-hadoop-command-line.md).

2. In **/tmp**, assuming it is the download directory, unpack the distribution:

  `[tmp] $ tar zxvf microsoft_r_server_9.1.0.tar.gz`

3. Verify system repositories are up to date:

    `sudo yum clean all`

4. Change to the directory to which you mounted or unpacked the installer (for example, /tmp/MRS90HADOOP if you unpacked the tar.gz file):

  `[tmp] $ cd MRS_Linux`

5. Run the script with the **-n** parameter to preview the actions.

  `[tmp MRS_Linux] $ sudo bash generate_mrs_parcel.sh -n`

  You will be prompted to read and accept license agreements. 
  
  You are also asked to specify the underlying operating system. If the platform supports it, the parcel generator adds installation instructions for features having a dependency on .NET Core. Namely, these features include Microsoft machine learning and application components used for remote execution, web service deployment, web node, and compute node configurations. RHEL 7.x is the platform with .NET Core support.

  When the script is finished, the location of the parcel, checksum, and CSD is printed to the console.

  [console output messages][media/rserver-install-cloudera/parcelgeneratoroutput.png]

## Copy the parcel to the Cloudera parcel repository

1. Copy the parcel file `MRS-9.1.0-el7.parcel` and `MRS-9.1.0-el7.parcel.sha` to the Cloudera parcel repository, typically /opt/cloudera/parcels.

    `cp MRS-9.1.0-el7.parcel /opt/cloudera/parcels`

## Copy the CSD to the Cloudera CSD repository

1. Copy the CSD file `MRS-9.1.0-el7-CONFIG.jar` to the Cloudera CSD directory, typically /opt/cloudera/csd.

2. Modify the permissions of CSD file as follows: 

        sudo chmod 644 /opt/cloudera/csd/MRS-9.1.0-CONFIG.jar
        sudo chown cloudera-scm:cloudera-scm /opt/cloudera/csd/MRS-9.1.0-CONFIG.jar

3.	Stop and restart the cloudera-scm-server service using the following shell commands:

        sudo service cloudera-scm-server stop
        sudo service cloudera-scm-server start


## Next steps

After you generate a parcel and CSD and copy the files to the appropriate repositories, the next step is to [deploy the parcel using Cloudera Manager and activate r Server instance](rserver-install-cloudera-deploy-activate.md).

## See Also

[Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](rserver-install-cloudera.md)
