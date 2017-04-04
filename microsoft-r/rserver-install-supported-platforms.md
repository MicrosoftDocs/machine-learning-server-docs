---

# required metadata
title: "Supported Platforms for Microsoft R Server"
description: "A list of the operating systems supported by editions and versions of Microsoft R Server."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/06/2017"
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
# Supported Platforms for Microsoft R Server

This article specifies supported operating systems, distributions, and database platforms for all supported versions of Microsoft R Server, Revolution R Server, and Revolution R Workstation.

> [!Note]
> 64-bit operating systems with x86-compatible Intel architecture (commonly known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips) are required on all platforms. Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

## Microsoft R Server 9.1.0

All SKUs include [Microsoft R Open 3.3.3](r-open.md), based on R-3.3.3, and require 64-bit operating systems with x86-compatible Intel architecture.

| SKU | Platforms |
|-----|-----------|
| **R Server for Hadoop** | *Hadoop Distributions:* <sup>1</sup><br/>Cloudera CDH 5.5-5.8, Hortonworks HDP 2.3-2.5, MapR 5.0-5.2 <br/>*Operating Systems:* <br/>RHEL 6.x and 7.x, SUSE SLES11 (minus some features <sup>2</sup> ), Ubuntu 14.04 (minus some features <sup>2</sup> )<br/>*Spark:* <sup>3</sup> <br/>Versions 1.6 and 2.0.  |
| **R Server for Linux** | Red Hat Enterprise Linux (RHEL) and CentOS 6.x and 7.x<br/>SUSE Linux Enterprise Server 11 (SLES11)<br/>Ubuntu 14.04 and 16.04|
| **R Server for Windows** | Windows 7 SP1, Windows 8.1, Windows 10 <br/>Windows Server 2012 R2, Windows Server 2016 | 
| **R Server for Teradata** | Teradata Database 14.10, 15.00, 15.10 on SUSE Linux Enterprise Server 11 (SLES11) |

