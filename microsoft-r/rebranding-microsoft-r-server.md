---

# required metadata
title: "Microsoft R Server is renamed - Machine Learning Server "
description: "Microsoft R Server was rebranded as Machine Learning Server in Sept. 2017."
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: "07/15/2019"
ms.topic: "overview"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Microsoft R Server is now Microsoft Machine Learning Server

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

In September 2017, Microsoft R Server was released under the name of **Microsoft Machine Learning Server**. The product was renamed from R Server to Machine Learning Server to reflect the addition of Python-based analytics.

In subsequent releases, including the current version of 9.4, the R components that originated in R Server continue to be  distributed under the Machine Learning Server product name. Specifically, the R components in Machine Learning Server include packages such as RevoScaleR, olapR, and mrsdeploy to name a few.

As related to R components, some highlights from previous versions include improvements to web service deployment in version 9.3, and improvements to Apache Spark deployment in version 9.4. Read the [What's new in this release](whats-new-in-machine-learning-server.md) to learn more.

## Building on our commitment to R

Microsoft continues its commitment and development in R, not only in the latest Machine Learning Server release, but also in the newest [Microsoft R Client](r-client/what-is-microsoft-r-client.md) and [Microsoft R Open](https://mran.microsoft.com) releases. You can also find R and Python support in [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/sql-server-machine-learning-services) on Windows and Linux, and R support in [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-machine-learning-services-overview).

## Upgrade to Machine Learning Server

R components are backwards compatible. You should be able to run existing R script on newer versions, with the exception of dependencies on packages or platforms that are no longer supported, or [known issues](resources-known-issues.md) that require a workaround or code change.

Moving from R Server to Machine Learning Server is an in-place upgrade unless you configured the server for operationalization (using mrsdeploy or deployR, depending on the product version you are using). 

1. Follow these instructions to [install Machine Learning Server](install/machine-learning-server-install.md) on Windows, Linux, or Hadoop. Microsoft R Server 9.1 for Teradata was the last release for that platform and does not have a Machine Learning Server equivalent. 

1. Perform additional steps to [upgrade your web and compute nodes](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) if the server is operationalized.

## Download prior R Server releases

You can no longer download older versions of Microsoft R Server. All Microsoft distributions are now some version of Machine Learning Server.

## Get support

+ [Review our servicing support timeline](resources-servicing-support.md)

+ [Post in our user forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftR)  

## About Microsoft R Client

[Microsoft R Client](r-client/what-is-microsoft-r-client.md) continues on as an R-only client for Machine Learning Server and R Server. 

A Python interpreter can also be installed locally along with the custom Python packages to gain similar functionality on a Windows machine. [Learn more](install/python-libraries-interpreter.md).
