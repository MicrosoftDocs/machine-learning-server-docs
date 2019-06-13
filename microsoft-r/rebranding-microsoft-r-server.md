---

# required metadata
title: "What happened to Microsoft R Server - Machine Learning Server "
description: "Microsoft R Server was rebranded as Machine Learning Server in Sept. 2017."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
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

In September 2017, Microsoft R Server was released under the new name of **Microsoft Machine Learning Server**. The product was renamed from R Server to Machine Learning Server to reflect the addition of Python-based analytics.

In subsequent releases, including the current version of 9.4, the R components that originated in R Server continue to be  distributed under this product name. Specifically, the R components in Machine Learning Server include packages such as RevoScaleR, olapR, and mrsdeploy to name a few.

As related to R components, some highlights from previous versions include improvements to web service deployment in version 9.3, and improvements to Apache Spark deployment in verison 9.4. Read the [What's new in this release](whats-new-in-machine-learning-server.md) to learn more.

## Building on our commitment to R

Microsoft continues its commitment and development in R, not only in the latest Machine Learning Server release, but also in the newest [Microsoft R Client](r-client/what-is-microsoft-r-client.md) and [Microsoft R Open](https://mran.microsoft.com) releases.

## Upgrade to Machine Learning Server

R components are backwards compatible. You should be able to run existing R script on newer versions, with the exception of dependencies on packages or platforms that are no longer supported, or [known issues](resources-known-issues.md) that require a workaround or code change.

Moving from R Server to Machine Learning Server is an in-place upgrade unless you configured the server for operationalization (using mrsdeploy or deployR, depending on the product version you are using). 

1. Follow these instructions to [install Machine Learning Server](install/machine-learning-server-install.md) on Windows, Linux, or Hadoop. Microsoft R Server 9.1 for Teradata was the last release for that platform and does not have a Machine Learning Server equivalent. 

1. Perform additional steps to [upgrade your web and compute nodes](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) if the server is operationalized.

## Download prior R Server releases

The following table provides links for downloading older versions of Microsoft R Server. [Find the instructions for installing R Server here](install/r-server-install.md). 

| Site for R Server | Edition | Details |
|------|---------|---------|
| [Visual Studio Dev Essentials](https://go.microsoft.com/fwlink/?LinkId=717968&clcid=0x409) | Developer (free) | This option provides a zipped file, free when you sign up for Visual Studio Dev Essentials. Developer edition has the same features as Enterprise, except it is licensed for development scenarios. |
|[Volume Licensing Service Center (VLSC)](https://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | Enterprise | Sign in, search for R Server. Choose the right version for your OS. |

From [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/):

1. Click **Join or access now** to sign up for download benefits.
2. Check the URL to verify it changed to *https://my.visualstudio.com/*.
3. Click **Downloads** to search for R Server.
4. Click **Downloads** for a specific version to select the platform.

![Download page on Visual Studio benefits page](./install/media/mlserver-install-older-versions.png)

## Get support

+ [Review our servicing support timeline](resources-servicing-support.md)

+ [Post in our user forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftR)  

## The future of Microsoft R Client

[Microsoft R Client](r-client/what-is-microsoft-r-client.md) continues on as an R-only client for Machine Learning Server and R Server. 

A Python interpreter can also be installed locally along with the custom Python packages to gain similar functionality on a Windows machine. [Learn more](install/python-libraries-interpreter.md).
