---
# required metadata
title: "Install Machine Learning Server for Hadoop"
description: "How to install, connect to, and use Machine Learning Server on a Hadoop cluster with Spark or MapReduce"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/15/2017"
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

# Install Machine Learning Server for Hadoop

On a Hadoop cluster, Machine Learning Server must be installed on all data nodes. Optionally, you can install [operationalization features](../operationalize/concept-operationalize-deploy-consume.md) on edge nodes.

## Per-data node installation

## Edge node installs

[Operationalization](../operationalization/concept-operationalize-deploy-consume.md) features relevant to Hadoop include remote interactive sessions and the ability to deploy and consume compiled script as a Web service. To use these features in Hadoop, install the packages providing operationalization functionality on an edge node. Operationalization does not support Yarn queues and cannot run in a distributed manner.

## Next steps

## See also

  [Install on Linux](machine-learning-server-linux-install.md)