--- 
 
# required metadata 
title: "MicrosoftML package for R (Microsoft R Server) | Microsoft Docs" 
description: "Function help reference for the MicrosoftML R package of Microsoft R." 
keywords: "MicrosoftML-package, package" 
author: "bradsev"
ms.author: "bradsev" 
manager: "jhubbard" 
ms.date: "08/22/2017" 
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

The **MicrosoftML** library provides state-of-the-art fast, scalable machine learning algorithms and transforms for R. The package is used with the [**RevoScaleR** package](../revoscaler/revoscaler.md).

| Package details | |
|--------|-|
| Version: |  1.3.0 |
| Supported on: | [Microsoft R Client (Windows and Linux)](../../r-client/what-is-microsoft-r-client.md) <br/>[Microsoft R Server (all platforms)](../../what-is-microsoft-r-server.md)   <br/>[SQL Server 2016 and later (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) |
| Built on: | R 3.3.x (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|

## How to use MicrosoftML for R

The **MicrosoftML** module is installed as part of Microsoft R when you add R to your installation. It is also installed with the pretrained machine learning models. You can use any R IDE to write R script calling functions in **MicrosoftML**, but the script must run on a computer having our interpreters and libraries.

<a name="ml-algorithms"></a>

## Machine learning algorithms

| Function name | Description |
|---------------|-------------|
|[rxFastTrees](rxfasttrees.md) | An implementation of FastRank, an efficient implementation  of the MART gradient boosting algorithm.  |
|[rxFastForest](rxfastforest.md) | A random forest and Quantile regression forest  implementation using [rxFastTrees](rxfasttrees.md).  |
|[rxLogisticRegression](logisticregression.md) | Logistic regression using L-BFGS.  |
|[rxOneClassSvm](rxoneclasssvm.md) | One class support vector machines.  
|[rxNeuralNet](rxneuralnet.md) | Binary, multi-class, and regression neural net.  |
|[rxFastLinear](rxfastlinear.md) | Stochastic dual coordinate ascent optimization for linear binary classification and regression. |
|[rxEnsemble](rxensemble.md) | Trains a number of models of various kinds to obtain better predictive performance than could be obtained from a single model.|


<a name="ml-transforms"></a>

## Transformation functions

| Function name | Description |
|---------------|-------------|
|[concat](concat.md) | Transformation to create a single vector-valued column from multiple columns.  |
|[categorical](categorical.md) | Create indicator vector using categorical transform with dictionary.  |
|[categoricalHash](categoricalhash.md) | Converts the categorical value into an indicator array by hashing. |
|[featurizeText](featurizetext.md) | Produces a bag of counts of sequences of consecutive words, called n-grams, from a given corpus of text. It offers language detection, tokenization, stopwords removing, text normalization and feature generation.  |
|[getSentiment](getsentiment.md) | Scores natural language text and creates a column that contains probabilities that the sentiments in the text are positive.|
|[ngram](ngram.md) | allows defining arguments for count-based and hash-based feature extraction.|
|[selectFeatures](selectfeatures.md) | Selects features from the specified variables using a specified mode.|
|[loadImage](loadimage.md) | Loads image data.|
|[resizeImage](resizeimage.md) | Resizes an image to a specified dimension using a specified resizing method.|
|[extractPixels](extractpixels.md) | Extracts the pixel values from an image.|
|[featurizeImage](featurizeimage.md) | Featurizes an image using a pre-trained deep neural network model.|


## Scoring and training functions

| Function name | Description |
|---------------|-------------|
|[rxPredict.mlModel](rxpredict.md) | Runs the scoring library either from SQL Server, using the stored procedure, or from R code enabling real-time scoring to provide much faster prediction performance.|
|[rxFeaturize](rxfeaturize.md) | Transforms data from an input data set to an output data set.|
|[mlModel](mlmodel.md) | Provides a summary of a Microsoft R Machine Learning model.|


## Loss functions for classification and regression

| Function name | Description |
|---------------|-------------|
|[expLoss](loss.md) | Specifications for exponential classification loss function. | 
|[logLoss](loss.md) | Specifications for log classification loss function.  |
|[hingeLoss](loss.md) | Specifications for hinge classification loss function. | 
|[smoothHingeLoss](loss.md) | Specifications for smooth hinge classification loss function.  |
| [poissonLoss](loss.md) | Specifications for poisson regression loss function. | 
|[squaredLoss](loss.md) | Specifications for squared regression loss function.   |   

## Feature selection functions

| Function name | Description |
|---------------|-------------|
|[minCount](mincount.md) | Specification for feature selection in count mode. |
|[mutualInformation](mutualinformation.md) | Specification for feature selection in mutual information mode. |

## Ensemble modeling functions

| Function name | Description |
|---------------|-------------|
|[fastTrees](fasttrees.md) | Creates a list containing the function name and arguments to train a Fast Tree model with [rxEnsemble](rxensemble.md).|
|[fastForest](rxfastforest.md) | Creates a list containing the function name and arguments to train a Fast Forest model with [rxEnsemble](rxensemble.md).|
|[fastLinear](fastlinear.md) | Creates a list containing the function name and arguments to train a Fast Linear model with [rxEnsemble](rxensemble.md).|
|[logisticRegression](logisticregression.md) | Creates a list containing the function name and arguments to train a  Logistic Regression model with [rxEnsemble](rxensemble.md).|
|[oneClassSvm](oneclasssvm.md) | Creates a list containing the function name and arguments to train a OneClassSvm model with [rxEnsemble](rxensemble.md).|
 
## Neural networking functions

| Function name | Description |
|---------------|-------------|
|[optimizer](optimizer.md) | Specifies optimization algorithms for the [rxNeuralNet](rxneuralnet.md) machine learning algorithm.|


## Next steps

Add R packages to your computer by running setup for R Server or R Client: 

+ [R Client](../../r-client/what-is-microsoft-r-client.md) 
+ [R Server](../../what-is-microsoft-r-server.md)

Next, refer to these introductory articles and samples to get started:

+ [Cheatsheet: Choosing an algorithm](../../r/how-to-choose-microsoftml-algorithms-cheatsheet.m\d)  
+ [MicrosoftML samples](../../r/sample-microsoftml)

## See also

 [R Package Reference](../introducing-r-server-r-package-reference.md) 

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
