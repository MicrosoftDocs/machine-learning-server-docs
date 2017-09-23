---

# required metadata
title: "Supported Platforms for Machine Learning Server and Microsoft R Server | Microsoft Docs"
description: "A list of the operating systems supported by editions and versions of Machine Learning Server and Microsoft R Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/07/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---
# Supported platforms for Machine Learning Server and Microsoft R Server

This article specifies supported operating systems, distributions, and database platforms for all supported versions of Machine Learning Server, Microsoft R Server, Revolution R Server, and Revolution R Workstation.

> [!Note]
> 64-bit operating systems with x86-compatible Intel architecture (commonly known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips) are required on all platforms. Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

## Machine Learning Server 9.2.1

Python support is based on Anaconda 4.2 over Python 3.5. R support is based on [Microsoft R Open 3.4.1](https://mran.microsoft.com/open/), based on R-3.4.1.

| SKU | Platforms |
|-----|-----------|
| Machine Learning <br/>Server for Hadoop | Hadoop Distributions:**<sup><big>1,4</big></sup>** Cloudera CDH 5.7-5.11, Hortonworks HDP 2.4-2.6, MapR 5.0-5.2 <br/><br/>Operating Systems: Red Hat Enterprise Linux 6.x and 7.x, SUSE Linux Enterprise Server 11 **<sup><big>2,4</big></sup>**, Ubuntu 14.04 and 16.04 **<sup><big>2</big></sup>**<br/><br/>Spark **<sup><big>3</big></sup>** 2.0 and 2.4  |
| Machine Learning <br/>Server for Linux | Red Hat Enterprise Linux  and CentOS 6.x **<sup><big>4</big></sup>** and 7.x<br/>SUSE Linux Enterprise Server 11 **<sup><big>4</big></sup>**<br/>Ubuntu 14.04 and 16.04|
| Machine Learning <br/>Server&nbsp;for&nbsp;Windows | Windows 7 SP1 **<sup><big>4</big></sup>**, Windows 8.1 **<sup><big>4</big></sup>**, Windows 10 **<sup><big>4</big></sup>** <br/>Windows Server 2012 R2, Windows Server 2016 | 

<sup>1</sup> You can install Machine Learning Server for Hadoop on open source Apache Hadoop from [http://hadoop.apache.org](http://hadoop.apache.org) but we can only offer support for CDH, HDP, or MapR.

<sup>2</sup> Cloudera installation using the [built-in parcel installation script](machine-learning-server-cloudera-install.md) can execute on CentOS/RHEL 7.0. The parcel generator excludes any R Server features that it cannot install. If parcel installation is too restrictive, follow the instructions for a generic [Hadoop installation](machine-learning-server-hadoop-install.md).

<sup>3</sup> Spark integration is supported only through a Hadoop distribution on CDH, HDP, or MapR. Your version of Hadoop must include a supported level of Spark (2.0-2.4). Spark 1.6 support is dropped for this release.

<sup>4</sup> **.NET Core platform dependency**: The following features require .NET Core:
(a) configuring Machine Learning Server to [operationalize your R analytics](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization), and (b) the machine learning algorithms  in the [MicrosoftML R package](../r-reference/microsoftml/microsoftml-package.md). These features are NOT available on the marked platforms. On Hadoop, you can install these features on edge nodes, assuming the native file system is CentOS/RHEL 7.x or Ubuntu.


## Microsoft R Server 9.1

All SKUs include [Microsoft R Open 3.3.3](https://mran.microsoft.com/open/), based on R-3.3.3, and require 64-bit operating systems with x86-compatible Intel architecture.

| SKU | Platforms |
|-----|-----------|
| **R Server for Hadoop** | **Hadoop <sup> 1, 5</sup> Distributions:** <br/>Cloudera CDH 5.5-5.9, Hortonworks HDP 2.3-2.5, MapR 5.0-5.2 <br/>**Operating Systems:** <br/>RHEL 6.x and 7.x, SUSE Linux Enterprise Server 11 (SLES11) (<sup>2</sup>, <sup>4</sup>), Ubuntu 14.04 (<sup>2</sup>)<br/>**Spark <sup> 3</sup>:**  <br/>Versions 1.6 and 2.0.  |
| **R Server for Linux** | Red Hat Enterprise Linux (RHEL) and CentOS 6.x(<sup>4</sup>) and 7.x<br/>SLES11(<sup>4</sup>)<br/>Ubuntu 14.04 and 16.04|
| **R Server for Windows** | Windows 7 SP1(<sup>4</sup>), Windows 8.1(<sup>4</sup>), Windows 10(<sup>4</sup>) <br/>Windows Server 2012 R2, Windows Server 2016 | 
| **R Server for Teradata** | Teradata Database 14.10, 15.00, 15.10 on SUSE Linux Enterprise Server 11(<sup>4</sup>) |

Hardware and software requirements for SQL Server Machine Learning Services and R Server (Standalone) in SQL Server can be found in [SQL Server production documentation](https://docs.microsoft.com/sql/advanced-analytics/r-services/r-services).

<sup>1</sup> You can install **R Server for Hadoop** on open source Apache Hadoop from [http://hadoop.apache.org](http://hadoop.apache.org) but we can only offer support for R Server on CDH, HDP, or MapR.

<sup>2</sup> Cloudera installation using the built-in parcel generator script for 9.1 requires CentOS/RHEL 7.0 as the operating system. The parcel generator excludes any R Server features that it cannot install. For more information, see [Install R Server 9.1 on CDH](r-server-install-cloudera.md).

<sup>3</sup> Spark integration is supported only through a Hadoop distribution on CDH, HDP, or MapR. Not all supported versions of Hadoop include a supported level of Spark. Specifically, HDP must be at least 2.3.4 to get a supported level of Spark.

<sup>4</sup>**.NET Core platform dependency**: Several features in R Server have a .NET Core dependency. These features include [Overview of MicrosoftML algorithms](../r-reference/microsoftml/microsoftml-package.md) bundled in the MicrosoftML package as well as the ability to configure R Server to [operationalize your R analytics](../what-is-operationalization.md). Due to the .Net Core dependency, these features are NOT available on these platforms. 

<sup>5</sup>To operationalize your analytics or use the MicrosoftML package on R Server for Hadoop, you must deploy on edge nodes in a Hadoop cluster, if the underlying operating system is CentOS/RHEL 7.x or Ubuntu 14.04. It is not supported on SUSE SLES11.



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

- Hadoop Distributions: Cloudera CDH 5.5-5.9, Hortonworks HDP 2.3-2.5, MapR 5.0-5.2
- Operating Systems: RHEL 6.x and 7.x, SUSE SLES11, Ubuntu 14.04 (excluding Cloudera Parcel install on Ubuntu)
- Spark versions: 1.6 and 2.0. Not all supported versions of Hadoop include a supported level of Spark. Specifically, HDP must be at least 2.3.4 to get a supported level of Spark.

**Operationalization and mrsdeploy**

After installation, you can [configure](operationalize-r-server-one-box-config.md) R Server to [operationalize](../what-is-operationalization.md) your R analytics. Due to an ASP .Net Core dependency, operationalization is currently supported only on:
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
- [Prerequisites for DeployR on Linux](../deployr/deployr-install-on-linux.md#system-requirements)
- [Prerequisites for DeployR on Windows](../deployr/deployr-install-on-windows.md#system-requirements)

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


## End of Life versions

On 10/22/2015, we announced the End of Life for Platform LSF and Microsoft HPC for clustering solutions. Support ended on 12/31/2016. Please contact Microsoft if you have questions about alternatives.

The following versions have reached the end of life and are no longer available or eligible for Technical Support.

- Revolution R Enterprise 7.x
- Revolution R Enterprise 6.x
- Revolution R Enterprise 5.0
- Revolution R Enterprise 4.3 & 4.2
- RevoDeployR 1.2 & 1.1
