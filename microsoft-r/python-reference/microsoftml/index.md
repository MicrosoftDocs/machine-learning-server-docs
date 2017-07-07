--- 
 
# required metadata 
title: "" 
description: "" 
keywords: "microsoftml API, API" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
ms.topic: "reference" 
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

## learners


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### training function

* [``rx_fast_forest``: Random Forest](rx_fast_forest.md) 

* [``rx_fast_linear``: Fast Linear Model – Stochastic Dual Coordinate Ascent](rx_fast_linear.md) 

* [``rx_fast_trees``: Fast Tree](rx_fast_trees.md) 

* [``rx_logistic_regression``: Logistic Regression](rx_logistic_regression.md) 

* [``rx_neural_network``: Neural Network](rx_neural_network.md) 

* [``rx_oneclass_svm``: Detect Anomalies](rx_oneclass_svm.md) 


### models

* [Learners Objects](learners_object.md) 

  * [Base Learner](learners_object.md) 

  * [Specific Learners](learners_object.md) 


## transforms


### categorical variable handling

* [``categorical``: Convert text column into categories](categorical.md) 

* [``categorical_hash``: Hash and convert text column into categories](categorical_hash.md) 


### schema manipulation

* [``concat``: Concatenate multiple columns into a single vector](concat.md) 

* [``drop_columns``: Select and drop a subset of columns](drop_columns.md) 

* [``select_columns``: Select and keep a subset of columns](select_columns.md) 


### variable selection

* [``count_select``: Count and filter out features](count_select.md) 

* [``mutualinformation_select``: Feature Selection Mutual Information](mutualinformation_select.md) 


### text analytics

* [``featurize_text``: Convert text columns into numerical features](featurize_text.md) 

* [``n_gram``: Convert text into features using N-Grams](n_gram.md) 

* [``n_gram_hash``: Convert text into features using hashed N-Grams](n_gram_hash.md) 

* [``custom``: Remove Custom Stop Words](custom.md) 

* [``predefined``: Remove Stop Words](predefined.md) 

* [``get_sentiment``: Sentiment Analyzer](get_sentiment.md) 


### image analytics

* [``featurize_image``: Convert an image into features](featurize_image.md) 

* [``load_image``: Load an image](load_image.md) 

* [``resize_image``: Resize an Image](resize_image.md) 

* [``extract_pixels``: Extract pixels form an image](extract_pixels.md) 


## scorers

* [``rx_predict``: Score using a Microsoft ML Machine Learning model](rx_predict.md) 

featurizers
========== =

* [``rx_featurize``: Data Transformation for Data Sources](rx_featurize.md) 


## optimizers

* [``adadelta_optimizer``: Adaptive learing rate method](adadelta_optimizer.md) 

* [``sgd_optimizer``: Stochastic Gradient Descent](sgd_optimizer.md) 


## loss functions

* [``hinge_loss``: Hinge Loss Function](hinge_loss.md) 

* [``log_loss``: Log Loss Function](log_loss.md) 

* [``smoothed_hinge_loss``: Smoothed Hinge Loss Function](smoothed_hinge_loss.md) 

* [``squared_loss``: Square Loss Function](squared_loss.md) 


## math

* [``avx_math``](avx_math.md) 

* [``clr_math``](clr_math.md) 

* [``gpu_math``](gpu_math.md) 

* [``mkl_math``](mkl_math.md) 

* [``sse_math``](sse_math.md) 
