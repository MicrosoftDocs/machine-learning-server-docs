---

# required metadata
title: "Supported Platforms for Microsoft R Server"
description: "A list of the operating systems supported by editions and versions of Microsoft R Server and Revolution R Enterprise."
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/20/2016"
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

On 10/22/2015, we announced the End of Life for Platform LSF and Microsoft HPC for clustering solutions. Support will end on 12/31/2016. Please contact Microsoft if you have questions about alternatives.

Additionally, Revolution R Enterprise 6.x reached end of life in January 2016 and will stop being supported July 2016.

## Microsoft R Server 8.0.5

Server Platforms <sup>4</sup>  | Architecture
------------------------------|--------------
Red Hat Enterprise Linux (RHEL) 6.x and 7.x, or CentOS | 64-Bit  
SUSE Linux Enterprise Server 11 (SLES11) and later | 64-Bit

**Microsoft R Server for Teradata**

- Teradata Database 14.10, 15.00, 15.10
- Operating System: SLES11 and later

**Microsoft R Server for Hadoop**

- Hadoop Distributions: Cloudera CDH 5.5-5.7, Hortonworks HDP 2.3-2.4, MapR 5.0-5.1
- Operating Systems: RHEL 6.x and 7.x, SUSE SLES11
- Spark version: 1.5.0-1.6.1 (if using Spark)

## Microsoft R Server 8.0.0

Microsoft R Server 8.0.0, formerly known as "Revolution R Enterprise", is supported on the platforms listed below.

Server Platforms <sup>4</sup>  | Architecture
------------------------------|--------------
Windows 7 through 10	| 64-Bit
Windows Server 2012  |  64-Bit
Red Hat Enterprise Linux - 5.x and  6.x <sup>4</sup> <sup>6</sup> | 64-Bit
SUSE Linux Enterprise Server 11 SP2, SP3 <sup>4</sup> <sup>6</sup> | 64-Bit

**Hadoop Distributions <sup>4</sup>**

- Cloudera CDH 5.0 - 5.4 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>
- MapR M3 v3.1, 3.1.1, 4.0.1, 4.0.2, 4.0.3 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup> <sup>10</sup>
- MapR M5 v3.1, 3.1.1, 4.0.1, 4.0.2, 4.0.3 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup> <sup>10</sup>
- MapR M7 v3.1, 3.1.1, 4.0.1, 4.0.2, 4.0.3 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup> <sup>10</sup>

**EDW Platforms**

- Teradata Database 14.10/SLES 10.x <sup>6</sup> <sup>9</sup>
- Teradata Database 14.10/SLES 11.x <sup>6</sup> <sup>9</sup>
- Teradata Database 15.0, 15.10/SLES 10.x <sup>6</sup> <sup>9</sup>
- Teradata Database 15.0, 15.10/SLES 11.x <sup>6</sup> <sup>9</sup>

Additional Hadoop Distributions Supported as HDFS Data Sources via ConnectR

- IBM Big Insights 1.4.1, 2.0 HDFS <sup>7</sup>
- Intel Hadoop(IDH) 1.03, 2.2 HDFS <sup>7</sup>
- Pivotal HD 1.0 HDFS <sup>7</sup>

<sup>1</sup> DevelopR is only supported on Windows platforms or Windows emulators, and accessible via Windows Remote Desktop for remote Windows servers.

<sup>2</sup> On platforms with distributed computing capabilities: clusters, grids, Hadoop and appliances.

<sup>3</sup> DeployR supports the following browsers:  Internet Explorer 8.0 and higher, Mozilla Firefox 35.0.1 and higher, Google Chrome 28.0 and higher.

<sup>4</sup> Linux platforms require 64-bit processor with x86-compatible architecture

<sup>5</sup> MRO is now an add-on to separately installed or built R 3.2.2.

<sup>6</sup> Install DevelopR on a separate Windows workstation.

<sup>7</sup> Includes technical support for the open-source RHadoop project.

<sup>8</sup> Install DeployR on a Hadoop edge node or additional Linux or Windows server.

<sup>9</sup> Install DeployR on a separate Linux or Windows server.

<sup>10</sup> MapR 4.0.2 requires the mapr-patch-4.0.2.29870.GA-30600; contact MapR to obtain the patch.

## Revolution R Enterprise 7.4 and 7.4.1 Workstation

Revolution R Enterprise Workstation is licensed for a single named user, and available in two editions: Entry and Power. DevelopR is only supported on Windows platforms.

- Revolution Enterprise Workstation — Entry Edition: One desktop or laptop, 1-4 cores.
- Revolution Enterprise Workstation — Power Edition: One desktop or laptop, 5-8 cores. For machines with more than 8 cores, use Revolution R Enterprise Server.
- Revolution R Enterprise Workstation is supported on the following platforms:
    - 64-Bit Windows 7
    - 64-Bit Windows 8
    - 64-Bit Red Hat Enterprise Linux 5.x, 6.x

## Revolution R Enterprise 7.4 and 7.4.1 Server

