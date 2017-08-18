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

This section contains Python reference documentation for two proprietary packages used for creating and training machine learning models, scoring data, and preparing data. Currently, the packages are integrated only with SQL Server 2017. There are two use cases for this release: 

+ Calling Python functions in T-SQL script or stored procedures running on SQL Server.  
+ Calling **revoscalepy** functions in Python script executing in a SQL Server [compute context](../r/concept-what-is-compute-context.md). 

Supported platforms: SQL Server 2017 (Windows only).

Built on: [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc), included when you add Python support during installation. 

## Python libraries

|Package | Version | Description |
|--------|---------|-------------|
|[revoscalepy](revoscalepy/revoscalepy-package.md) | 9.2.0 | Data access, manipulation and transformations, visualization, and analysis. The revoscalepy functions support a broad spectrum of statistical and analytical tasks. Developers who are familiar with Microsoft R Server and the RevoScaleR package will see notable similarities in the functions provided in revoscalepy. Conceptually, revoscalepy is the Python equivalent of the Microsoft R RevoScaleR package.|
|[microsoftml](microsoftml/microsoftml-package.md)| 1.4.0 | A collection of Python functions used for machine learning use cases. |

## How to get packages

You can get the packages described in this section when you run SQL Server 2017 Setup and choose features that include Machine Learning with Python support. In addition to the packages, SQL Server Setup installs the interpreters and libraries required to run any script or code that calls functions from either package.

By default, packages are installed in the C:\Program Files\Microsoft SQL Server\140 folder.

Ships in:
+  [SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) 
+ [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server).

## How to list packages and versions

To get the version of a Python package installed on your computer, start Python from the command line or open a Python IDE and execute the following commands:

1. Start interactive help: >>>`help()`
2. Get a list of all installed modules: help> `modules`
3. import the module: `import revoscalepy`
4. Get the version: `revoscalepy._version_`

## Note to R Users: Python naming conventions

Both **revoscalepy** and **microsoftml** correspond to the Microsoft R packages, [RevoScaleR](../r-reference/revoscaler/revoscaler.md) and [MicrosoftML](../r-reference/microsoftml/microsoftml-package.md). If you have a background in these libraries, you might notice similarities in function names and operations, with Python versions adhering to the naming conventions of that language:

* lowercase package names (**microsoftml** contrasted with **MicrosoftML**) and most function names
* underscore in function names (rx_import in **revoscalepy** contrasted with rxImport in **RevoScaleR**)

## Next steps

Get started with these Python libraries in SQL Server 2017: [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Move forward with these tutorials to understand the advantages of working with Python in SQL Server:

+ [Use revoscalpy to create a model](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model) 
+ [Run Python in T-SQL](https://docs.microsoft.com/sql/advanced-analytics/tutorials/run-python-using-t-sql) 

## See also

  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)