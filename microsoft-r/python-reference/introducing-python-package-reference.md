---

# required metadata
title: "Python function library help (SQL Server Machine Learning)"
description: "Function reference for Python packages: microsoftml, revoscalepy"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "06/29/2017"
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

# Python function help

**Applies to: SQL Server 2017 RC1**

This section contains Python reference documentation for two proprietary packages installed with [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server) and [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) in the SQL Server 2017 RC1 release.

Both libraries are built on the [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc). Both SQL Server offerings include Python 3.5 when you add Python support during installation. No additional software is required to use the functions in T-SQL.

## Python libraries

|Library | Description |
|----|----|
|[revoscalepy](revoscalepy/revoscalepy-package.md) | Data access, manipulation and transformations, visualization, and analysis. The revoscalepy functions support a broad spectrum of statistical and analytical tasks. Developers who are familiar with Microsoft R Server and the RevoScaleR package will see notable similarities in the functions provided in revoscalepy. Conceptually, revoscalepy is the Python equivalent of the Microsoft R RevoScaleR package.|
|[microsoftml](microsoftml/microsoftml-package.md)|A collection of Python functions used for machine learning use cases. |

## Python naming conventions

Both **revoscalepy** and **microsoftml** correspond to R language libraries in the form of [RevoScaleR](../r-reference/revoscaler/revoscaler-package.md) and [MicrosoftML](../r-reference/microsoftml/microsoftml-package.md). If you have a background in R language or in these libraries in particular, you might notice similarities in function names and operations, with Python versions adhering to the naming conventions of that language:

* lowercase package names (**microsoftml** contrasted with **MicrosoftML**) and most function names
* underscore in function names (rx_import in **revoscalepy** contrasted with rxImport in **RevoScaleR**)

At this time, full parity does not yet exist between language-specific versions of each package, but you can expect gaps to close over time. For now, we recommend using the documentation specific to each language.

## See also

  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)