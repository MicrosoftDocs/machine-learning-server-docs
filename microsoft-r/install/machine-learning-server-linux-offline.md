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

<a name="file-list"></a>

## Package downloads

On an internet-connected computer, download the following Microsoft packages:

    dotnet-host
    dotnet-hostfxr-1.1.0
    dotnet-sharedframework-microsoft.netcore.app-1.1.2 
    
    microsoft-mlserver-adminutil-9.2
    microsoft-mlserver-all-9.2.1 
    microsoft-mlserver-computenode-9.2
    microsoft-mlserver-config-rserve-9.2 
    microsoft-mlserver-hadoop-9.2.1
    microsoft-mlserver-mlm-py-9.2.1 
    microsoft-mlserver-mlm-r-9.2.1
    microsoft-mlserver-mml-py-9.2.1
    microsoft-mlserver-mml-r-9.2.1
    microsoft-mlserver-packages-py-9.2.1 
    microsoft-mlserver-packages-r-9.2.1
    microsoft-mlserver-python-9.2.1 
    microsoft-mlserver-webnode-9.2
    microsoft-r-open-foreachiterators-3.4.1 
    microsoft-r-open-mkl-3.4.1
    microsoft-r-open-mro-3.4.1 

## Package dependencies

rpm -qpR {.rpm-file}
rpm -qR {package}

You might also need the following additional packages:

    fontconfig
    fontconfig-config 
    fonts-dejavu-core 
    libcairo2 
    libdatrie1 
    libfontconfig1
    libgomp1 
    libgraphite2-3 
    libharfbuzz0b 
    libice6 
    liblttng-ust-ctl2
    liblttng-ust0 
    libpango-1.0-0 
    libpango1.0-0 
    libpangocairo-1.0-0
    libpangoft2-1.0-0 
    libpangox-1.0-0 
    libpangoxft-1.0-0 
    libpixman-1-0 
    libsm6
    libthai-data 
    libthai0 
    libunwind8 
    liburcu1 
    libxcb-render0 
    libxcb-shm0 
    libxft2
    libxrender1 
    libxt6 
    x11-common

## Transfer and place files

Use a tool or device to transfer the files the offline server. Place files in the following locations:

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