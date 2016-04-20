
---

# required metadata
title: "What's New in DeployR"
description: "Updates, improvements, and changes in this release of DeployR"
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "03/17/2016"
ms.topic: "article"
ms.prod: "deployr"
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

# DeployR Release Notes

## What's New in DeployR

This topic presents the changes and new features in DeployR version 8.0.0 as well as several prior releases.

-   [Release 8.0.0](#version-8-0-0)
-   [Release 7.4.1](#version-7-4-1)
-   [Release 7.4.0](#version-7-4-0)
-   [Release 7.3](#version-7-3)

### Version 8.0.0

> DeployR 8.0.0 is now available for [download](https://deployr.revolutionanalytics.com/download).

DeployR 8.0.0 has been rebranded for distribution via Microsoft Corporation.

## Past Releases

For historical purposes, we've included the list of changes and additions from several releases prior to DeployR 8.0.0.

### Version 7.4.1

The following list highlights the major changes and improvements to DeployR 7.4.1.

-   DeployR Enterprise now supports for RRE 7.4.1.
-   DeployR Open now supports R 3.2.x and RRO 3.2.x.
-   DeployR is now Java 8 compatible.
-   Windows 10 is now an experimental platform for DeployR Open.
-   New [Data Scientist Getting Started](deployr-data-scientist-getting-started.md) guide walks data scientists through their role and resources
-   New [Application Developer Getting Started](deployr-application-developer-getting-started.md) guide walks application developers through their role and resources
-   New [Administrator Getting Started](deployr-administrator-getting-started.md) guide walks DeployR admins through their role and resources
-   New [Writing Portable R Code](deployr-data-scientist-write-portable-r-code.md) guide was added for data scientists.
-   New R package: `deployrUtils`. The goal of deployrUtils is to solve several R portability issues that arise when Data Scientists develop R analytics for use in their local R environment and in the DeployR server environment. [Install from GitHub](https://github.com/deployr/deployrUtils/releases).

### Version 7.4.0

The following list highlights the major changes and improvements to DeployR 7.4.0.

-   New [R Session Process Controls](deployr-admin-security.md#r-session-process-controls) introduce fine-grain file system access controls for R sessions on Linux platforms.
-   New [DeployR Command Line Tool (CLI)](https://github.com/deployr/deployr-cli) with initial support for running DeployR-enabled example applications.
-   New and updated Web service API support as detailed in the [API Reference Guide change history](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/74changehistory.html).
-   Updated [Java, JavaScript and .NET RBroker Framework](https://github.com/deployr?query=rbroker) with additional features, patches and updated change history.
-   Updated [Java, JavaScript and .NET client libraries](https://github.com/deployr?query=client) with additional features, patches and updated change history.
-   Updated user account password security with automatic [bcrypt hashing](https://en.wikipedia.org/wiki/Bcrypt), which is resistent to rainbow table and brute-force search attacks.

### Version 7.3

The following list highlights the major changes and improvements to DeployR 7.3.

-   Support for [DeployR Enterprise and DeployR Open](https://deployr.revolutionanalytics.com/download).
-   Support for this new DeployR website providing documentation and downloads.
-   New [RBroker Framework](deployr-tools-and-samples.md) providing a simple yet powerful API that supports the rapid integration of on-demand R analytics inside any JavaScript, Java, or .NET application.
-   New and updated API support as detailed in the [API Reference Guide change history](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/73changehistory.html).
-   New user account password and user account locking policies as detailed in the [Security Guide](deployr-admin-security.md).
-   New Administrator Diagnostic Tools that the `admin` can use to diagnose the DeployR environment to help identify unresponsive components, including the server itself, grid nodes and the database.
-   New [Server Log Events](deployr-common-administration-tasks.md#inspecting-server-logs) are now captured in the DeployR server log files. These log events provide a permanent record of API call/response events, authentication events, HTTP session events, and grid R session events.
