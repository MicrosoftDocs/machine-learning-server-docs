--- 
 
# required metadata 
title: "microsoftml package for Python (Microsoft Machine Learning Server and SQL Server Machine Learning Server) " 
description: "Function help reference for the microsoftml python package of SQL Server Machine Learning Server." 
keywords: "microsoftml API, API" 
author: "dphansen"
ms.author: "davidph" 
manager: "cgronlun" 
ms.date: "09/25/2017" 
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
 
---

# microsoftml package

The **microsoftml** module is a collection of Python functions used in machine learning solutions. It includes functions for training and transformations, scoring, text and image analysis, and feature extraction for deriving values from existing data.

| Package details | Information |
|--------|-|
| Current version: |  9.4 |
| Built on: | [Anaconda 4.2](https://www.continuum.io/why-anaconda) distribution of [Python 3.7.1](https://www.python.org/doc) |
| Package distribution: | [Machine Learning Server 9.x](../../what-is-machine-learning-server.md) </br>[SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services) </br>[SQL Server 2017 Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) |


## How to use microsoftml

The **microsoftml** module is installed as part of Microsoft Machine Learning Server or SQL Server Machine Learning when you add Python to your installation. You get the full collection of proprietary packages plus a Python distribution with its modules and interpreters. You can use any Python IDE to write Python script calling functions in **microsoftml**, but the script must run on a computer having either Microsoft Machine Learning Server or SQL Server Machine Learning Server with Python.

**Microsoftml** and **revoscalepy** are tightly coupled; data sources used in **microsoftml** are defined as [**revoscalepy**](../revoscalepy/revoscalepy-package.md) objects. Compute context limitations in **revoscalepy** transfer to **microsoftml**. Namely, all functionality is available for local operations, but switching to a remote compute context requires [RxSpark](../revoscalepy/RxSpark.md) or [RxInSQLServer](../revoscalepy/RxInSQLServer.md).

## Functions by category

This section lists the functions by category to give you an idea of how each one is used. You can also use the table of contents to find functions in alphabetical order.

## 1-Training functions

| Function | Description |
|----------|-------------|
|[microsoftml.rx_ensemble](rx-ensemble.md) | Train an ensemble of models |
|[microsoftml.rx_fast_forest](rx-fast-forest.md)  | Random Forest |
|[microsoftml.rx_fast_linear](rx-fast-linear.md) | Linear Model with Stochastic Dual Coordinate Ascent |
|[microsoftml.rx_fast_trees](rx-fast-trees.md) | Boosted Trees |
|[microsoftml.rx_logistic_regression](rx-logistic-regression.md) | Logistic Regression |
|[microsoftml.rx_neural_network](rx-neural-network.md) | Neural Network |
|[microsoftml.rx_oneclass_svm](rx-oneclass-svm.md) | Anomaly Detection |

<a name="ml-transforms"></a>

## 2-Transform functions

### Categorical variable handling

| Function | Description |
|----------|-------------|
|[microsoftml.categorical](categorical.md) | Converts a text column into categories |
|[microsoftml.categorical_hash](categorical-hash.md) | Hashes and converts a text column into categories |

### Schema manipulation

| Function | Description |
|----------|-------------|
|[microsoftml.concat](concat.md) | Concatenates multiple columns into a single vector |
|[microsoftml.drop_columns](drop-columns.md) | Drops columns from a dataset |
|[microsoftml.select_columns](select-columns.md) | Retains columns of a dataset |


### Variable selection

| Function | Description |
|----------|-------------|
|[microsoftml.count_select](count-select.md) |Feature selection based on counts |
|[microsoftml.mutualinformation_select](mutualinformation-select.md) | Feature selection based on mutual information |


### Text analytics

| Function | Description |
|----------|-------------|
|[microsoftml.featurize_text](featurize-text.md) | Converts text columns into numerical features |
|[microsoftml.get_sentiment](get-sentiment.md) | Sentiment analysis |


### Image analytics 

| Function | Description |
|----------|-------------|
|[microsoftml.load_image](load-image.md) | Loads an image |
|[microsoftml.resize_image](resize-image.md) | Resizes an Image |
|[microsoftml.extract_pixels](extract-pixels.md) | Extracts pixels from an image |
|[microsoftml.featurize_image](featurize-image.md) | Converts an image into features |


### Scoring functions

| Function | Description |
|----------|-------------|
|[microsoftml.rx_predict](rx-predict.md) | Scores using a Microsoft machine learning model |


### Featurization functions

| Function | Description |
|----------|-------------|
|[microsoftml.rx_featurize](rx-featurize.md) | Data transformation for data sources |


## Next steps

For SQL Server, add both Python modules to your computer by running setup: 

+ [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Follow this SQL Server tutorial for hands-on experience: 

+ [Using the MicrosoftML Package with SQL Server](https://docs.microsoft.com/sql/advanced-analytics/using-the-microsoftml-package) 

## See also

  [Machine Learning Server](../../what-is-machine-learning-server.md)  
  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) 