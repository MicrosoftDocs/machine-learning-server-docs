---
# required metadata
title: "How to install and configure one-box Machine Learning Server"
description: "Quickstart tutorial for installation and configuration of Machine Learning server on Windows"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
ms.date: "02/16/2018"
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

# How to install and configure a "one-box" development environment for Machine Learning Server on Windows

Build a development machine from the ground up, with full access to all of Machine Learning Server. Post-install configuration tasks enable operationalization features like web service deployment, diagnostics, and capacity testing. 

This quickstart uses the new [Azure Command Line Interface (CLI) for Machine Learning](../operationalize/configure-admin-cli-launch.md) introduced in the 9.3 release for post-installation tasks. The simplest approach is a "one-box" configuration that consolidates web and compute nodes on a single machine.

> [!div class="checklist"]
> * Download and install Machine Learning Server (free developer edition) on Windows
> * Use the Administrator command line utility for one-box configuration
> * Check configuration settings and validate node status
> * Add R Tools for Visual Studio
> * Add Python to Visual Studio
> * Deploy a web service
> * Evaluate web service capacity and run diagnostics

## Prerequisites

Start with / Download the following software. 

1. Windows Server
    Use 2012 R2 or Windows 2016 on-premises
    - Or - [provision a Windows Server 2016 virtual machine on Azure](https://azure.microsoft.com/free/services/virtual-machines/).

2. Download Visual Studio 2017 Community 

3. Download Developer edition of Machine Learning Server

## Install Machine Learning Server

Run ServerSetup.exe

## Configure one-box
Run Admin CL

This utility is installed on the server and runs locally, similar to the Azure CLI used for managing cloud resources from desktop workstations behind the firewall.

    Admin prompt
    node configuration is first --> onebox
    password-protect is for admin operations - user account is "admin"
    list nodes
    node diagnostics

## Install Visual Studio (IDE)

VS for Python (download, configure, test)

VS for R (download, configure, test)

## Web service deployment

    service diagnostics

## Next steps