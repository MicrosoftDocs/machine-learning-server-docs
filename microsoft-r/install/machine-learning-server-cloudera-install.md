---
# required metadata
title: "Install Machine Learning Server on Cloudera"
description: "How to install, connect to, and use Machine Learning Server on a Cloudera Hadoop disribution."
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

# Install Machine Learning Server using Cloudera Manager

Cloudera offers a parcel installation methodology for adding services and features to a cluster. This article explains how to generate, deploy, and activate an installation parcel for Machine Learning Server 9.2.1 on a Cloudera distribution of Apache Hadoop (CDH). 

On a Hadoop cluster, Machine Learning Server is installed on all data nodes. You can use a generated parcel to distribute Machine Learning Server packages across all the nodes of your CDH cluster.

## Requirements

The Machine Learning Server parcel installation script can be generated on any [supported version of Linux](r-server-install-supported-platforms.md), but must execute on RHEL or CentOS 7.0 as the native operating system. 

If your operating system is not 7.0, or if you want to add [operationalization features](../operationalize/concept-operationalize-deploy-consume.md) on edge nodes, use the regular [Hadoop installation instuctions](machine-learning-server-hadoop-install.md) instead.

## Preparation

+ Machine Learning Server 9.2.1 for Hadoop, downloaded to a writable directory such as /tmp/, and unpacked.

+ Parcel name is `MLServer-9.2.1` (used to be MRS-9.2.1)
+ Parcel generator is `generate_mlserver_parcel.sh`
+ The CSD is also called `MLServer`

Python is a part of the Parcel installer, controlled by the command line option, `-l` or `--add-mml`.

## Parcel generation

## Parcel execution

## Rollback (optional)

## Next steps

## See also


[Install R Server 9.1 on the Cloudera distribution](r-server-install-cloudera.md)