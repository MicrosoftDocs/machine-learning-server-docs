---
# required metadata
title: "Linux offline installation for Machine Learning Server 9.2.1 | Microsoft Docs"
description: "How to install Machine Learning Server for Linux with no internet connection."
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

# Offline installation of Machine Learning Server for Linux 

By default, installers connect to Microsoft download sites to get required and updated components for Machine Learning Server 9.2.1 for Linux. If firewall restrictions or constraints on internet access prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

Before you start, review the following article for requirements and restrictions:

+ [Install Machine Learning Server 9.2.1 on Linux](machine-learning-server-linux-install.md) for an internet-connected installation.

> [!Note] 
> There is no install.sh script in this release.

## Downloads

On an internet-connected computer, download all of the following packages.

<a name="file-list"></a>

| Package | Download | Used for | 
|-----------|----------|----------|
|TBD 1 | TBD2 | R Server |
|TBD 1 | TBD2 | Python |
|TBD 1 | TBD2 | Pre-trained models, R or Python  |

## Transfer and place files

Use a tool or device to transfer the files the offline server. Extract the zipped executable for setup. Place files in the following locations:

+ TBD

## Run setup

After files are placed, run setup following the same procedure as articulated for internet-connected setup:

+ [Install Machine Learning Server > How to install](machine-learning-server-linux-install.md#how-to-install)

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

### See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)