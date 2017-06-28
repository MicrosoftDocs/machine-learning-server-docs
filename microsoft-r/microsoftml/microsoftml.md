--- 
 
# required metadata 
title: "MicrosoftML: machine learning R algorithms" 
description: "A package that provides state-of-the-art machine learning algorithms for R, developed  by Microsoft. It is used with the **RevoScaleR** package." 
keywords: "MicrosoftML-package, package" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "06/20/2017" 
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
 
 
 
# MicrosoftML: machine learning R algorithms  
 
The MicrosoftML package provides state-of-the-art fast, scalable machine learning algorithms and transforms for R. The package is used with the **RevoScaleR** package.

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

* [rxFastTrees](packagehelp/rxFastTrees.md): An implementation of FastRank, an efficient implementation  of the MART gradient boosting algorithm.  
* [rxFastForest](packagehelp/rxFastForest.md): A random forest and Quantile regression forest  implementation using [rxFastTrees](packagehelp/rxFastTrees.md).  
* [rxLogisticRegression](packagehelp/rxLogisticRegression.md): Logistic regression using L-BFGS.  
* [rxOneClassSvm](packagehelp/rxOneClassSvm.md): One class support vector machines.  
* [rxNeuralNet](packagehelp/rxNeuralNet.md): Binary, multi-class, and regression neural net.  
* [rxFastLinear](packagehelp/rxFastLinear.md): Stochastic dual coordinate ascent optimization for linear binary classification and regression. 
* [rxEnsemble](packagehelp/rxEnsemble.md): trains a number of models of various kinds to obtain better predictive performance than could be obtained from a single model.


<a name="ml-transforms"></a>
### Machine learning transforms

* [concat](../r-reference/microsoftml/concat.md): Transformation to create a single vector-valued column from multiple columns.  
* [categorical](../r-reference/microsoftml/categorical.md): Create indicator vector using categorical transform with dictionary.  
* [categoricalHash](../r-reference/microsoftml/categoricalhash.md): Converts the categorical value into an indicator array by hashing. 
* [featurizeText](../r-reference/microsoftml/featurizetext.md): Produces a bag of counts of sequences of consecutive words, called n-grams, from a given corpus of text. It offers language detection, tokenization, stopwords removing, text normalization and feature generation.  
* [getSentiment](../r-reference/microsoftml/getsentiment.md): Scores natural language text and creates a column that contains probabilities that the sentiments in the text are positive.
* [ngram](packagehelp/ngram.md): allows defining arguments for count-based and hash-based feature extraction.
* [selectFeatures](packagehelp/selectFeatures.md): Selects features from the specified variables using a specified mode.
* [loadImage](../r-reference/microsoftml/loadimage.md): Loads image data.
* [resizeImage](packagehelp/resizeImage.md): Resizes an image to a specified dimension using a specified resizing method.
* [extractPixels](../r-reference/microsoftml/extractpixels.md): Extracts the pixel values from an image.
* [featurizeImage](../r-reference/microsoftml/featurizeimage.md): Featurizes an image using a pre-trained deep neural network model.


### Scoring and training and model summary

* [rxPredict.mlModel](packagehelp/rxPredict.md): Runs the scoring library either from SQL Server, using the stored procedure, or from R code enabling real-time scoring to provide much faster prediction performance.
* [rxFeaturize](packagehelp/rxFeaturize.md): Transforms data from an input data set to an output data set.
* [mlModel](packagehelp/mlModel.md) Provides a summary of a Microsoft R Machine Learning model.


### Helper functions

**Loss functions for classification and regression.**

* [expLoss](packagehelp/loss.md): Specifications for exponential classification loss function.  
* [logLoss](packagehelp/loss.md): Specifications for log classification loss function.  
* [hingeLoss](packagehelp/loss.md): Specifications for hinge classification loss function.  
* [smoothHingeLoss](packagehelp/loss.md): Specifications for smooth hinge classification loss function.  
* [poissonLoss](packagehelp/loss.md): Specifications for poisson regression loss function.  
* [squaredLoss](packagehelp/loss.md): Specifications for squared regression loss function.      

**Functions for feature selection.**

* [minCount](packagehelp/minCount.md): Specification for feature selection in count mode. 
* [mutualInformation](packagehelp/mutualInformation.md): Specification for feature selection in mutual information mode. 

**Functions for ensemble modeling.**

* [fastTrees](../r-reference/microsoftml/fasttrees.md): Creates a list containing the function name and arguments to train a Fast Tree model with [rxEnsemble](packagehelp/rxEnsemble.md).
* [fastForest](packagehelp/rxFastForest.md): Creates a list containing the function name and arguments to train a Fast Forest model with [rxEnsemble](packagehelp/rxEnsemble.md).
* [fastLinear](../r-reference/microsoftml/fastlinear.md): Creates a list containing the function name and arguments to train a Fast Linear model with [rxEnsemble](packagehelp/rxEnsemble.md).
* [logisticRegression](packagehelp/logisticRegression.md): Creates a list containing the function name and arguments to train a  Logistic Regression model with [rxEnsemble](packagehelp/rxEnsemble.md).
* [oneClassSvm](packagehelp/oneClassSvm.md)Creates a list containing the function name and arguments to train a OneClassSvm model with [rxEnsemble](packagehelp/rxEnsemble.md).
 
**Functions for neural networks.**
* [optimizer](packagehelp/optimizer.md) Specifies optimization algorithms for the [rxNeuralNet](packagehelp/rxNeuralNet.md) machine learning algorithm.

 
## What's next?

[Introduction to Microsoft R](../microsoft-r-getting-started.md)

[Diving into data analysis in Microsoft R](../data-analysis-in-microsoft-r.md)

[Cheat Sheet: How to choose a MicrosoftML algorithm](../microsoftml-algorithm-cheat-sheet.md)

[MicrosoftML samples](../microsoftml-quickstarts.md).


##Microsoft Technical Support
To request technical support from the Microsoft Corporation, click on [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409).
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
