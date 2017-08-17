--- 
 
# required metadata 
title: "microsoftml package for Python (Microsoft Machine Learning Server) | Microsoft Docs" 
description: "Function help reference for the microsoftml python package of Microsoft Machine Learning Server." 
keywords: "microsoftml API, API" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/13/2017" 
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

Applies to: [**Microsoft Machine Learning Server**](../what-is-microsoft-r-server.md) version 9.2, [**SQL Server 2017 Machine Learning Services**](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services), [**SQL Server 2017 Machine Learning Server (Standalone)**](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server)

The **microsoftml** library is a proprietary Python package used for applying machine learning transforms to data.

## How to use microsoftml

TBD


## Training functions

* [*microsoftml.rx_fast_forest*: Random Forest](rx-fast-forest.md) 
* [*microsoftml.rx_fast_linear*: Linear Model with Stochastic Dual Coordinate Ascent](rx-fast-linear.md) 
* [*microsoftml.rx_fast_trees*: Boosted Trees](rx-fast-trees.md) 
* [*microsoftml.rx_logistic_regression*: Logistic Regression](rx-logistic-regression.md) 
* [*microsoftml.rx_neural_network*: Neural Network](rx-neural-network.md) 
* [*microsoftml.rx_oneclass_svm*: Anomaly Detection](rx-oneclass-svm.md) 


## Transform functions

### Categorical Variable Handling

* [*microsoftml.categorical*: Converts a text column into categories](categorical.md) 
* [*microsoftml.categorical_hash*: Hashes and converts a text column into categories](categorical-hash.md) 


### Schema Manipulation

* [*microsoftml.concat*: Concatenates multiple columns into a single vector](concat.md) 
* [*microsoftml.drop_columns*: Drops columns from a dataset](drop-columns.md) 
* [*microsoftml.select_columns*: Retains columns of a dataset](select-columns.md) 


### Variable Selection

* [*microsoftml.count_select*: Feature selection based on counts](count-select.md) 
* [*microsoftml.mutualinformation_select*: Feature selection based on mutual information](mutualinformation-select.md) 


### Text Analytics

* [*microsoftml.featurize_text*: Converts text columns into numerical features](featurize-text.md) 
  * [N-grams extractors](featurize-text.md) 
    * [*microsoftml.n_gram*: Converts text into features using n-grams](n-gram.md) 
    * [*microsoftml.n_gram_hash*: Converts text into features using hashed n-grams](n-gram-hash.md) 
  * [Stopwords removers](featurize-text.md) 
    * [*microsoftml.custom*: Removes custom stopwords](custom.md) 
    * [*microsoftml.predefined*: Removes predefined stopwords](predefined.md) 
* [*microsoftml.get_sentiment*: Sentiment analysis](get-sentiment.md) 


### Image Analytics

* [*microsoftml.load_image*: Loads an image](load-image.md) 
* [*microsoftml.resize_image*: Resizes an Image](resize-image.md) 
* [*microsoftml.extract_pixels*: Extracts pixels form an image](extract-pixels.md) 
* [*microsoftml.featurize_image*: Converts an image into features](featurize-image.md) 


### Scorers

* [*microsoftml.rx_predict*: Scores using a Microsoft machine learning model](rx-predict.md) 


### Featurization

* [*microsoftml.rx_featurize*: Data transformation for data sources](rx-featurize.md) 


## Next steps

For more information on using this library, see our articles in [Quickstarts]() and [How-to guidance]().

## See also

 [Python function library help (Machine Learning Server)](../introducing-python-package-reference.md)   