Revolution R Enterprise 7 Server is available in two editions: Premium and Premium Plus. The table below lists the features included in each edition.

Feature | Premium | Premium Plus
---------|---------|------------
RevoR | Included | Included
DevelopR <sup>1</sup> | Included | Included
ConnectR | Included | Included
ScaleR | Included | Included
DistributedR <sup>2</sup> | Included | Included
DeployR <sup>3</sup>	 | Included | -
Standard Support | Included | -
Premium Support	Available <sup>3</sup>| Included | -

Revolution R Enterprise 7.4 Server is supported on the platforms listed below. Note that some editions are not supported on all platforms.

Server Platforms <sup>4</sup> | Premium | Premium Plus
---|---|---
64-Bit Windows 7 | Supported | Supported
64-Bit Windows 8 | Supported | Supported
64-Bit Windows Server 2008 SP2 | Supported | Supported
64-Bit Windows Server 2012 | Supported | Supported
64-Bit Red Hat Enterprise Linux 5.0, 5.1, 5.2, 5.3 <sup>4</sup> <sup>6</sup> | Supported | Supported
64-Bit Red Hat Enterprise Linux 5.4 through 5.9 <sup>4</sup> <sup>6</sup> | Supported | Supported
64-Bit Red Hat Enterprise Linux 6.x <sup>4</sup> <sup>6</sup>	 | Supported | Supported
64-Bit SUSE Linux Enterprise Server 11 SP3 <sup>4</sup> <sup>6</sup> | Supported | Supported

Clusters and Grids | Premium | Premium Plus
---|---|---
Platform LSF 7, 9 on 64-bit RHEL 5.0, 5.1, 5.2, 5.3 <sup>4</sup> <sup>6</sup> | Supported | Supported
Platform LSF 7, 9 on 64-bit RHEL 5.4 through 5.9 <sup>4</sup> <sup>6</sup> | Supported | Supported
Platform LSF 7, 9 on 64-bit RHEL 6.x <sup>4</sup> <sup>6</sup> | Supported | Supported
64-Bit Windows Server 2008 SP2/HPC Pack 2008 | Supported | Supported

Hadoop Distributions <sup>4</sup> | Premium | Premium Plus
---|---|---
Cloudera CDH 5.0, 5.1, 5.2, 5.3 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
Hortonworks HDP 1.3 on RHEL 5.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
Hortonworks HDP 1.3 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
Hortonworks HDP 2.0, 2.1 and 2.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
MapR M3 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
MapR M5 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
MapR M7 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported

EDW Platforms | Premium | Premium Plus
---|---|---
Teradata Database 14.10/SLES 10.x <sup>6</sup> <sup>9</sup>| Supported | Supported
Teradata Database 14.10/SLES 11.x <sup>6</sup> <sup>9</sup>| Supported | Supported
Teradata Database 15.0/SLES 10.x <sup>6</sup> <sup>9</sup>| Supported | Supported
Teradata Database 15.0/SLES 11.x <sup>6</sup> <sup>9</sup>| Supported | Supported

Additional Hadoop Distributions Supported as HDFS Data Sources via ConnectR

- IBM Big Insights 1.4.1, 2.0 HDFS <sup>6</sup>
- Intel Hadoop(IDH) 1.03, 2.2 HDFS <sup>6</sup>
- Pivotal HD 1.0 HDFS <sup>6</sup>

<sup>1</sup> DevelopR is only supported on Windows platforms or Windows emulators, and accessible via Windows Remote Desktop for remote Windows servers.

<sup>2</sup> On platforms with distributed computing capabilities: clusters, grids, Hadoop and appliances.

<sup>3</sup> DeployR supports the following browsers:  Internet Explorer 8.0 and higher, Mozilla Firefox 35.0.1 and higher, Google Chrome 28.0 and higher.

<sup>4</sup> Linux platforms require 64-bit processor with x86-compatible architecture.

<sup>5</sup> Supported currently but has been deprecated for removal in subsequent releases.

<sup>6</sup> Install DevelopR on a separate Windows workstation.

<sup>7</sup> Includes technical support for the open-source RHadoop project.

<sup>8</sup> Install DeployR on a Hadoop edge node or additional Linux or Windows server.

<sup>9</sup> Install DeployR on a separate Linux or Windows server.

## Revolution R Enterprise 7.3 Workstation
Revolution R Enterprise Workstation is licensed for a single named user, and available in two editions: Entry and Power.

- Revolution Enterprise Workstation — Entry Edition: One desktop or laptop, 1-4 cores
- Revolution Enterprise Workstation — Power Edition: One desktop or laptop, 5-8 cores. For machines with more than 8 cores, use Revolution R Enterprise Server.

Revolution R Enterprise Workstation is supported on the following platforms. DevelopR is only supported on Windows platforms.

- 64-Bit Windows 7
- 64-Bit Windows 8
- 64-Bit Red Hat Enterprise Linux 5.x, 6.x

## Revolution R Enterprise 7.3 Server
Revolution R Enterprise 7 Server is available in two editions: Premium and Premium Plus. The table below lists the features included in each edition.

