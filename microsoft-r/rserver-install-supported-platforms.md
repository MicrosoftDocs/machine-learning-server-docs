---

# required metadata
title: "Supported Platforms for Microsoft R Server"
description: "A list of the operating systems supported by editions and versions of Microsoft R Server and Revolution R Enterprise."
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/21/2016"
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
# Supported Platforms for Microsoft R Server

This article specifies supported operating systems, distributions, and database platforms for all supported versions of Microsoft R, Revolution R Server, and Revolution R Workstation.

## Microsoft R Server 8.0.5

**Operating Systems (64-Bit only)**

- Red Hat Enterprise Linux (RHEL) 6.x and 7.x, or CentOS   
- SUSE Linux Enterprise Server 11 (SLES11)

**Microsoft R Server for Teradata**

- Teradata Database 14.10, 15.00, 15.10
- Operating System: SLES11

**Microsoft R Server for Hadoop**

- Hadoop Distributions: Cloudera CDH 5.5-5.7, Hortonworks HDP 2.3-2.4, MapR 5.0-5.1
- Operating Systems: RHEL 6.x and 7.x, SUSE SLES11
- Spark version: 1.5.0-1.6.1 (if using Spark)

## Microsoft R Server 8.0.0

Microsoft R Server 8.0.0, formerly known as "Revolution R Enterprise", is supported on the platforms listed below.

**Operating Systems**

- Windows 7 through 10 (64-Bit)  
- Windows Server 2012 (64-Bit)  
- Red Hat Enterprise Linux - 5.x and  6.x (64-Bit)  
- SUSE Linux Enterprise Server 11 SP2, SP3 (64-Bit)  

Linux platforms require 64-bit processor with x86-compatible architecture.

**Hadoop Distributions**

- Cloudera CDH 5.0 - 5.4 on RHEL 6.x
- MapR M3 v3.1, 3.1.1, 4.0.1, 4.0.2, 4.0.3 on RHEL 6.x
- MapR M5 v3.1, 3.1.1, 4.0.1, 4.0.2, 4.0.3 on RHEL 6.x
- MapR M7 v3.1, 3.1.1, 4.0.1, 4.0.2, 4.0.3 on RHEL 6.x

MapR 4.0.2 requires the mapr-patch-4.0.2.29870.GA-30600; contact MapR to obtain the patch.

**EDW Platforms**

- Teradata Database 14.10/SLES 10.x
- Teradata Database 14.10/SLES 11.x
- Teradata Database 15.0, 15.10/SLES 10.x
- Teradata Database 15.0, 15.10/SLES 11.x

Additional Hadoop Distributions Supported as HDFS Data Sources via ConnectR (includes technical support for the open-source RHadoop project):

- IBM Big Insights 1.4.1, 2.0 HDFS
- Intel Hadoop(IDH) 1.03, 2.2 HDFS
- Pivotal HD 1.0 HDFS

**DevelopR and DeployR**

- DevelopR is only supported on Windows platforms or Windows emulators, and accessible via Windows Remote Desktop for remote Windows servers. Install DevelopR on a separate Windows workstation, or on a Hadoop edge node or additional Linux or Windows server.
- DeployR browser requirements: Internet Explorer 8.0 and higher, Mozilla Firefox 35.0.1 and higher, Google Chrome 28.0 and higher.

## Revolution R Enterprise 7.4 and 7.4.1 Workstation

Revolution R Enterprise Workstation is licensed for a single named user, and available in two editions: Entry and Power. DevelopR is only supported on Windows platforms.

- Revolution Enterprise Workstation — Entry Edition: One desktop or laptop, 1-4 cores.
- Revolution Enterprise Workstation — Power Edition: One desktop or laptop, 5-8 cores. For machines with more than 8 cores, use Revolution R Enterprise Server.
- Revolution R Enterprise Workstation is supported on the following platforms:
    - 64-Bit Windows 7
    - 64-Bit Windows 8
    - 64-Bit Red Hat Enterprise Linux 5.x, 6.x

