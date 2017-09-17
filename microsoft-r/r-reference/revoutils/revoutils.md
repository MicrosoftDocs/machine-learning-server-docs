---

# required metadata
title: "RevoUtils Functions R Package for Mirosoft R | Microsoft Docs"
description: "A package providing utility functions useful for script running on the Microsoft R ScaleR engine."
keywords: "RevoUtils package reference"
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "08/20/2017"
ms.topic: "reference"
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

# RevoUtils package 

The **RevoUtils** package provides utility functions that return information about your installation, such as path and memory usage.

| Package details | |
|--------|-|
| Version: |  10.0.4 |
| Runs on: | [Machine Learning Server 9.2.1](../../what-is-machine-learning-server.md) </br>[Microsoft R Client (Windows and Linux)](../../r-client/what-is-microsoft-r-client.md) <br/>[Microsoft R Server 9.1 and earlier](../../what-is-microsoft-r-server.md)   <br/>[SQL Server 2016 and later (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) |
| Built on: | R 3.3.x (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|

## How to use RevoUtils

From any R console window, you can call **RevoUtils** functions in an R session. You might want to check on memory usage if your script is crashing or performance is slow. If you get file not found errors, you can use these functions to quickly determine the location of the Home directory or packages installed with Microsoft R.

In an R session, load **RevoUtils** from the command line by typing`library(RevoUtils)`.

## Function list

|Class | Description |
|------|-------------|
|[getRevoRepos](getrevorepos.md) | Path to the package repositories. |
|[readNews](readnews.md)  | Read R or package NEWS files.|
|[Revo.home](revo-home.md)  | Path the Home directory. |
|`RevoInfo](revoinfo.md)  | Functions giving information about Microsoft R.|
|[totalSystemMemory](totalsystemmemory.md) |Uses operating system tools to return total system memory. |

## Next steps

Add R packages to your computer by running setup for R Server or R Client: 

+ [R Client](../../r-client/what-is-microsoft-r-client.md) 
+ [R Server](../../what-is-microsoft-r-server.md)

## See also

 [Package Reference](../introducing-r-server-r-package-reference.md)    