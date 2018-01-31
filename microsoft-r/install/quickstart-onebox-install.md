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

# How to install and configure a "one-box" Machine Learning Server on Windows

Post-install configuration tasks enable the full range of Machine Learning Server capabilities. The simplest appraoch is a "one-box" configuration that consolidates web and compute nodes on a single machine.

This quickstart uses the new [Azure Command Line Interface (CLI) for Machine Learning](../operationalize/configure/admin-cli-launch.md) introduced in the Machine Learning Server 9.3 release. This utility is installed on the server and runs locally, similar to the Azure CLI used for managing cloud resources from desktop workstations behind the firewall.

After completing these tasks, you will have a fully operational R and Python development environment for Machine Learning Server.

> [!div class="checklist"]
> * Download and install Machine Learning Server (free developer edition) on Windows
> * Use the Administrator command line utility for one-box configuration
> * Check configuration settings and validate node status
> * Deploy a web service
> * Evaluate web service capacity and run diagnostics

Start with Windows Server
    Use 2012 R2 or Windows 2016 on-premises
    - Or - [provision a Windows Server 2016 virtual machine on Azure](https://azure.microsoft.com/free/services/virtual-machines/).

Download the developer edition of Machine Learning Server

Run ServerSetup.exe

Run Admin CL
    Admin prompt
    node configuration is first --> onebox
    password-protect is for admin operations - user account is "admin"
    list nodes
    node diagnostics

VS for Python (download, configure, test)

VS for R (download, configure, test)

Next steps