## Revolution R Enterprise 7.4 and 7.4.1 Server

**Operating Systems**

- Windows 7 through 8 (64-Bit)  
- Windows Server 2008 SP2 through 2012 (64-Bit)  
- Red Hat Enterprise Linux 5.x through 6.x (64-Bit)  
- SUSE Linux Enterprise Server 11 SP2, SP3 (64-Bit)  

Linux platforms require 64-bit processor with x86-compatible architecture.

**Clusters and Grids**

- Platform LSF 7, 9 on 64-bit RHEL 5.x through 6.x
- 64-Bit Windows Server 2008 SP2/HPC Pack 2008

**Hadoop Distributions**

- Cloudera CDH 5.0, 5.1, 5.2, 5.3 on RHEL 6.x
- Hortonworks HDP 1.3 on RHEL 5.x through 6.x
- Hortonworks HDP 2.0, 2.1 and 2.2 on RHEL 6.x
- MapR M3 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x
- MapR M5 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x
- MapR M7 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x

**EDW Platforms**

- Teradata Database 14.10/SLES 10.x
- Teradata Database 14.10/SLES 11.x
- Teradata Database 15.0/SLES 10.x
- Teradata Database 15.0/SLES 11.x

Additional Hadoop Distributions Supported as HDFS Data Sources via ConnectR:

- IBM Big Insights 1.4.1, 2.0 HDFS
- Intel Hadoop(IDH) 1.03, 2.2 HDFS
- Pivotal HD 1.0 HDFS

## Revolution R Enterprise 7.3 Workstation
Revolution R Enterprise Workstation is licensed for a single named user, and available in two editions: Entry and Power.

- Revolution Enterprise Workstation — Entry Edition: One desktop or laptop, 1-4 cores
- Revolution Enterprise Workstation — Power Edition: One desktop or laptop, 5-8 cores. For machines with more than 8 cores, use Revolution R Enterprise Server.

Revolution R Enterprise Workstation is supported on the following platforms. DevelopR is only supported on Windows platforms.

- 64-Bit Windows 7 through 8
- 64-Bit Red Hat Enterprise Linux 5.x, 6.x

## Revolution R Enterprise 7.3 Server

**Operating Systems**

- Windows 7 through 8 (64-Bit)  
- Windows Server 2008 SP2 through 2012 (64-Bit)  
- Red Hat Enterprise Linux 5.x through 6.x (64-Bit)  
- SUSE Linux Enterprise Server 11 SP3 (64-Bit)  

Linux platforms require 64-bit processor with x86-compatible architecture.

**Clusters and Grids**

- Platform LSF 7, 9 on 64-bit RHEL 5.x through 6.x
- 64-Bit Windows Server 2008 SP2/HPC Pack 2008

**Hadoop Distributions**

- Cloudera CDH 5.0, 5.1, 5.2, 5.3 on RHEL 6.x
- Hortonworks HDP 1.3 on RHEL 5.x through 6.x
- Hortonworks HDP 2.0, 2.1 and 2.2 on RHEL 6.x
- MapR M3 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x
- MapR M5 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x
- MapR M7 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x

**EDW Platforms**

- Teradata Database 14.10/SLES 10.x
- Teradata Database 14.10/SLES 11.x
- Teradata Database 15.0/SLES 10.x
- Teradata Database 15.0/SLES 11.x

Additional Hadoop Distributions Supported as HDFS Data Sources via ConnectR

- IBM Big Insights 1.4.1, 2.0 HDFS
- Intel Hadoop(IDH) 1.03, 2.2 HDFS
- Pivotal HD 1.0 HDFS

## Revolution R Enterprise 7.0, 7.1, 7.2 for Windows
Revolution R Enterprise for Windows include RevoScaleR, the R Productivity Environment (RPE), and ParallelR.
Approximately 300MB free disk space is required for a full install of Revolution R Enterprise. We recommend at least 2GB of RAM to use Revolution R Enterprise.

