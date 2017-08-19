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

This section contains the Python reference documentation for two proprietary packages from Microsoft used for data science at scale and machine learning, respectively.  

**Supported platforms:** SQL Server 2017 (Windows only). Additional platforms and compute contexts are planned for future releases.

**Built on:** [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc) (included when you add Python support during installation). 

## Python libraries

|Modules | Version | Description |
|--------|---------|-------------|
|[revoscalepy](revoscalepy/revoscalepy-package.md) | 9.2.0 | Data access, manipulation and transformations, visualization, and statistical analysis. The revoscalepy functions support a broad spectrum of statistical and analytical tasks that operate at scale, bringing analytical operations to your data residing in SQL Server. |
|[microsoftml](microsoftml/microsoftml-package.md)| 1.4.0 | A collection of Python functions used for machine learning operations, including training and transformations, scoring, text and image analysis, and feature extraction for deriving values from existing data. |

You can use the modules together or individually. You can also use any 35-compatible Python module and call its functions from Python code you run on SQL Server.

> [!Note]
> Developers who are familiar with Microsoft R packages might notice similarities in the functions provided in revoscaley and microsoftmo. Conceptually, revoscalepy and microsftml are the Python equivalents of the RevoScaleR R package and the MicrosoftML R package, respectively.

## How to get packages

You can get the packages when you run SQL Server 2017 Setup and choose features that include Machine Learning with Python support. In addition to the packages, SQL Server Setup installs the Python interpreters and base modules required to run any script or code that calls functions from either package.

By default, packages are installed in the C:\Program Files\Microsoft SQL Server\140 folder.

Ships in:
+  [SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) 
+ [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server).

## How to list modules and versions

To get the version of a Python module installed on your computer, start Python from the command line or open a Python IDE and execute the following commands:

1. Open interactive help: `help()`
2. List installed modules: `modules`
3. Import the module for which you want version information: `import revoscalepy`
4. Get the version: `revoscalepy._version_`

## Note to R Users: Python naming conventions

Both **revoscalepy** and **microsoftml** correspond to the Microsoft R packages, [RevoScaleR](../r-reference/revoscaler/revoscaler.md) and [MicrosoftML](../r-reference/microsoftml/microsoftml-package.md). If you have a background in these libraries, you might notice similarities in function names and operations, with Python versions adhering to the naming conventions of that language:

* lowercase package names (**microsoftml** contrasted with **MicrosoftML**) and most function names
* underscore in function names (rx_import in **revoscalepy** contrasted with rxImport in **RevoScaleR**)

## Next steps

First, read the introduction to each package to learn about common use case scenarios:

+ [revoscalepy package for Python](revoscalepy/revoscalepy-package.md)  
+ [microsoftmlpackage for Python](microsoftml/microsoftml-package.md) 

Next, add these Python modules to your SQL Server 2017 instance: 

+ [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Lastly, follow these tutorials for hands on experience:

+ [Use revoscalpy to create a model](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model) 
+ [Run Python in T-SQL](https://docs.microsoft.com/sql/advanced-analytics/tutorials/run-python-using-t-sql) 

## See also

  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)