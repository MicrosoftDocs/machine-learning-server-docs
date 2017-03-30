---

# required metadata
title: "R Server installation on Cloudera CDH"
description: "Install Microsoft R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/29/2017"
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

# Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)

Microsoft R Server installation on CDH is enhanced in 9.1.0. If you have prior experience installing R Server on CDH, please review our updated installation instructions to learn about the new workflow.

## Feature installation restrictions

R Server includes two packages, `MicrosoftML` and `mrsdeploy`, that either cannot be included in the parcel, or included only if the underlying operating system is a specific platform and version.

+ `MicrosoftML` can be included in the parcel if the underlying operating system is CentOS/RHEL 7.x. 

+ `mrsdeploy` cannot be included in the parcel at all.

The workaround is to perform a manual installation of individual packages. For instructions, see TBD.

## See Also

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)

[Configure R Server to operationalize analytics](operationalize/configuration-initial.md)
