---

# required metadata
title: "Python function library reference (SQL Server Machine Learning) | Microsoft Docs"
description: "Function reference for Python packages: microsoftml, revoscalepy"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "08/17/2017"
ms.author: heidist
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
ms.custom: ""

---

# Python Function Library Reference

This section contains Python reference documentation for two proprietary packages used for data transformation and manipuation, for machine learning workloads that you run in SQL Server 2017.

Built on: [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc), included when you add Python support during installation. 

Supported platforms: Windows only.

## Python libraries

|Package | Version | Description |
|--------|---------|-------------|
|[revoscalepy](revoscalepy/revoscalepy-package.md) | | Data access, manipulation and transformations, visualization, and analysis. The revoscalepy functions support a broad spectrum of statistical and analytical tasks. Developers who are familiar with Microsoft R Server and the RevoScaleR package will see notable similarities in the functions provided in revoscalepy. Conceptually, revoscalepy is the Python equivalent of the Microsoft R RevoScaleR package.|
|[microsoftml](microsoftml/microsoftml-package.md)| | A collection of Python functions used for machine learning use cases. |

## How to get packages

The packages documented in this section are found only on installations of the Microsoft products or Azure services that provide them. Setup programs or scripts install the propertietary Python packages from Microsoft and any package dependencies. Unless otherwise noted, all of the packages listed in the preceding table are installed with the product or service.

By default, packages are installed in the C:\Program Files\Microsoft SQL Server\140 folder on Windows.

Ships in:
+  [SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) 
+ [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server).

## How to list packages and versions

To get the version of an R package installed on your ocmputer, open an R console application and execute the following command: `installed.packages()`

## Python naming conventions

Both **revoscalepy** and **microsoftml** correspond to the Microsoft R packages, [RevoScaleR](../r-reference/revoscaler/revoscaler.md) and [MicrosoftML](../r-reference/microsoftml/microsoftml-package.md). If you have a background in these libraries, you might notice similarities in function names and operations, with Python versions adhering to the naming conventions of that language:

* lowercase package names (**microsoftml** contrasted with **MicrosoftML**) and most function names
* underscore in function names (rx_import in **revoscalepy** contrasted with rxImport in **RevoScaleR**)

At this time, full parity does not yet exist between language-specific versions of each package, but you can expect gaps to close over time. For now, we recommend using the documentation specific to each language.

No additional software is required to call Python functions in T-SQL script or stored procedures, or call **revoscalepy** functions in Python script executing in a SQL Server compute context.

## Next steps

Get started with these Python libraries in SQL Server 2017: [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

## See also

  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)