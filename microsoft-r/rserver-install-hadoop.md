---

# required metadata
title: "Hadoop installation and configuration guide for Microsoft R Server"
description: "Hadoop installation and configuration guide for Microsoft R Server version 8.0.0."
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/14/2016"
ms.topic: ""
ms.prod: "rserver"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# Hadoop installation and configuration guide for Microsoft R Server

Microsoft R Server is a scalable data analytics server that can be deployed as a single-user workstation, a local network of connected servers, or on a cluster in the cloud. This article explains how to configure a Hadoop cluster for use with Microsoft R Server.

## Links

[Install Microsoft R 8.0.0 on Hadoop](rserver-install-hadoop-800.md)

[Adjust your Hadoop cluster configuration for R server workloads](rserver-install-hadoop-configuration-r-workloads.md)

[Create an R package parcel for Hadoop cluster using Cloudera Manager](rserver-install-hadoop-create-r-package-cloudera-manager.md)

[Troubleshoot Microsoft R installation problems on Hadoop](rserver-install-hadoop-troubleshoot.md)

## Download R Server

Microsoft R Server consists of two parts: Microsoft R Open for Microsoft R Server (open source), and Microsoft R Server 2016 (proprietary). Each part is downloaded separately:

- [Microsoft R Open for Microsoft R Server](http://go.microsoft.com/fwlink/?LinkID=699383&clcid=0x409)
- Microsoft R Server 2016 is available through the following distribution channels, depending upon how you purchased the product:

    - [Volume Licensing Service Center](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) (VLSC)
    - [MSDN subscription](http://go.microsoft.com/fwlink/?LinkId=717967&clcid=0x409)
    - [Visual Studio Dev Essentials](http://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409)

Microsoft R Open for Microsoft R Server is distributed as an rpm file (or, if you are installing via Cloudera Manager, a Cloudera Manager parcel file).

Microsoft R Server is distributed in two different formats. Through VLSC, it is in the form of a DVD img file. Through MSDN or Dev Essentials, it is a tar.gz file.
