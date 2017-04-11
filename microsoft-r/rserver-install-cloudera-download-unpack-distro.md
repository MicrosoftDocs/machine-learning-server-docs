---

# required metadata
title: "Download and unpack an R Server distribution on Cloudera CDH"
description: "Download and unpack an R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/10/2017"
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

# Download and unpack an R Server distribution on Cloudera CDH

If you are using a parcel installer to deploy R Server 9.1.0 using Cloudera Manager, the first step is to download and unpack the distribution on the master node.

## Download R Server installer

Get the zipped RServerSetup installer file from one of the following download sites.

| Site | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios. <br/><br/>1. Click **Join or Access Now** and enter your account information.<br/>2. Make sure you're in the right place: *my.visualstudio.com*.<br/>3. Click **Downloads**, and then search for *Microsoft R*. |
|[Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for R Server for Hadoop. A selection for **R Server 9.1.0 for Hadoop** is provided on this site. |
| [MSDN subscription downloads](https://msdn.microsoft.com/subscriptions/downloads/hh442898.aspx) | Developer or Enterprise | Subscribers can download software at given subscription levels. Depending on your subscription, you can get either edition. |

## Unpack the distribution

Download the software to a writable directory, such as **/tmp**, unpack the distribution and then run the installation script.

The distribution includes one installer for Microsoft R Server. For a gzipped TAR file, you should unpack the file as follows (be sure you have downloaded the file to a writable directory, such as **/tmp**):

1. Log in as root or a user with super user privileges (`sudo su`).
2. Switch to the **/tmp** directory (assuming it's the download location)
3. Unpack the file:
        `[tmp] $ tar zxvf microsft-r-server-9.1.0.tar.gz`


## See Also

[Troubleshoot R Server installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)

[Configure R Server to operationalize analytics](operationalize/configuration-initial.md)
