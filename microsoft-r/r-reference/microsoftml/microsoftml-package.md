--- 
 
# required metadata 
title: "MicrosoftML package for R (Microsoft Machine Learning Server) | Microsoft Docs" 
description: "Function help reference for the MicrosoftML R package of Microsoft Machine Learning Server." 
keywords: "MicrosoftML-package, package" 
author: "bradsev"
ms.author: "bradsev" 
manager: "jhubbard" 
ms.date: "06/20/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
# MicrosoftML package for R

Applies to: [**Microsoft Machine Learning Server**](../what-is-microsoft-r-server.md) version 9.2, [**SQL Server 2017 Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/python/sql-server-python-services), [**SQL Server 2017 Machine Learning Server (Standalone)](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone#whats-new-in-microsoft-machine-learning-server)
 
The **MicrosoftML** package provides state-of-the-art fast, scalable machine learning algorithms and transforms for R. The package is used with the **RevoScaleR** package.

This topic includes links to the reference documentation for the ML algorithms and transforms, and for the scoring and helper functions.

| Item | Data |
| :---| :--- |
|  Package  |  MicrosoftML |
|  Type  |  Package |
|  Version  |  1.0.0 |
|  License  |  file LICENSE |
|  LazyLoad  |  yes |


##Key algorithms and transforms in the package

<a name="ml-algorithms"></a>
### Machine learning algorithms

* [rxFastTrees](rxfasttrees.md): An implementation of FastRank, an efficient implementation  of the MART gradient boosting algorithm.  
* [rxFastForest](rxfastforest.md): A random forest and Quantile regression forest  implementation using [rxFastTrees](rxfasttrees.md).  
* [rxLogisticRegression](logisticregression.md): Logistic regression using L-BFGS.  
* [rxOneClassSvm](rxoneclasssvm.md): One class support vector machines.  
* [rxNeuralNet](rxneuralnet.md): Binary, multi-class, and regression neural net.  
* [rxFastLinear](rxfastlinear.md): Stochastic dual coordinate ascent optimization for linear binary classification and regression. 
* [rxEnsemble](rxensemble.md): trains a number of models of various kinds to obtain better predictive performance than could be obtained from a single model.


<a name="ml-transforms"></a>
### Machine learning transforms

* [concat](concat.md): Transformation to create a single vector-valued column from multiple columns.  
* [categorical](categorical.md): Create indicator vector using categorical transform with dictionary.  
* [categoricalHash](categoricalhash.md): Converts the categorical value into an indicator array by hashing. 
* [featurizeText](featurizetext.md): Produces a bag of counts of sequences of consecutive words, called n-grams, from a given corpus of text. It offers language detection, tokenization, stopwords removing, text normalization and feature generation.  
* [getSentiment](getsentiment.md): Scores natural language text and creates a column that contains probabilities that the sentiments in the text are positive.
* [ngram](ngram.md): allows defining arguments for count-based and hash-based feature extraction.
* [selectFeatures](selectfeatures.md): Selects features from the specified variables using a specified mode.
* [loadImage](loadimage.md): Loads image data.
* [resizeImage](resizeimage.md): Resizes an image to a specified dimension using a specified resizing method.
* [extractPixels](extractpixels.md): Extracts the pixel values from an image.
* [featurizeImage](featurizeimage.md): Featurizes an image using a pre-trained deep neural network model.


### Scoring and training and model summary

* [rxPredict.mlModel](rxpredict.md): Runs the scoring library either from SQL Server, using the stored procedure, or from R code enabling real-time scoring to provide much faster prediction performance.
* [rxFeaturize](rxfeaturize.md): Transforms data from an input data set to an output data set.
* [mlModel](mlmodel.md) Provides a summary of a Microsoft R Machine Learning model.


### Helper functions

**Loss functions for classification and regression.**

* [expLoss](loss.md): Specifications for exponential classification loss function.  
* [logLoss](loss.md): Specifications for log classification loss function.  
* [hingeLoss](loss.md): Specifications for hinge classification loss function.  
* [smoothHingeLoss](loss.md): Specifications for smooth hinge classification loss function.  
* [poissonLoss](loss.md): Specifications for poisson regression loss function.  
* [squaredLoss](loss.md): Specifications for squared regression loss function.      

**Functions for feature selection.**

* [minCount](mincount.md): Specification for feature selection in count mode. 
* [mutualInformation](mutualinformation.md): Specification for feature selection in mutual information mode. 

**Functions for ensemble modeling.**

* [fastTrees](fasttrees.md): Creates a list containing the function name and arguments to train a Fast Tree model with [rxEnsemble](rxensemble.md).
* [fastForest](rxfastforest.md): Creates a list containing the function name and arguments to train a Fast Forest model with [rxEnsemble](rxensemble.md).
* [fastLinear](fastlinear.md): Creates a list containing the function name and arguments to train a Fast Linear model with [rxEnsemble](rxensemble.md).
* [logisticRegression](logisticregression.md): Creates a list containing the function name and arguments to train a  Logistic Regression model with [rxEnsemble](rxensemble.md).
* [oneClassSvm](oneclasssvm.md)Creates a list containing the function name and arguments to train a OneClassSvm model with [rxEnsemble](rxensemble.md).
 
**Functions for neural networks.**
* [optimizer](optimizer.md) Specifies optimization algorithms for the [rxNeuralNet](rxneuralnet.md) machine learning algorithm.

 
## What's next?

[Introduction to Microsoft R](../../microsoft-r-getting-started.md)

[Diving into data analysis in Microsoft R](../../r/how-to-introduction.md)

[Cheat Sheet: How to choose a MicrosoftML algorithm](../../r/how-to-choose-microsoftml-algorithms-cheatsheet.md)

[MicrosoftML samples](../../r/sample-microsoftml.md).


##Microsoft Technical Support
To request technical support from the Microsoft Corporation, click on [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409).
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
