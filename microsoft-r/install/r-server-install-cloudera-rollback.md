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

# Rollback to previous version

**Applies to:** R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)

If you deployed and activated Microsoft R Server using a parcel and Custom Service Descriptor (CSD), you have the option of rolling back the active deployment in Cloudera Manager. You might do this if there is an older version of R Server in your cluster that you want to use instead.

> [!Note]
> You can have multiple versions of R Server in Cloudera, but only can be active at any given time.

1. In Cloudera Manager, click the Parcel icon to open the parcel list.

2. Find MRS and click **Deactivate**.

The parcel still exists, but R Server is not operational in the cluster.

The above steps only work for 9.1.0 and later. If you have an older version of R Server, see [Install R Server 9.0.1 on CDH](r-server-install-cloudera-901.md) for information about how it was installed.

## See Also

[Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](r-server-install-cloudera.md)
