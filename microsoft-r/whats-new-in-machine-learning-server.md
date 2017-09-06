---

# required metadata
title: "New in Machine Learning Server 9.2 | Microsoft Docs"
description: "New features, improvements, and changes in this release of Machine Learning Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/05/2017"
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

# What's new in Machine Learning Server

Machine Learning Server 9.2 is based on Microsoft R Server 9.1, now with libraries and an interpreter for Python. Language-specific development is available when you add Python or R support (or both) during setup.

## Python development

+ libraries
+ interpreter
+ interoperability
+ Use cases
  + Deploy Python models and code as web services using the convenient Python classes and functions in the azureml-model-management-sdk library.
  + Deploy realtime Python models as web services

Python libraries include **revoscalepy**, **microsoftml**, and **azureml-model-management-sdk**. Modules are built on Anaconda 4.2 over Python 3.5. You can run any 3.5-compatible library on tPython interpreter included in Machine Learning Server.

## R development

+ libraries
+ interpreter (core engine))
+ interoperability
+ Use cases
  + R realtime model scoring is now also supported on Linux

R packages include ... Function libraries are built on Microsoft R Open (MRO), Microsoft's distribution of open source R 3.4.1.

## Configuration

+ Role-based access control (RBAC) has been extended with a new explicit Reader role.
 
+ Register your compute nodes with your web nodes in a centralized and simplified way in the Administration Utility.

## Notes

For a What's New for Microsoft R Client, see [here](what-is-microsoft-r-client.md#r-client-whats-new).

## Previous versions

See [Feature announcements in R Server](whats-new-in-r-server.md), version 9.1 and earlier.

## See Also

 [Welcome to Machine Learning Server](what-is-machine-learning-server.md) 
 [Install Machine Learning Server on Windows](install/r-server-install-windows.md)  
 [Install Machine Learning Server on Linux](install/r-server-install-linux-server.md)  
 [Install Machine Learning Server on Hadoop](install/r-server-install-hadoop.md)
 [Configure Machine Learning Server to operationalize your analytics](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) 