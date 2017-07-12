--- 
 
# required metadata 
title: "" 
description: "" 
keywords: "microsoftml API, API" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/12/2017" 
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

# learners


**Applies to: SQL Server 2017, Machine Learning Services 9.3**


## training function

* [*microsoftml.rx_fast_forest*: Random Forest](rx_fast_forest.md) 

* [*microsoftml.rx_fast_linear*: Fast Linear Model – Stochastic Dual Coordinate Ascent](rx_fast_linear.md) 

  * [loss functions](rx_fast_linear.md) 

    * [*microsoftml.hinge_loss*: Hinge loss function, options for *rx_fast_linear*](hinge_loss.md) 

    * [*microsoftml.log_loss*: Log loss function, options for *rx_fast_linear*](log_loss.md) 

    * [*microsoftml.smoothed_hinge_loss*: Smoothed hinge loss function](smoothed_hinge_loss.md) 

    * [*microsoftml.squared_loss*: Square loss function](squared_loss.md) 

* [*microsoftml.rx_fast_trees*: Fast Tree](rx_fast_trees.md) 

* [*microsoftml.rx_logistic_regression*: Logistic Regression](rx_logistic_regression.md) 

* [*microsoftml.rx_neural_network*: Neural Network](rx_neural_network.md) 

  * [optimizers](rx_neural_network.md) 

    * [*microsoftml.adadelta_optimizer*: Adaptive learing rate method](adadelta_optimizer.md) 

    * [*microsoftml.sgd_optimizer*: Stochastic Gradient Descent](sgd_optimizer.md) 

  * [math](rx_neural_network.md) 

    * [*microsoftml.avx_math*: Options for *rx_neural_network*](avx_math.md) 

    * [*microsoftml.clr_math*: Options for *rx_neural_network*](clr_math.md) 

    * [*microsoftml.gpu_math*: Options for *rx_neural_network*](gpu_math.md) 

    * [*microsoftml.mkl_math*: Options for *rx_neural_network*](mkl_math.md) 

    * [*microsoftml.sse_math*](sse_math.md) 

* [*microsoftml.rx_oneclass_svm*: Detects Anomalies](rx_oneclass_svm.md) 


# transforms


## categorical variable handling

* [*microsoftml.categorical*: Converts a text column into categories](categorical.md) 

* [*microsoftml.categorical_hash*: Hashes and converts a text column into categories](categorical_hash.md) 


## schema manipulation

* [*microsoftml.concat*: Concatenates multiple columns into a single vector](concat.md) 

* [*microsoftml.drop_columns*: Selects and drops a subset of columns](drop_columns.md) 

* [*microsoftml.select_columns*: Selects and keeps a subset of columns](select_columns.md) 


## variable selection

* [*microsoftml.count_select*: Counts and filters out features](count_select.md) 

* [*microsoftml.mutualinformation_select*: Feature Selection based on Mutual Information](mutualinformation_select.md) 


## text analytics

* [*microsoftml.featurize_text*: Converts text columns into numerical features](featurize_text.md) 

  * [N-grams extractors](featurize_text.md) 

    * [*microsoftml.n_gram*: Converts text into features using N-Grams](n_gram.md) 

    * [*microsoftml.n_gram_hash*: Converts text into features using hashed N-Grams](n_gram_hash.md) 

  * [Stopwords removers](featurize_text.md) 

    * [*microsoftml.custom*: Removes custom stop words](custom.md) 

    * [*microsoftml.predefined*: Removes stop words](predefined.md) 

* [*microsoftml.get_sentiment*: Sentiment Analyzer](get_sentiment.md) 


## image analytics

* [*microsoftml.featurize_image*: Converts an image into features](featurize_image.md) 

* [*microsoftml.load_image*: Loads an image](load_image.md) 

* [*microsoftml.resize_image*: Resizes an Image](resize_image.md) 

* [*microsoftml.extract_pixels*: Extracts pixels form an image](extract_pixels.md) 


# scorers

* [*microsoftml.rx_predict*: Scores using a Microsoft ML Machine Learning model](rx_predict.md) 


# featurizers

* [*microsoftml.rx_featurize*: Data Transformation for Data Sources](rx_featurize.md) 
