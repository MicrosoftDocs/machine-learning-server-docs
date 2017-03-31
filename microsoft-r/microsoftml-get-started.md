---

# required metadata
title: "Get started with MicrosoftML"
keywords: ""
author: "bradsev"
manager: "jhubbard"
ms.date: "03/30/2017"
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
ms.technology: "r-server"
ms.custom: ""

---

# Get started with MicrosoftML

**MicrosoftML** is a new package for [Microsoft R Server](microsoft-r-getting-started.md) and [Microsoft R Client](r-client.md) that adds state-of-the-art algorithms and data transforms to Microsoft R Server functionality. It enables you to run these functions locally or on HDInsight (Hadoop) clusters. Pretrained models for sentiment analysis and image featurization can also be installed and deployed with the  MicrosoftML package.

- For an introduction to MicrosoftML, see [Introduction to MicrosoftML](microsoftml-introduction.md).
- For an overview of the data transforms and machine learning algorithms provided by the MicrosoftML package, see [Overview of MicrosoftML functions](overview-microsoftml-functions.md).
- For documentation on the individual transforms and functions in the product help, see [MicrosoftML: State-of-the-Art Machine Learning Algorithms from Microsoft Corporation](microsoftml/microsoftml.md).
- For guidance when choosing the appropriate machine learning algorithm from the MicrosoftML package, see the [MicrosoftML algorithm cheat sheet](microsoftml-algorithm-cheat-sheet.md).
- For an introduction to the pretrained models, see [How to deploy pretrained models with MicrosoftML](deploy-pretrained-microsoftml-models.md).
- For MicrosoftML samples and quickstarts, see [Quickstarts for MicrosoftML](microsoftml-quickstarts.md).


## Platform availability
The **MicrosoftML** package is currently available on the following platforms:

- [Microsoft R Server for Windows](rserver-install-windows.md)
- [Microsoft R Client](r-client-install.md)
- [Microsoft R Server for Linux](rserver-install-linux-server.md)
- [Microsoft R Server 9.0.1 on Hadoop](rserver-install-hadoop.md)
- [SQL Server R Services](sql-server-r-services.md)
 
If you want to have access to the pretrained models, you must check the **ML Model** box when configuring options for the install.

For additional information on the supported platforms, see [Supported Platforms for Microsoft R Server](rserver-install-supported-platforms.md).

## What’s new?

MicrosoftML supports new end-to-end scenarios that include new features and functionality. You can now:
-	Create text classification models for problems such as sentiment analysis and support ticket classification.
-	Train deep neural nets with GPU acceleration in order to solve complex problems such as retail image classification and handwriting analysis.
-	Work with high-dimensional categorical data for scenarios like online advertising click-through prediction.
-	Solve many other common machine learning tasks such as churn prediction, loan risk analysis, and demand forecasting using state-or-the-art, fast and accurate algorithms.
- Train models 2x faster than logistic regression with the Fast Linear Algorithm (SDCA).
- Train multilayer custom nets on GPUs up to 8x faster with GPU acceleration for Neural Nets.
- Reduce training time up to 10x while still retaining model accuracy using feature selection.


## What's next?

For additional information on R Server, see [Microsoft R Getting Started Guide](microsoft-r-getting-started.md) and [What's New in SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604847.aspx). 