Feature | Premium | Premium Plus
---------|---------|------------
RevoR | Included | Included
DevelopR <sup>1</sup>  | Included | Included
ConnectR | Included | Included
ScaleR | Included | Included
DistributedR <sup>2</sup>  | Included | Included
DeployR <sup>3</sup>  | Included |   Included
Standard Support | Included |  
Premium Support	Available <sup>3</sup>  | Included |  

Revolution R Enterprise 7.3 Server is supported on the platforms listed below. Note that some platforms do not support all editions.

Server Platforms <sup>4</sup> | Premium | Premium Plus
---|---|---
64-Bit Windows 7 | Supported | Supported
64-Bit Windows 8 | Supported | Supported
64-Bit Windows Server 2008 SP2 | Supported | Supported
64-Bit Windows Server 2012 | Supported | Supported
64-Bit Red Hat Enterprise Linux 5.0, 5.1, 5.2, 5.3 <sup>4</sup> <sup>6</sup> | Supported | Supported
64-Bit Red Hat Enterprise Linux 5.4 through 5.9 <sup>4</sup> <sup>6</sup> | Supported | Supported
64-Bit Red Hat Enterprise Linux 6.x <sup>4</sup> <sup>6</sup>	 | Supported | Supported
64-Bit SUSE Linux Enterprise Server 11 SP3 <sup>4</sup> <sup>6</sup> | Supported | Supported

Clusters and Grids | Premium | Premium Plus
---|---|---
Platform LSF 7, 9 on 64-bit RHEL 5.0, 5.1, 5.2, 5.3 <sup>4</sup> <sup>6</sup> | Supported | Supported
Platform LSF 7, 9 on 64-bit RHEL 5.4 through 5.9 <sup>4</sup> <sup>6</sup> | Supported | Supported
Platform LSF 7, 9 on 64-bit RHEL 6.x <sup>4</sup> <sup>6</sup> | Supported | Supported
64-Bit Windows Server 2008 SP2/HPC Pack 2008 | Supported | Supported

Hadoop Distributions <sup>4</sup> | Premium | Premium Plus
---|---|---
Cloudera CDH 5.0, 5.1, 5.2, 5.3 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
Hortonworks HDP 1.3 on RHEL 5.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
Hortonworks HDP 1.3 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
Hortonworks HDP 2.0, 2.1 and 2.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
MapR M3 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
MapR M5 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported
MapR M7 v2.02, 3.1, 3.1.1, 4.0.1, 4.0.2 on RHEL 6.x <sup>4</sup> <sup>6</sup> <sup>8</sup>| Supported | Supported

EDW Platforms | Premium | Premium Plus
---|---|---
Teradata Database 14.10/SLES 10.x <sup>6</sup> <sup>9</sup>| Supported | Supported
Teradata Database 14.10/SLES 11.x <sup>6</sup> <sup>9</sup>| Supported | Supported
Teradata Database 15.0/SLES 10.x <sup>6</sup> <sup>9</sup>| Supported | Supported
Teradata Database 15.0/SLES 11.x <sup>6</sup> <sup>9</sup>| Supported | Supported

Additional Hadoop Distributions Supported as HDFS Data Sources via ConnectR

- IBM Big Insights 1.4.1, 2.0 HDFS <sup>6</sup>
- Intel Hadoop(IDH) 1.03, 2.2 HDFS <sup>6</sup>
- Pivotal HD 1.0 HDFS <sup>6</sup>

<sup>1</sup> DevelopR is only supported on Windows platforms or Windows emulators, and accessible via Windows Remote Desktop for remote Windows servers.

<sup>2</sup> On platforms with distributed computing capabilities: clusters, grids, Hadoop and appliances.

<sup>3</sup> DeployR supports the following browsers:  Internet Explorer 8.0 and higher, Mozilla Firefox 35.0.1 and higher, Google Chrome 28.0 and higher.

<sup>4</sup> Linux platforms require 64-bit processor with x86-compatible architecture.

<sup>5</sup> Supported currently but has been deprecated for removal in subsequent releases.

<sup>6</sup> Install DevelopR on a separate Windows workstation.

<sup>7</sup> Includes technical support for the open-source RHadoop project.

<sup>8</sup> Install DeployR on a Hadoop edge node or additional Linux or Windows server.

<sup>9</sup> Install DeployR on a separate Linux or Windows server.

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

•    Red Hat Enterprise Linux 5.x
•    Red Hat Enterprise Linux 6.x

Platform LSF 7 or later is required to use the distributed computing features of RevoScaleR.

Note: This excludes Revolution R Enterprise for IBM PureData System for Analytics, R for Hadoop and Revolution R for Teradata. Please contact Microsoft Support for the latest compatibility information for these platforms.

## End of Life versions

These versions have reached the end of life and are no longer available or eligible for Technical Support.

- Revolution R Enterprise 5.0
- Revolution R Enterprise 4.3 & 4.2
- RevoDeployR 1.2 & 1.1
