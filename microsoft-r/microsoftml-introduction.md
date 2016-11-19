---

# required metadata
title: "Introduction to MicrosoftML"
description: "Learn about the capabilities of MicrosoftML."
keywords: ""
author: "bradsev"
manager: "jhubbard"
ms.date: "11/18/2016"
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

MicrosoftML is a new Machine Learning package for R. It contains a collection of functions in Microsoft R used for doing machine learning at scale. Microsoft R is a collection of servers and tools that extend the capabilities of R, making it easier and faster to build and deploy R-based solutions, including big data scenarios. Microsoft R Server brings you the ability to do parallel and chunked data processing that addresses the restrictions that limit in-memory open source R. MicrosoftML adds learners and transforms that are used by product teams across Microsoft to the existing Microsoft R Server functionality. This brings new machine learning functionality with increased speed, performance and scale, especially for handling a large corpus of text data and high-dimensional categorical data.  

## What’s New?

- Fast Linear Learner (SDCA) allows you to train 2x faster than logistic regression. [Notebook Link](http://notebookhost.redmond.corp.microsoft.com/notebooks/Tutorials%20and%20Samples/3.%20Samples/FastLinear_Twitter.ipynb)
- GPU acceleration for Neural Nets allows you to train multilayer custom nets on GPUs up to 8x faster. [Notebook Link](http://notebookhost.redmond.corp.microsoft.com/notebooks/Tutorials%20and%20Samples/2.%20Demos/MNIST_GPU.ipynb).
- Feature selection can reduce training time up to 10x while still retaining model accuracy. [Notebook Link](http://notebookhost.redmond.corp.microsoft.com/notebooks/Tutorials%20and%20Samples/2.%20Demos/FeatureSelection_Twitter.ipynb)
- SQL Server integration allows you to easily pull data from SQL and train in R and use your trained model inside SQL. [More Info]()

## MicrosoftML transforms and learners

The `MicrosoftML` package provides transform pipelines that allow you to compose a custom set of transforms that can be applied to your data for featurization before training or testing to facilitate these processes. These include:

- Concatenate
- Categorical Hash
- Categorical 
- Select Features
- Ngram
- Featurize Text

The `MicrosoftML` package provides fast and scalable machine learning algorithms that enable you to tackle common machine learning tasks such as classification, regression and ranking. These include:

Algorithm | ML task supported | Applications
--------- | ----------------- | ------------
Fast Linear model (SDCA) |  binary classification, linear regression | Mortgage default prediction, Email spam filtering
OneClass SVM | anomaly detection | Credit card fraud detection
Fast Tree | binary classification, regression | Page ranking by search
Fast Forest | binary classification, regression | Churn Prediction
Neural Network | binary and multiclass classification, regression | Check signature recognition, OCR, Click Prediction 
Logistic regression | binary and multiclass classification | Classifying sentiments from feedback

For more information, see [MicrosoftML functions](microsoftml/microsoftml-introduction.md)and the documentation for the individual transforms and functions in the product help.

## Getting started with MicrosoftML

MicrosoftML functions are provided through the **MicrosoftML** package installed for free in [Microsoft R Client](r-client.md) or commercially in [Microsoft R Server](rserver.md) on supported platforms. 


## Working with MicrosoftML

Data scientists and developers can include MicrosoftML functions in custom script or solutions that run locally against R Client or remotely on R Server. 

A common workflow is to write the initial code or script against a subset of data on a local computer, change the compute context to specify a large set of data on a big data platform, and then operationalize the solution by deploying it to the target environment, making it accessible to users.
 

## See Also

[Introduction to Microsoft R](microsoft-r-getting-started.md)

[Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md)

