---

# required metadata
title: "Supported Platforms for Machine Learning Server and Microsoft R Server "
description: "A list of the operating systems supported by editions and versions of Machine Learning Server and Microsoft R Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "01/29/2018"
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

This article specifies supported operating systems, distributions, and database platforms for all supported versions of Machine Learning Server and Microsoft R Server.

Machine Learning Server is available **on-premises** on Windows, Linux, Hadoop Spark, and SQL Server. It is also **[in the cloud](machine-learning-server-in-the-cloud.md)** on Azure Machine Learning Server VMs, SQL Server VMs, Data Science VMs, and on Azure HDInsight for Hadoop and Spark. In Public Preview, you can get Machine Learning Server on Azure SQL DB, Azure Machine Learning, and Azure Data Lake Analytics.

This article identifies the supported platforms for on-premises installations.

> [!Note]
> 64-bit operating systems with x86-compatible Intel architecture (commonly known as AMD64, Intel64, x86-64, IA-32e, EM64T, or x64 chips) are required on all platforms. Itanium-architecture chips (also known as IA-64) are not supported. Multiple-core chips are recommended.

## Machine Learning Server 9.3

R support includes [Microsoft R Open 3.4.3](https://mran.microsoft.com/open/), which is based on R-3.4.3. Python support is based on Anaconda 4.2 over Python 3.5. 

[Operationalization features](../what-is-operationalization.md) have a .NET Core platform dependency, which is not available in some operating systems.

| Operating system or platform | SKU | Operationalization |
|------------------------------|-----|--------------------|
| Windows 7 SP1, Windows 8.1, Windows 10 | [Machine Learning Server for Windows <br>(developer edition)](machine-learning-server-windows-install.md) | No |
| Windows Server 2012 R2, Windows Server 2016 | [Machine Learning Server for Windows](machine-learning-server-windows-install.md) | Yes |
| CentOS/ RedHat 6.x - 7.x  | [Machine Learning Server for Linux](machine-learning-server-linux-install.md#redhat) | 6.x - No <br>7.x - Yes |
| Ubuntu 14.04 and 16.04| [Machine Learning Server for Linux](machine-learning-server-linux-install.md#ubuntu)| No | 
| SUSE Linux Enterprise Server 11 | [Machine Learning Server for Linux](machine-learning-server-linux-install.md#suse)| No |
| Cloudera CDH 5.7-5.12 | [Cloudera Manager installation](machine-learning-server-cloudera-install.md) | Edge nodes only |
| Hortonworks HDP 2.4-2.6| [Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md) | Edge nodes only |
| MapR 5.0-5.2 | [Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md)| Edge nodes only |

You can install Machine Learning Server on open source Apache Hadoop from [http://hadoop.apache.org](http://hadoop.apache.org) but we can only offer support for commercial distributions.

We support Apache Spark 2.0 and 2.4 through a Hadoop distribution on CDH, HDP, or MapR. 



## Machine Learning Server 9.2.1

R support includes [Microsoft R Open 3.4.1](https://mran.microsoft.com/open/), which is based on R-3.4.1. Python support is based on Anaconda 4.2 over Python 3.5. 

| SKU | Platforms |
|-----|-----------|
| [Machine Learning <br/>Server for Hadoop](machine-learning-server-hadoop-install.md) | Hadoop Distributions: [Cloudera CDH 5.7-5.11](machine-learning-server-cloudera-install.md), Hortonworks HDP 2.4-2.6, MapR 5.0-5.2 <br/>You can install Machine Learning Server on open source Apache Hadoop from [http://hadoop.apache.org](http://hadoop.apache.org) but we can only offer support for commercial distributions.<br/><br/>Operating Systems: Red Hat Enterprise Linux 6.x and 7.x, SUSE Linux Enterprise Server 11 **<sup><big>1</big></sup>**, Ubuntu 14.04 and 16.04 <br/><br/>Spark 2.0 and 2.4 through a Hadoop distribution on CDH, HDP, or MapR. <br/><br/>Machine Learning Server can be configured on CentOS/RHEL 7.x or Ubuntu to operationalize on edge nodes only.|
| [Machine Learning <br/>Server for Linux](machine-learning-server-linux-install.md) | Red Hat Enterprise Linux  and CentOS 6.x **<sup><big>1</big></sup>** and 7.x<br/>SUSE Linux Enterprise Server 11 **<sup><big>1</big></sup>**<br/>Ubuntu 14.04 and 16.04|
| [Machine Learning <br/>Server&nbsp;for&nbsp;Windows](machine-learning-server-windows-install.md) | Windows 7 SP1 **<sup><big>1,2</big></sup>**, Windows 8.1 **<sup><big>1,2</big></sup>**, Windows 10 **<sup><big>1,2</big></sup>** <br/>Windows Server 2012 R2, Windows Server 2016 

<sup>1</sup> **.NET Core platform dependency**: Certain features like the machine learning algorithms in the [MicrosoftML R package](../r-reference/microsoftml/microsoftml-package.md) and configuring to [operationalize your analytics](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) are NOT supported on the marked (1) platforms since they require .NET Core.

<sup>2</sup> Use a **server OS for operationalizing analytics.** We do NOT recommend that you [operationalize your analytics](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) on non-server platforms such as these marked (2). While some might work, only server platforms are supported. See the full list of supported platforms for operationalizing [here](../operationalize/configure-start-for-administrators.md#supported-platforms).


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


## End of Life versions

For R Server 8.0, support ended on 1/1/2018. Please contact Microsoft if you have questions about alternatives. This version is no longer available for support.
