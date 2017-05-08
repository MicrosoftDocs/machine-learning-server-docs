---

# required metadata
title: "Introduction to MicrosoftML"
description: "Learn about the capabilities of MicrosoftML."
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

# Introduction to MicrosoftML

**MicrosoftML** is a package that adds state-of-the-art machine learning algorithms and data transforms to **Microsoft R Server**. Microsoft R is a collection of servers and tools that extend the capabilities of R, making it easier and faster to build and deploy R-based solutions. Microsoft R brings you the ability to do parallel and chunked data processing that relax the restrictions on dataset size imposed by in-memory open source R. 

The **MicrosoftML** package brings new machine learning functionality with increased speed, performance and scalability, especially for handling a large corpus of text data or high-dimensional categorical data. The MicrosoftML package is installed with **Microsoft R Client**, **Microsoft R Server** and with the **SQL Server Machine Learning Services**.


## Data transforms

The **MicrosoftML package** provides a machine learning transform pipelines that allows you to compose a custom set of transforms that can be applied to your data for featurization before training or testing to facilitate these processes. These include:

- **`concat()`**: creates a single vector-valued column from multiple columns. Combining features of the same type into a vector can significantly speed up training times.
- **`categoricalHash()`**: converts a categorical value into an indicator array using hashing. Useful when the number of categories is large or highly variable
- **`categorical()`**: converts a categorical value into an indicator array using a dictionary. Useful when the number of categories is smaller or fixed.
- **`selectFeatures()`**: selects features from the specified variables using one of the two modes: count or mutual information.
- **`featurizeText()`**: produces a bag of counts of n-grams (sequences of consecutive words) from a given text. It offers language detection, tokenization, stopwords removing, text normalization, feature generation, and term weighting using TF, IDF and TF-IDF.
- **`featurizeImage()`**: featurizes an image using the specified pre-trained deep neural network model. Available only on Microsoft R 9.1.0 and later.
- **`getSentiment()`**: returns a sentiment score of the specified natural language text, without the need for any text pre-processing. A value that is closer to 0 indicates a negative sentiment while a value that is closer to 1 indicates a positive sentiment. Available only on Microsoft R 9.1 and later.

## Machine learning algorithms

The MicrosoftML package provides fast and scalable machine learning algorithms that enable you to tackle common machine learning tasks such as classification, regression and anomaly detection. These are high-performance algorithms that are multi-threaded, some of which execute off disk, so that they can scale up to to 100s of GBs on a single-node. These include:

Algorithm | ML task supported | Scalability | Application Examples
--------- | ----------------- | ------------ | -----------
**`rxFastLiner()`** <br>Fast Linear model <br>(SDCA) |  binary classification, linear regression | #cols: ~1B;<br> #rows: ~1B;<br> CPU: multi-proc | Mortgage default prediction, Email spam filtering
**`rxOneClassSvm()`** <br>OneClass SVM | anomaly detection | cols: ~1K;<br> #rows: RAM-bound;<br> CPU: single-proc | Credit card fraud detection
**`rxFastTrees()`** <br>Fast Tree | binary classification, regression | #cols: ~50K;<br> #rows: RAM-bound;<br> CPU: multi-proc | Bankruptcy prediction
**`rxFastForest()`** <br>Fast Forest | binary classification, regression | #cols: ~50K;<br> #rows: RAM-bound;<br> CPU: multi-proc | Churn Prediction
**`rxNeuralNet()`** <br>Neural Network | binary and multiclass classification, regression | #cols: ~10M;<br> #rows: Inf;<br> CPU: multi-proc CUDA GPU | Check signature recognition, OCR, Click Prediction
**`rxLogisticRegression()`** <br>Logistic regression | binary and multiclass classification |#cols: ~100M; <br>#rows: Inf for single-proc CPU<br> #rows: RAM-bound for multi-proc CPU| Classifying sentiments from feedback
- **`rxEnsemble()`**: trains a number of models of various kinds to obtain better predictive performance than could be obtained from a single model.

For more information, see [MicrosoftML functions](overview-microsoftml-functions.md) For documentation on the individual transforms and functions in the product help, see [MicrosoftML: State-of-the-Art Machine Learning Algorithms from Microsoft Corporation](microsoftml/microsoftml.md).

For guidance when choosing the appropriate machine learning algorithm from the MicrosoftML package, see the [MicrosoftML algorithm cheat sheet](microsoftml-algorithm-cheat-sheet.md).

## Getting started with MicrosoftML

MicrosoftML is currently available on various platforms in **Microsoft R Server 9.1.0 and 9.0.1** and in the **SQL Server Machine Learning Services CTP2**. 

For additional information, see [Microsoft R Getting Started Guide](https://msdn.microsoft.com/en-us/microsoft-r/microsoft-r-getting-started) and [What's New in SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604847.aspx). 

MicrosoftML functions are provided through the **MicrosoftML** package which is installed in the freely available [Microsoft R Client](r-client.md) and in the commercial product [Microsoft R Server](rserver.md).

For a list  of these platforms, see the [Platform availability](microsoftml-get-started.md#platform-availability) section in the getting started topic.


## What's next?

[Introduction to Microsoft R](microsoft-r-getting-started.md) for platform availability of Microsoft R Server, what's new with the current release, and guidance to the MicrosoftML documentation. 

[Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md) on how to use the [ScaleR functions](scaler-user-guide-introduction.md) for data acquisition, transformation and manipulation, visualization, and analysis using Microsoft R products and technologies.

[Overview of MicrosoftML functions](overview-microsoftml-functions.md) for a brief overview of the MicrosoftML functions that enable you to tackle common machine learning and data science tasks such as text and image featurization, classification, anomaly detection, regression and ranking. 

[MicrosoftML: State-of-the-art machine learning R algorithms](microsoftml/microsoftml.md) for reference documentation of the algorithms and transforms in the package.

[MicrosoftML algorithm cheat sheet](microsoftml-algorithm-cheat-sheet.md) provides guidance on how to approach the choice of an ML algorithm for your scenario.

[Quickstarts for MicrosoftML](microsoftml-quickstarts.md) shows how to use pretrained models for sentiment analysis and image featurization.

