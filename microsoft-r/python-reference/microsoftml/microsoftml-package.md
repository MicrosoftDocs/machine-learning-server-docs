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

The **microsoftml** module is a collection of Python functions used in machine learning operations. It includes functions for training and transformations, scoring, text and image analysis, and feature extraction for deriving values from existing data.

**Ships in:** [SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services), [SQL Server 2017 Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server)

**Built on:** [Anaconda](https://www.continuum.io/why-anaconda) distribution of [Python 3.5](https://www.python.org/doc) (included when you add Python support during installation). 

## How to use microsoftml

The **microsoftml** module is installed as part of SQL Server Machine Learning. When you add Python to your installation, you get the full set of proprietary packages plus a Python distribution with its modules and interpreters. You can use any Python IDE to write Python script calling functions in **microsoftml**, but the script must run on a computer having SQL Server Machine Learning with Python.

There are two primary use cases for this release: 

+ Calling Python functions in T-SQL script or stored procedures running on SQL Server.  
+ Calling **microsoftml** functions in Python script executing in a SQL Server [compute context](../r/concept-what-is-compute-context.md). 

Setup adds Python 3.5 to your path. There are no limitations on the Python functions you can call. Your code can call functions from **microsoftml**, [**revoscalepy**](../revoscalepy/revoscalepy-package.md), or any 35-compatible module you have installed on the computer.

## Functions by category

This section lists the functions by category to give you an idea of how each one is used. You can use the TOC to find functions listed in alphabetical order.

### Training functions

* [*microsoftml.rx_fast_forest*: Random Forest](rx-fast-forest.md) 
* [*microsoftml.rx_fast_linear*: Linear Model with Stochastic Dual Coordinate Ascent](rx-fast-linear.md) 
* [*microsoftml.rx_fast_trees*: Boosted Trees](rx-fast-trees.md) 
* [*microsoftml.rx_logistic_regression*: Logistic Regression](rx-logistic-regression.md) 
* [*microsoftml.rx_neural_network*: Neural Network](rx-neural-network.md) 
* [*microsoftml.rx_oneclass_svm*: Anomaly Detection](rx-oneclass-svm.md) 


### Transform functions

#### Categorical variable handling

* [*microsoftml.categorical*: Converts a text column into categories](categorical.md) 
* [*microsoftml.categorical_hash*: Hashes and converts a text column into categories](categorical-hash.md) 

#### Schema manipulation

* [*microsoftml.concat*: Concatenates multiple columns into a single vector](concat.md) 
* [*microsoftml.drop_columns*: Drops columns from a dataset](drop-columns.md) 
* [*microsoftml.select_columns*: Retains columns of a dataset](select-columns.md) 


#### Variable selection

* [*microsoftml.count_select*: Feature selection based on counts](count-select.md) 
* [*microsoftml.mutualinformation_select*: Feature selection based on mutual information](mutualinformation-select.md) 


#### Text analytics

* [*microsoftml.featurize_text*: Converts text columns into numerical features](featurize-text.md) 
  * [N-grams extractors](featurize-text.md) 
    * [*microsoftml.n_gram*: Converts text into features using n-grams](n-gram.md) 
    * [*microsoftml.n_gram_hash*: Converts text into features using hashed n-grams](n-gram-hash.md) 
  * [Stopwords removers](featurize-text.md) 
    * [*microsoftml.custom*: Removes custom stopwords](custom.md) 
    * [*microsoftml.predefined*: Removes predefined stopwords](predefined.md) 
* [*microsoftml.get_sentiment*: Sentiment analysis](get-sentiment.md) 


#### Image analytics 

* [*microsoftml.load_image*: Loads an image](load-image.md) 
* [*microsoftml.resize_image*: Resizes an Image](resize-image.md) 
* [*microsoftml.extract_pixels*: Extracts pixels form an image](extract-pixels.md) 
* [*microsoftml.featurize_image*: Converts an image into features](featurize-image.md) 


#### Scoring fucntions

* [*microsoftml.rx_predict*: Scores using a Microsoft machine learning model](rx-predict.md) 


#### Featurization functions

* [*microsoftml.rx_featurize*: Data transformation for data sources](rx-featurize.md) 


## Next steps

Get started with these Python libraries in SQL Server 2017: [Set up Python Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/setup-python-machine-learning-services).

Continue on with the following tutorial to further your understanding:

+ [Using the MicrosoftML Package with SQL Server](https://docs.microsoft.com/en-us/sql/advanced-analytics/using-the-microsoftml-package) 

## See also

  [SQL Server Machine Learning Services with Python](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services)  
  [SQL Server Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) 