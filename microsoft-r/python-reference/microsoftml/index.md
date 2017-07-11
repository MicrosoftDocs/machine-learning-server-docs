--- 
 
# required metadata 
title: "" 
description: "" 
keywords: "microsoftml API, API" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/11/2017" 
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

## learners


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### training function

* [*rx_fast_forest*: Random Forest](rx_fast_forest.md) 

* [*rx_fast_linear*: Fast Linear Model – Stochastic Dual Coordinate Ascent](rx_fast_linear.md) 

  * [loss functions](rx_fast_linear.md) 

    * [*hinge_loss*: Hinge Loss Function](hinge_loss.md) 

    * [*log_loss*: Log loss function](log_loss.md) 

    * [*smoothed_hinge_loss*: Smoothed hinge loss function](smoothed_hinge_loss.md) 

    * [*squared_loss*: Square loss function](squared_loss.md) 

* [*rx_fast_trees*: Fast Tree](rx_fast_trees.md) 

* [*rx_logistic_regression*: Logistic Regression](rx_logistic_regression.md) 

* [*rx_neural_network*: Neural Network](rx_neural_network.md) 

  * [optimizers](rx_neural_network.md) 

    * [*adadelta_optimizer*: Adaptive learing rate method](adadelta_optimizer.md) 

    * [*sgd_optimizer*: Stochastic Gradient Descent](sgd_optimizer.md) 

  * [math](rx_neural_network.md) 

    * [*avx_math*](avx_math.md) 

    * [*clr_math*](clr_math.md) 

    * [*gpu_math*](gpu_math.md) 

    * [*mkl_math*](mkl_math.md) 

    * [*sse_math*](sse_math.md) 

* [*rx_oneclass_svm*: Detects Anomalies](rx_oneclass_svm.md) 


## transforms


### categorical variable handling

* [*categorical*: Converts a text column into categories](categorical.md) 

* [*categorical_hash*: Hashes and converts a text column into categories](categorical_hash.md) 


### schema manipulation

* [*concat*: Concatenates multiple columns into a single vector](concat.md) 

* [*drop_columns*: Selects and drops a subset of columns](drop_columns.md) 

* [*select_columns*: Selects and keeps a subset of columns](select_columns.md) 


### variable selection

* [*count_select*: Counts and filters out features](count_select.md) 

* [*mutualinformation_select*: Feature Selection based on Mutual Information](mutualinformation_select.md) 


### text analytics

* [*featurize_text*: Converts text columns into numerical features](featurize_text.md) 

  * [N-grams extractors](featurize_text.md) 

    * [*n_gram*: Converts text into features using N-Grams](n_gram.md) 

    * [*n_gram_hash*: Converts text into features using hashed N-Grams](n_gram_hash.md) 

  * [Stopwords removers](featurize_text.md) 

    * [*custom*: Removes custom stop words](custom.md) 

    * [*predefined*: Removes stop words](predefined.md) 

* [*get_sentiment*: Sentiment Analyzer](get_sentiment.md) 


### image analytics

* [*featurize_image*: Converts an image into features](featurize_image.md) 

* [*load_image*: Loads an image](load_image.md) 

* [*resize_image*: Resizes an Image](resize_image.md) 

* [*extract_pixels*: Extracts pixels form an image](extract_pixels.md) 


## scorers

* [*rx_predict*: Scores using a Microsoft ML Machine Learning model](rx_predict.md) 


## featurizers

* [*rx_featurize*: Data Transformation for Data Sources](rx_featurize.md) 
