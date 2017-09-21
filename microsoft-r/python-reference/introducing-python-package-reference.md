---

# required metadata
title: "Python function library reference - Machine Learning Server | Microsoft Docs"
description: "Function reference for Python packages: microsoftml, revoscalepy, azureml-model-mananagement-sdk in Machine Learning Server."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "08/29/2017"
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

This section contains the Python reference documentation for three proprietary packages from Microsoft used for data science and machine learning on premises and at scale.  

You can use these libraries and functions in combination with other open source or third-party packages, but to use the proprietary packages, your Python code must run against a service or on a computer that provides the interpreters.

| Library details | |
|--------|-|
| [Supported platforms](../install/r-server-install-supported-platforms.md) | [Machine Learning Server 9.2.1](../what-is-machine-learning-server.md) </br><br/>[SQL Server 2017  (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm)|
| Built on: | [Anaconda 4.2.0](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc) (included when you [add Python support](#how-to-install) during installation). |

## Python modules

|Module | Version | Description |
|--------|---------|-------------|
|[azureml-model-management-sdk](azureml-model-management-sdk/azureml-model-management-sdk.md) | 1.0.0 | Classes and functions to authenticate, deploy, manage, and consume analytic web services in Python.  |
|[microsoftml](microsoftml/microsoftml-package.md)| 1.5.0 | A collection of Python functions used for machine learning operations, including training and transformations, scoring, text and image analysis, and feature extraction for deriving values from existing data. |
|[revoscalepy](revoscalepy/revoscalepy-package.md) | 9.2.1 | Data access, manipulation and transformations, visualization, and statistical analysis. The revoscalepy functions support a broad spectrum of statistical and analytical tasks that operate at scale, bringing analytical operations to local data or remote on a Spark cluster or data residing in SQL Server. |

> [!Note]
> Developers who are familiar with Microsoft R packages might notice similarities in the functions provided in revoscalepy and microsoftml. Conceptually, revoscalepy and microsftml are the Python equivalents of the RevoScaleR R package and the MicrosoftML R package, respectively.

<a name="how-to-install"></a>

## How to get packages

You can get the packages when you run Machine Learning Server, or SQL Server 2017 Setup, and choose features that include Machine Learning with Python support. In addition to the packages, Machine Learning Server setup and SQL Server setup both install the Python interpreters and base modules required to run any script or code that calls functions from properietary package.

For SQL Server, packages are installed by default in the \Program files\Microsoft SQL Server\*instance name*\PYTHON_SERVICES

Ships in:
+ [Machine Learning Server](../what-is-machine-learning-server.md) 
+ [SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) 
+ [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server)

## How to list modules and versions

To get the version of a Python module installed on your computer, start Python and execute the following commands:

1. Double-click **Python.exe** in \Program Files\Microsoft\ML Server\PYTHON_SERVER.
2. Open interactive help: `help()`
3. Type the name of a module at the help prompt: help> `revoscalepy`. Help returns the name, package contents, version, and file location.
4. List all installed modules: `modules`
5. Import a module: `import revoscalepy`

## Note to R Users: Python naming conventions

**revoscalepy**, **microsoftml**, and **azureml-model-management-sdk** correspond to the Microsoft R packages, [RevoScaleR](../r-reference/revoscaler/revoscaler.md), [MicrosoftML](../r-reference/microsoftml/microsoftml-package.md), and [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md). If you have a background in these libraries, you might notice similarities in function names and operations, with Python versions adhering to the naming conventions of that language:

* lowercase package names (**microsoftml** contrasted with **MicrosoftML**) and most function names
* underscore in function names (rx_import in **revoscalepy** contrasted with rxImport in **RevoScaleR**)

## Next steps

First, read the introduction to each package to learn about common use case scenarios:

+ [azureml-model-management-sdk package for Python](azureml-model-management-sdk/azureml-model-management-sdk.md)  
+ [microsoftmlpackage for Python](microsoftml/microsoftml-package.md) 
+ [revoscalepy package for Python](revoscalepy/revoscalepy-package.md)  

Next, follow these tutorials for hands on experience:

+ [Use revoscalpy to create a model (in SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model) 
+ [Run Python in T-SQL](https://docs.microsoft.com/sql/advanced-analytics/tutorials/run-python-using-t-sql) 
+ [Deploy a model as a web service in Python](../operationalize/python/quickstart-deploy-python-web-service.md)

## See also

  [How-to guides in Machine Learning Server](../r/how-to-introduction.md)     
  [Machine Learning Server](../what-is-machine-learning-server.md)   
  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)    
  [Additional learning resources and sample datasets](../resources-more.md)  