--- 
 
# required metadata 
title: " MicrosoftML: State-of-the-Art Machine Learning Algorithms from Microsoft Corporation " 
description: " A package that provides state-of-the-art machine learning algorithms for R, developed  by Microsoft. It is used with the **RevoScaleR** package.   " 
keywords: ", MicrosoftML-package, package" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "03/13/2017" 
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
 
 
 
 
#`MicrosoftML-package`:  MicrosoftML: State-of-the-Art Machine Learning Algorithms from Microsoft Corporation  

##Description
 
The MicrosoftML-package package provides state-of-the-art machine learning algorithms for R developed by Microsoft. It is used with the **RevoScaleR** package.
 


| Item | Data |
| :---| :--- |
|  Package:  |  MicrosoftML |
|  Type:  |  Package |
|  Version:  |  0.0.5 |
|  License:  |  file LICENSE |
|  LazyLoad:  |  yes |


##Key functions/algorithms in the package

### Machine learning algorithms

* [rxFastTrees](rxFastTrees.md): An implementation of FastRank, an efficient implementation  of the MART gradient boosting algorithm.  
* [rxFastForest](rxFastForest.md): A random forest and Quantile regression forest  implementation using [rxFastTrees](rxFastTrees.md).  
* [rxLogisticRegression](LogisticRegression.md): Logistic regression using L-BFGS.  
* [rxOneClassSvm](OneClassSvm.md): One class support vector machines.  
* [rxNeuralNet](NeuralNet.md): Binary, multi-class, and regression neural net.  
* [rxFastLinear](rxFastLinear.md): Stochastic dual coordinate ascent optimization for linear binary classification and regression.  


### Scoring

* [rxPredict.mlModel](rxPredict.md): Scores using a model created by one of the machine learning algorithms.  


### Helper functions for arguments

* [expLoss](loss.md): Specifications for exponential classification loss function.  
* [logLoss](loss.md): Specifications for log classification loss function.  
* [hingeLoss](loss.md): Specifications for hinge classification loss function.  
* [smoothHingeLoss](loss.md): Specifications for smooth hinge classification loss function.  
* [poissonLoss](loss.md): Specifications for poisson regression loss function.  
* [squaredLoss](loss.md): Specifications for squared regression loss function.  
* [linearKernel](Kernel.md): Specification for linear kernel.  
* [rbfKernel](Kernel.md): Specification for radial basis function kernel.  
* [polynomialKernel](Kernel.md): Specification for polynomial kernel.  
* [sigmoidKernel](Kernel.md): Specification for sigmoid kernel.  
* [minCount](minCount.md): Specification for feature selection in count mode. 
* [mutualInformation](mutualInformation.md): Specification for feature selection in mutual information mode. 
 

### Helper functions for machine learning transforms

* [featurizeText](featurizeText.md): Transformation to produce a bag of counts of ngrams in a given text.  It offers language detection, tokenization, stopwords removing, text normalization and feature generation.  
* [concat](concat.md): Transformation to create a single vector-valued column from multiple columns.  
* [categorical](categorical.md): Create indicator vector using categorical transform with dictionary.  
* [categoricalHash](categoricalHash.md): Converts the categorical value into an indicator array by hashing.  
* [selectFeatures](selectFeatures.md): Selects features from the specified variables.  


> [!NOTE]
> To list all public functions, type library(help="MicrosoftML") at the R prompt.
>
 
##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