Supported 32-bit and 64-bit Windows platforms:

- Windows XP SP2 (applies to 64-bit only)
- Windows XP SP3 (applies to 32-bit only)
- Windows Vista
- Windows 7
- Windows 8 (version 6.1 and higher)
- Windows Server 2003
- Windows Server 2008
- Windows HPC Server 2008 (applies to 64-bit only)
- Windows Server 2012 (applies only Revolution R Enterprise 7.2 only)

Windows HPC Server 2008 R2 clusters are required to use the distributed computing features of RevoScaleR.

Note: This excludes Revolution R Enterprise for IBM PureData System for Analytics, R for Hadoop and Revolution R for Teradata. Please contact Technical Support - support@revolutionanalytics.com for the latest compatibility information for these platforms.

## Revolution R Enterprise 7.0, 7.1, 7.2 for Linux

Revolution R Enterprise for Linux includes RevoScaleR and ParallelR.
Approximately 300MB free disk space is required for a full install of Revolution R Enterprise. We recommend at least 2GB of RAM to use Revolution R Enterprise.

Supported 64-bit Linux platforms:

- Red Hat Enterprise Linux 5.x
- Red Hat Enterprise Linux 6.x
- SUSE Enterprise Linux v11 SP3

Platform LSF 7 or later is required to use the distributed computing features of RevoScaleR

Note: This excludes Revolution R Enterprise for IBM PureData System for Analytics, R for Hadoop and Revolution R for Teradata. Please contact Technical Support - support@revolutionanalytics.com for the latest compatibility information for these platforms.

## Revolution R Enterprise 6.x for Windows

**Revolution R Enterprise 6.x reached end of life in January 2016 and will stop being supported July 2016.**

Revolution R Enterprise for Windows includes RevoScaleR, the R Productivity Environment (RPE), and ParallelR.
Approximately 300MB free disk space is required for a full install of Revolution R Enterprise. We recommend at least 2GB of RAM to use Revolution R Enterprise.

Supported 32-bit and 64-bit Windows platforms:

- Windows XP SP2 (applies to 64-bit only)
- Windows XP SP3 (applies to 32-bit only)
- Windows Vista
- Windows 7
- Windows 8 (version 6.1 and higher)
- Windows Server 2003
- Windows Server 2008
- Windows HPC Server 2008 (64-bit only)

Windows HPC Server 2008 R2 clusters are required to use the distributed computing features of RevoScaleR.

Note: This excludes Revolution R Enterprise for IBM PureData System for Analytics, R for Hadoop and Revolution R for Teradata. Please contact Microsoft Support for the latest compatibility information for these platforms.

## Revolution R Enterprise 6.0, 6.1, 6.2 for Linux

**Revolution R Enterprise 6.x reached end of life in January 2016 and will stop being supported July 2016.**

Revolution R Enterprise for Linux includes RevoScaleR and ParallelR. Approximately 300MB free disk space is required for a full install of Revolution R Enterprise. We recommend at least 2GB of RAM to use Revolution R Enterprise.

Supported 64-bit Linux platforms:

- Red Hat Enterprise Linux 5.x
- Red Hat Enterprise Linux 6.x

Platform LSF 7 or later is required to use the distributed computing features of RevoScaleR.

Note: This excludes Revolution R Enterprise for IBM PureData System for Analytics, R for Hadoop and Revolution R for Teradata. Please contact Microsoft Support for the latest compatibility information for these platforms.

## End of Life versions

On 10/22/2015, we announced the End of Life for Platform LSF and Microsoft HPC for clustering solutions. Support will end on 12/31/2016. Please contact Microsoft if you have questions about alternatives.

Additionally, Revolution R Enterprise 6.x reached end of life in January 2016 and will stop being supported July 2016.

The following versions have reached the end of life and are no longer available or eligible for Technical Support.

- Revolution R Enterprise 5.0
- Revolution R Enterprise 4.3 & 4.2
- RevoDeployR 1.2 & 1.1