Hardware and software requirements for SQL Server R Services and R Server (Standalone) in SQL Server can be found in[SQL Server production documentation](https://docs.microsoft.com/sql/advanced-analytics/r-services/r-services).

<sup>1</sup> You can install **R Server for Hadoop** on open source Apache Hadoop from [http://hadoop.apache.org](http://hadoop.apache.org) but we only support R Server on CDH, HDP, and MapR.

<sup>2</sup> Cloudera installation using the built-in parcel generator script for 9.1.0 requires CentOS/RHEL 7.0 as the operating system. The parcel generator excludes any R Server features that it cannot install. For more information, see [Install R Server 9.1.0 on CDH](rserver-install-hadoop-910-cdh.md).

<sup>3</sup> Spark integration is supported only through a Hadoop distribution on CDH, HDP, or MapR. Not all supported versions of Hadoop include a supported level of Spark. Specifically, HDP must be at least 2.3.4 to get a supported level of Spark.

## Microsoft R Server 9.0.1

This release of R Server is built atop _Microsoft R Open 3.3.2_, which is based on R-3.3.2.


**Microsoft R Server (Windows or Linux)**

- Windows 7 SP1, Windows 8.1, Windows 10, Windows Server 2012 R2, and Windows Server 2016
- Red Hat Enterprise Linux (RHEL) and CentOS 6.x and 7.x  
- SUSE Linux Enterprise Server 11 (SLES11)
- Ubuntu 14.04 and 16.04

**Microsoft R Server for Teradata**

- Teradata Database 14.10, 15.00, 15.10
- Operating System: SLES11

**SQL Server R Services or R Server (Standalone)**

- [SQL Server vNext CTP 1.1](https://msdn.microsoft.com/library/mt788653.aspx)
- Operating System: Windows (see [SQL Server hardware and software requirements](https://msdn.microsoft.com/library/ms143506.aspx) for details)

**Microsoft R Server for Hadoop**

- Hadoop Distributions: Cloudera CDH 5.5-5.8, Hortonworks HDP 2.3-2.5, MapR 5.0-5.2
- Operating Systems: RHEL 6.x and 7.x, SUSE SLES11, Ubuntu 14.04 (excluding Cloudera Parcel install on Ubuntu)
- Spark versions: 1.6 and 2.0. Not all supported versions of Hadoop include a supported level of Spark. Specifically, HDP must be at least 2.3.4 to get a supported level of Spark.

**Operationalization and mrsdeploy**

After installation, you can [configure](operationalize/configuration-initial.md) R Server to [operationalize](operationalize/about.md) your R analytics. Due to an ASP .Net Core dependency, operationalization is currently supported only on:
+ Windows: Windows Server 2012 R2, Windows Server 2016
+ Linux: CentOS/RHEL 7.x, Ubuntu 14.04 and 16.04 _Note: Projected availability on SLES in 2017_
+ Hadoop: Linux-based edge nodes on CentOS/RHEL 7.x, Ubuntu 14.04

> [!Note]
> R Server operationalization is not available on Windows if you use the SQL Server installer. You must use the standalone Windows installer to get operationalization functionality.

## Microsoft R Server 8.0.5

This release of R Server is built atop _Microsoft R Open 3.3.2_, which is based on R-3.3.2.

**Operating Systems (64-Bit only)**

- Red Hat Enterprise Linux (RHEL) 6.x and 7.x, or CentOS   
- SUSE Linux Enterprise Server 11 (SLES11)

**Microsoft R Server for Teradata**

- Teradata Database 14.10, 15.00, 15.10
- Operating System: SLES11

**Microsoft R Server for Hadoop**

- Hadoop Distributions: Cloudera CDH 5.5-5.7, Hortonworks HDP 2.3-2.4, MapR 5.0-5.1
- Operating Systems: RHEL 6.x and 7.x, SUSE SLES11
- Spark version: 1.5.0-1.6.1 (if using Spark). Not all supported versions of Hadoop include a supported level of Spark. Specifically, HDP must be at least 2.3.4 to get a supported level of Spark.

**DeployR**
- [Prerequisites for DeployR on Linux](deployr-install-on-linux.md#system-requirements)
- [Prerequisites for DeployR on Windows](deployr-install-on-windows.md#system-requirements)

## Microsoft R Server 8.0.3

This release of R Server is built atop _Microsoft R Open 3.3.2_, which is based on R-3.3.2.

This Windows-only release as Microsoft R Server for Windows is part of SQL Server 2016. Learn more about [Microsoft R Server for Windows](https://msdn.microsoft.com/library/mt674874.aspx). The installation instructions are covered in this [setup guide](https://msdn.microsoft.com/library/mt695941.aspx).

## Microsoft R Server 8.0

This release of R Server is built atop _Microsoft R Open 3.3.2_, which is based on R-3.3.2.

Microsoft R Server 8.0, formerly known as "Revolution R Enterprise", is supported on the platforms listed below.

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

This release of R Server is built atop of _Revolution R Open 8.0.3_, which is based on R-3.1.3.

Revolution R Enterprise Workstation is licensed for a single named user, and available in two editions: Entry and Power. DevelopR is only supported on Windows platforms.

- Revolution Enterprise Workstation — Entry Edition: One desktop or laptop, 1-4 cores.
- Revolution Enterprise Workstation — Power Edition: One desktop or laptop, 5-8 cores. For machines with more than 8 cores, use Revolution R Enterprise Server.
- Revolution R Enterprise Workstation is supported on the following platforms:
    - 64-Bit Windows 7
    - 64-Bit Windows 8
    - 64-Bit Red Hat Enterprise Linux 5.x, 6.x

## Revolution R Enterprise 7.4 and 7.4.1 Server

This release of R Server is built atop of _Revolution R Open 8.0.3_, which is based on R-3.1.3.

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

This release of R Server is built atop of R-3.1.1.

Revolution R Enterprise Workstation is licensed for a single named user, and available in two editions: Entry and Power.

- Revolution Enterprise Workstation — Entry Edition: One desktop or laptop, 1-4 cores
- Revolution Enterprise Workstation — Power Edition: One desktop or laptop, 5-8 cores. For machines with more than 8 cores, use Revolution R Enterprise Server.

Revolution R Enterprise Workstation is supported on the following platforms. DevelopR is only supported on Windows platforms.

- 64-Bit Windows 7 through 8
- 64-Bit Red Hat Enterprise Linux 5.x, 6.x

## Revolution R Enterprise 7.3 Server

This release of R Server is built atop of R-3.1.1.

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

The following versions have reached the end of life and are no longer available or eligible for Technical Support.

- Revolution R Enterprise 6.x
- Revolution R Enterprise 5.0
- Revolution R Enterprise 4.3 & 4.2
- RevoDeployR 1.2 & 1.1
