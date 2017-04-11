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

Microsoft R Server installation on CDH is enhanced in 9.1.0 in two important ways. First, instead of using pre-built parcels downloaded from Microsoft, you can generate your own parcel using a new generate_mrs_parcel.sh script. Secondly, we added support for Custom Service Descriptors (CSDs). You can now deploy, configure, and monitor R Server in CDH as a managed service in Cloudera Manager.

If you have prior experience installing R Server on CDH, the new workflow consists of the following steps:

1. [Run the generate_mrs_parcel.sh script to create a parcel file](rserver-install-cloudera-generate-parcel.md)
2. [Add Microsoft R Server as a service using a Custom Service Descriptor (CSD)](rserver-install-cloudera-deploy-activate.md)

Download and unpacking the distribution is unchanged. Instructions are include with the parcel generation article.

**Feature restrictions in a parcel installation**

R Server includes two packages, `MicrosoftML` and `mrsdeploy`, that either cannot be included in the parcel, or included only if the underlying operating system is a specific platform and version.

+ `MicrosoftML` is conditionally available. The package can be included in the parcel if the underlying operating system is CentOS/RHEL 7.x. If CDH runs on any other operating system, such as Ubuntu or SUSE, the `MicrosoftML` package cannot be included.

+ `mrsdeploy` is excluded from a parcel installation. This package has a .NET Core dependency and cannot be added to a parcel.

The workaround is to perform a manual installation of individual packages. For instructions, see [Manual package installation](rserver-hadoop-manual-package.md).

## See Also

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)

[Configure R Server to operationalize analytics](operationalize/configuration-initial.md)
