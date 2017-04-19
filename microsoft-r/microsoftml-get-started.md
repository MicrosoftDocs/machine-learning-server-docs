---

# required metadata
title: "Get started with MicrosoftML"
keywords: ""
author: "bradsev"
manager: "jhubbard"
ms.date: "04/19/2017"
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

- For an introduction to MicrosoftML, see [Introduction to MicrosoftML](microsoftml-introduction.md).
- For an overview of the data transforms and machine learning algorithms provided by the MicrosoftML package, see [Overview of MicrosoftML functions](overview-microsoftml-functions.md).
- For documentation on the individual transforms and functions in the product help, see [MicrosoftML: State-of-the-Art Machine Learning Algorithms from Microsoft Corporation](microsoftml/microsoftml.md).
- For guidance when choosing the appropriate machine learning algorithm from the MicrosoftML package, see the [MicrosoftML algorithm cheat sheet](microsoftml-algorithm-cheat-sheet.md).
- To get started with the pretrained models, see [How to install and deploy pretrained models with MicrosoftML](deploy-pretrained-microsoftml-models.md).
- For MicrosoftML samples and quickstarts, see [Quickstarts for MicrosoftML](microsoftml-quickstarts.md).


<a name="platform-availability"></a>
## Platform availability
The **MicrosoftML** package is currently available on the following platforms:

- [Microsoft R Server for Windows](rserver-install-windows.md): install Microsoft R Server 9.1.0 on a standalone Windows server.
- [Microsoft R Client on Windows](r-client-install-windows.md): a free, data science tool built on top of Microsoft R Open for high performance analytics on a Windows machine.
- [Microsoft R Client on Linux](r-client-install-linux.md): a free, data science tool built on top of Microsoft R Open for high performance analytics on a Linux machine.
- [Microsoft R Server for Linux](rserver-install-linux-server.md)
- [Microsoft R Server 9.1.0 on Hadoop](rserver-install-hadoop.md): R Server requires MapReduce, Hadoop Distributed File System (HDFS), and Apache YARN. Optionally, Spark version 1.6-2.0 is supported for Microsoft R Server 9.x.
- [Microsoft R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](rserver-install-cloudera.md): uses Cloudera's parcel installation methodology to add R Server to a cluster.
- [SQL Server Machine Learning Services](sql-server-r-services.md): The **RevoScaleR** and **MicrosoftML** packages are included when you install **SQL Server R Services** on an instance of SQL Server 2016.
 
The pretrained ML models are not installed by default with R Server. If you want to have access to the pretrained models, you must check the **ML Models** checkbox on the **Configure the installation** page for Microsoft R Server. For details, see [How to install and deploy pretrained machine learning models with MicrosoftML](deploy-pretrained-microsoftml-models.md).

For additional information on the supported platforms, see [Supported Platforms for Microsoft R Server](rserver-install-supported-platforms.md).

## What’s new?

MicrosoftML supports new end-to-end scenarios that include new features and functionality. You can now:

-  Use **pre-trained deep neural network models** for **sentiment analysis** and **image featurization**. For instructions on how to install these models, see [How to install and deploy pre-trained machine learning models with MicrosoftML](deploy-pretrained-microsoftml-models.md).
-  Run MicrosoftML transforms and algorithms with **Apache Spark on a HDInsight cluster** for scalable and extremely high performance data management, analysis, and visualization. For installation instructions, see [Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](rserver-install-cloudera.md). For a tutorial walking you through the process, see [Get started with ScaleR on Apache Spark](scaler-spark-getting-started.md).
-  Deploy **Ensemble methods** that use a combination of learning algorithms to provide better predictive performance than the algorithms could individually. The approach is used primarily in the Hadoop/Spark environment for training across a multi-node cluster. But it can also be used in a single-node/local context.
-  Perform **real-time scoring in SQL Server** to execute R scripts from T-SQL without having to call an R interpreter. Scoring a model in this way reduces the overhead of multiple process interactions and provides much faster prediction performance in enterprise production scenarios. 
-	Create **text classification** models for problems such as sentiment analysis and support ticket classification. 
-	Train deep neural nets with **GPU acceleration** in order to solve complex problems such as retail image classification and handwriting analysis.
-	Work with **high-dimensional categorical data** for scenarios like online advertising click-through prediction.
-	Solve many other **common machine learning tasks** such as churn prediction, loan risk analysis, and demand forecasting using state-or-the-art, fast and accurate algorithms.
- **Train models 2x faster** than logistic regression with the Fast Linear Algorithm (SDCA).
- **Train multilayer custom nets** on GPUs up to 8x faster with GPU acceleration for Neural Nets.
- Reduce training time up to 10x while still retaining model accuracy using **feature selection**.


## What's next?

For additional information on R Server, see:

- [Microsoft R Getting Started Guide](microsoft-r-getting-started.md)
- [What's New in SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604847.aspx). 








