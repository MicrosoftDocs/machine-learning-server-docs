---

# required metadata
title: "Package reference (Microsoft R)"
description: "Function reference for Revo packages in Microsoft R, including RevoScaleR, RevoPemaR, and others."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "09/20/2016"
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
ms.technology:
  - r-client
  - r-server
ms.custom: ""

---

# Package Reference (Microsoft R)

This section contains the function reference documentation for proprietary revo packages installed with Microsoft R Client and R Server, or available in a service (through SQL Server or Azure). Each package offers functions used for specific categories of operations. To explore these packages at no charge, download and install the free Microsoft R Client.

|Package | Description |
|----|----|
|[RevoScaleR](scaler/scaler.md) | Data acquisition, manipulation and transformations, visualization, and analysis. RevoScaleR provides functions for the full range of statistical and analytical tasks. It's the backbone of [R Server](rserver.md) functionality. |
|[RevoPemaR](pemar-getting-started.md) | Developer functions for coding custom parallel external memory algorithms. |
|RevoIOQ and RUnit|Installation and Operational Qualification test functions, used in conjunction with the RUnit package to run a set of unit tests. It has only one user-facing function, also called **RevoIOQ**. Reference documentation is online only (`*`). |
|RevoMods|Microsoft modifications and extensions to standard R functions. Reference documentation is online only (`*`).  |
|RevoTreeView|Decision tree functions, including the **rxDTree** function. Reference documentation is online only (`*`). |
|RevoUtils|Utility functions useful when programming and developing R packages. Reference documentation is online only (`*`). |
|RevoUtilsMath|Microsoft's distribution of the Intel Math Kernal Library (MKL). Reference documentation is online only (`*`). |

`*` Learn more about this package by typing ?"packagename" in the RGUI.exe console window or in the R Help page in your preferred R IDE, such as **R Tools for Visual Studio**.

## Deprecated packages

The following packages exist for backward compatibility but are no longer under active development:

* RevoRpeConnector
* RevoRsrConnector
* revolpe
