---

# required metadata
title: "What is Microsoft R Server and R Client"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "06/22/2017"
ms.topic: "get-started-article"
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

# Microsoft R

Microsoft R is a collection of packages, interpreters, and infrastructure for developing and deploying R-based machine learning and data science solutions on a range of platforms, from local standalone installations on Linux and Windows, to large distributed deployments on node clusters. We make these capabilities available in several products and services to meet the needs of individual or teams of data scientists and developers.

+ [Microsoft R Server](rserver.md) is the flagship product and supports very large workloads in the enterprise. 

+ [Microsoft R Client](r-client.md) is a free workstation version. It includes the same R Server functionality, but for local workloads.

+ [Microsoft R Open](https://mran.microsoft.com/open/) is Microsoft's distribution of open source R, without the proprietary packages and infrastructure of our other products. This R distribution is included in both Microsoft R Client and R Server. 

## Quickstarts and Step-by-Step

In minutes, you can step through a classic what-if problem using sample data and R script in the first quickstart. Additional step-by-step tutorials offer a hands-on experience performing practical data science tasks.

* [Quickstart: Run R code in Microsoft R](quickstart-r-code.md) 

* [Quickstart: Deploy R code as a web service](operationalize/quickstart-publish-web-service.md) 

* [Tutorial: Explore R-to-RevoScaleR](microsoft-r-tutorial-R2RevoScaleR.md) 

* [Tutorial: Import and transform data](scaler-getting-started-data-import-exploration.md)  

* [Tutorial: Visualize and analyze data](scaler-getting-started-data-visualization-analysis.md) 

## Pre-built solutions and 

We offer solutions tailored to specific industries and problem sets. Find more solutions on the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/browse?s=R%20Server&skip=0&categories=%5B%2210%22%5D&tags=%5B%22Microsoft%20R%20Server%22%5D).

* [Solution: Predicting length of stay in hospitals](https://gallery.cortanaintelligence.com/Solution/Predicting-Length-of-Stay-in-Hospitals-1) 

## Embedded R Server

Additionally, you can use embedded Microsoft R packages, interpreters, and libraries in Azure data science virtual machines, in Azure HDInsight (Microsoft's distribution of Hadoop), and as an in-database service in SQL Server.

* [Azure: HDInsight and R Server](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) 

* [Azure: Data Science virtual machine](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview)

* [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services)

## Start with a local installation

If you are new to Microsoft R, we recommend starting with either [R Server for Windows](rserver-install-windows.md), [R Server for Linux](rserver-install-linux-server.md), or the free [R Client](r-client-install.md), and an integrated development environment like [R Tools for Visual Studio (RTVS)](https://docs.microsoft.com/visualstudio/rtvs/installation). 

You can install these configurations for free. Each one gives you Microsoft R Open with full support of all base R functions so that you can write R-only solutions, but also includes Microsoft R proprietary packages that run locally on your development computer.

Since the R engine lies underneath, you can use the R Core Team manuals that are part of every R distribution to learn how to code in R. Built-in manuals include *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and more. Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

However, because you have R Client or R Server, your script can also include functions shipped only with Microsoft R products, including the [MicrosoftML](microsoftml/microsoftml.md), [olapR](olapr/olapr.md), [mrsdeploy](mrsdeploy/mrsdeploy.md), and [RevoScaleR](scaler/scaler.md) packages. All of these packages are available in both R Client and R Server, but at different levels of capacity.

## Next steps

Learn more about Microsoft R in these articles:

+ [Operationalizing R code with R Server](operationalize/about.md)

+ [What's new in R Server](rserver-whats-new.md)

+ [What's new in R Client](rserver-whats-new.md)

+ [Microsoft R Product Web Page](https://www.microsoft.com/en-us/cloud-platform/r-server)

+ [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md)

+ [Microsoft R Client Get Started](r-client-get-started.md)

+ [Microsoft R Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)

+ [SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx)

+ [Additional Resources](microsoft-r-more-resources.md)

