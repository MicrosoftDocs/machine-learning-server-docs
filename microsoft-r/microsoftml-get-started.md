---

# required metadata
title: "Get started with MicrosoftML"
keywords: ""
author: "bradsev"
manager: "jhubbard"
ms.date: "05/05/2017"
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

**MicrosoftML** is a new package for **Microsoft R Server** and **Microsoft R Client** that adds state-of-the-art machine learning algorithms and data transforms to Microsoft R Server functionality. It enables you to run these functions locally on Windows or Linux machines or on Azure HDInsight (Hadoop/Spark) clusters. Pretrained models for sentiment analysis and image featurization can also be installed and deployed with the  MicrosoftML package.

<a name="platform-availability"></a>

## Platform availability

The **MicrosoftML** package is currently shipped as part of Microsoft R Server, SQL Server Machine Learning Services, and Microsoft R Client installers. To also install pretrained machine learning models, you must opt-in during setup. For details, see [How to install and deploy pretrained machine learning models with MicrosoftML](deploy-pretrained-microsoftml-models.md).

For additional information on the supported platforms, see [Supported Platforms for Microsoft R Server](rserver-install-supported-platforms.md).

## What’s new?

MicrosoftML supports new end-to-end scenarios that include new features and functionality. For the latest features, see [What's New in R Server](rserver-whats-new.md)

You can now:
-  Use **pre-trained deep neural network models** for **sentiment analysis** and **image featurization**. For instructions on how to install these models, see [How to install and deploy pre-trained machine learning models with MicrosoftML](deploy-pretrained-microsoftml-models.md). For quickstarts that show how to use pretrained models for sentiment analysis and image featurization, see [Quickstarts for MicrosoftML](microsoftml-quickstarts.md).
-  Run MicrosoftML transforms and algorithms with **Apache Spark on a HDInsight cluster** for scalable and extremely high performance data management, analysis, and visualization. For installation instructions, see [Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](rserver-install-cloudera.md). For a tutorial walking you through the process, see [Practice data import and exploration on Apache Spark](scaler-spark-getting-started.md).
-  Deploy **Ensemble methods** that use a combination of learning algorithms to provide better predictive performance than the algorithms could individually. The approach is used primarily in the Hadoop/Spark environment for training across a multi-node cluster. But it can also be used in a single-node/local context.
-  Perform **real-time scoring in SQL Server** to execute R scripts from T-SQL without having to call an R interpreter. Scoring a model in this way reduces the overhead of multiple process interactions and provides much faster prediction performance in enterprise production scenarios. 
-	Create **text classification** models for problems such as sentiment analysis and support ticket classification. 
-	Train deep neural nets with **GPU acceleration** in order to solve complex problems such as retail image classification and handwriting analysis.
-	Work with **high-dimensional categorical data** for scenarios like online advertising click-through prediction.
-	Solve many other **common machine learning tasks** such as churn prediction, loan risk analysis, and demand forecasting using state-or-the-art, fast and accurate algorithms.
- **Train models 2x faster** than logistic regression with the Fast Linear Algorithm (SDCA).
- **Train multilayer custom nets** on GPUs up to 8x faster with GPU acceleration for Neural Nets.
- Reduce training time up to 10x while still retaining model accuracy using **feature selection**.


## See also

 [Microsoft R Server Overview](rserver.md)    
 [Introduction to MicrosoftML](microsoftml-introduction.md)    
 [Overview of MicrosoftML algorithms](overview-microsoftml-functions.md)    
 [MicrosoftML function help pages](microsoftml/microsoftml.md)    
 [Choose a MicrosoftML algorithm (cheatsheet)](microsoftml-algorithm-cheat-sheet.md)    
 [Install and deploy pretrained models](deploy-pretrained-microsoftml-models.md)    
 [MicrosoftML samples](microsoftml-quickstarts.md)    