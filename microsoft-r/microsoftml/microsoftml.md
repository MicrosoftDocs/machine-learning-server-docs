--- 
 
# required metadata 
title: " MicrosoftML: State-of-the-Art Machine Learning R Algorithms from Microsoft Corporation " 
description: " A package that provides state-of-the-art machine learning algorithms for R, developed  by Microsoft. It is used with the **RevoScaleR** package.   " 
keywords: ", MicrosoftML-package, package" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "03/20/2017" 
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
 
 
 
# MicrosoftML: State-of-the-Art Machine Learning R Algorithms from Microsoft Corporation  

##Description
 
The MicrosoftML-package package provides state-of-the-art fast, scalable machine learning algorithms and transforms for R. The package is used with the **RevoScaleR** package.

This topic includes links to the reference documentation for the ML algorithms and transforms, and for the scoring and helper functions.


 


| Item | Data |
| :---| :--- |
|  Package  |  MicrosoftML |
|  Type  |  Package |
|  Version  |  1.0.0 |
|  License  |  file LICENSE |
|  LazyLoad  |  yes |


##Key algorithms and transforms in the package

### Machine learning algorithms

* [rxFastTrees](packagehelp/rxFastTrees.md): An implementation of FastRank, an efficient implementation  of the MART gradient boosting algorithm.  
* [rxFastForest](packagehelp/rxFastForest.md): A random forest and Quantile regression forest  implementation using [rxFastTrees](packagehelp/rxFastTrees.md).  
* [rxLogisticRegression](packagehelp/LogisticRegression.md): Logistic regression using L-BFGS.  
* [rxOneClassSvm](packagehelp/OneClassSvm.md): One class support vector machines.  
* [rxNeuralNet](packagehelp/NeuralNet.md): Binary, multi-class, and regression neural net.  
* [rxFastLinear](packagehelp/rxFastLinear.md): Stochastic dual coordinate ascent optimization for linear binary classification and regression.  

### Machine learning transforms

* [featurizeText](packagehelp/featurizeText.md): Transformation to produce a bag of counts of ngrams in a given text.  It offers language detection, tokenization, stopwords removing, text normalization and feature generation.  
* [concat](packagehelp/concat.md): Transformation to create a single vector-valued column from multiple columns.  
* [categorical](packagehelp/categorical.md): Create indicator vector using categorical transform with dictionary.  
* [categoricalHash](packagehelp/categoricalHash.md): Converts the categorical value into an indicator array by hashing.  
* [selectFeatures](packagehelp/selectFeatures.md): Selects features from the specified variables. 

### Scoring

* [rxPredict.mlModel](packagehelp/rxPredict.md): Scores using a model created by one of the machine learning algorithms.  


### Helper functions for arguments

* [expLoss](packagehelp/loss.md): Specifications for exponential classification loss function.  
* [logLoss](packagehelp/loss.md): Specifications for log classification loss function.  
* [hingeLoss](packagehelp/loss.md): Specifications for hinge classification loss function.  
* [smoothHingeLoss](packagehelp/loss.md): Specifications for smooth hinge classification loss function.  
* [poissonLoss](packagehelp/loss.md): Specifications for poisson regression loss function.  
* [squaredLoss](packagehelp/loss.md): Specifications for squared regression loss function.  
* [linearKernel](packagehelp/Kernel.md): Specification for linear kernel.  
* [rbfKernel](packagehelp/Kernel.md): Specification for radial basis function kernel.  
* [polynomialKernel](packagehelp/Kernel.md): Specification for polynomial kernel.  
* [sigmoidKernel](packagehelp/Kernel.md): Specification for sigmoid kernel.  
* [minCount](packagehelp/minCount.md): Specification for feature selection in count mode. 
* [mutualInformation](packagehelp/mutualInformation.md): Specification for feature selection in mutual information mode. 
 
## Get help on MicrosoftML functions from the R console

To see the **MicrosoftML** functions that can be called from the R console:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
1. In the console, open the package help by typing the following at the R prompt: `help(package="MicrosoftML")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.
 
> [!NOTE]
> To list all public functions, type library(help="MicrosoftML") at the R prompt.
>
 
## What's next?

[Introduction to Microsoft R](../microsoft-r-getting-started.md)

[Diving into data analysis in Microsoft R](../data-analysis-in-microsoft-r.md)

[Overview of MicrosoftML functions](../overview-microsoftml-functions.md)

[MicrosoftML algorithm cheat sheet](../microsoftml-algorithm-cheat-sheet.md)


##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
