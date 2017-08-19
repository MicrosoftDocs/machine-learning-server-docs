--- 
 
# required metadata 
title: "microsoftml package for Python (SQL Server Machine Learning Server) | Microsoft Docs" 
description: "Function help reference for the microsoftml python package of SQL Server Machine Learning Server." 
keywords: "microsoftml API, API" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "08/13/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# microsoftml package for Python

The **microsoftml** module is a collection of Python functions used in machine learning workloads. It includes functions for training and transformations, scoring, text and image analysis, and feature extraction for deriving values from existing data.

**Version:** 1.3.4

**Supported on:** [SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services), [SQL Server 2017 Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server)

**Built on:** [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc) (included when you add Python support during installation). 

## How to use microsoftml

The **microsoftml** module is installed as part of SQL Server Machine Learning when you add Python to your installation. You get the full collection of proprietary packages plus a Python distribution with its modules and interpreters. You can use any Python IDE to write Python script calling functions in **microsoftml**, but the script must run on a computer having SQL Server Machine Learning with Python.

There are two primary use cases for this release: 

+ Calling Python functions in T-SQL script or stored procedures running on SQL Server.  
+ Calling **microsoftml** functions in Python script executing in a SQL Server [compute context](../r/concept-what-is-compute-context.md). In your script, you can set a compute context to shift execution of **microsoftml** operations to a remote SQL Server instance that has the **microsoftml** interpreter.

Setup adds Python 3.5 to your path. You can use Python as you would normally, calling functions from any 35-compatible module you have installed on the computer.

## Functions by category

This section lists the functions by category to give you an idea of how each one is used. You can also use the table of contents to find functions in alphabetical order.

### Training functions

| Function | Description |
|----------|-------------|
|[microsoftml.rx_fast_forest](rx-fast-forest.md)  | Random Forest |
|[microsoftml.rx_fast_linear](rx-fast-linear.md) | Linear Model with Stochastic Dual Coordinate Ascent |
|[microsoftml.rx_fast_trees](rx-fast-trees.md) | Boosted Trees |
|[microsoftml.rx_logistic_regression](rx-logistic-regression.md) | Logistic Regression |
|[microsoftml.rx_neural_network](rx-neural-network.md) | Neural Network |
|[microsoftml.rx_oneclass_svm](rx-oneclass-svm.md) | Anomaly Detection |


### Transform functions

#### Categorical variable handling

| Function | Description |
|----------|-------------|
|[microsoftml.categorical](categorical.md) | Converts a text column into categories |
|[microsoftml.categorical_hash](categorical-hash.md) | Hashes and converts a text column into categories |

#### Schema manipulation

| Function | Description |
|----------|-------------|
|[microsoftml.concat](concat.md) | Concatenates multiple columns into a single vector |
|[microsoftml.drop_columns](drop-columns.md) | Drops columns from a dataset |
|[microsoftml.select_columns](select-columns.md) | Retains columns of a dataset |


#### Variable selection

| Function | Description |
|----------|-------------|
|[microsoftml.count_select](count-select.md) |Feature selection based on counts |
|[microsoftml.mutualinformation_select](mutualinformation-select.md) | Feature selection based on mutual information |


#### Text analytics

| Function | Description |
|----------|-------------|
|[microsoftml.featurize_text](featurize-text.md) | Converts text columns into numerical features |
|[microsoftml.get_sentiment](get-sentiment.md) | Sentiment analysis |


#### Image analytics 

| Function | Description |
|----------|-------------|
|[microsoftml.load_image](load-image.md) | Loads an image |
|[microsoftml.resize_image](resize-image.md) | Resizes an Image |
|[microsoftml.extract_pixels](extract-pixels.md) | Extracts pixels form an image |
|[microsoftml.featurize_image](featurize-image.md) | Converts an image into features |


#### Scoring fucntions

| Function | Description |
|----------|-------------|
|[microsoftml.rx_predict](rx-predict.md) | Scores using a Microsoft machine learning model |


#### Featurization functions

| Function | Description |
|----------|-------------|
|[microsoftml.rx_featurize](rx-featurize.md) | Data transformation for data sources |


## Next steps

Add both Python modules to your computer by running setup: [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Next, follow this tutorial for hands on experience: [Using the MicrosoftML Package with SQL Server](https://docs.microsoft.com/en-us/sql/advanced-analytics/using-the-microsoftml-package) 

## See also

  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) 