--- 

# required metadata 
title: "MicrosoftML package for R (Microsoft Machine Learning Server and SQL Server Machine Learning Server) " 
description: "Function help reference for the MicrosoftML R package of Microsoft R." 
keywords: "MicrosoftML-package, package" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "cgronlun" 
ms.date: "02/16/2018" 
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
#ms.technology: "" 
#ms.custom: "" 

--- 

# MicrosoftML package

The **MicrosoftML** library provides state-of-the-art fast, scalable machine learning algorithms and transforms for R. The package is used with the [**RevoScaleR** package](../revoscaler/revoscaler.md).

| Package details | |
|--------|-|
| Current version: |  9.4.0 |
| Built on: | R 3.5.2 |
| Package distribution: | [Machine Learning Server 9.x](../../what-is-machine-learning-server.md) </br>[Microsoft R Client (Windows and Linux)](../../r-client/what-is-microsoft-r-client.md) <br/>[Microsoft R Server 9.1](../../what-is-microsoft-r-server.md)   <br/>[SQL Server 2016 and later (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) |


## How to use MicrosoftML for R

The **MicrosoftML** module is installed as part of Microsoft Machine Learning Server or SQL Server Machine Learning Server when you add R to your installation. It is also installed with the pre-trained machine learning models. You can use any R IDE to write R script calling functions in **MicrosoftML**, but the script must run on a computer having our interpreters and libraries.

Use this library with [RevoScaleR](../revoscaler/revoscaler.md) data sources.

## Functions by category

This section lists the functions by category to give you an idea of how each one is used. You can also use the table of contents to find functions in alphabetical order.

<a name="ml-algorithms"></a>

## 1-Machine learning algorithms

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

## 2-Transformation functions

| Function name | Description |
|---------------|-------------|
|[concat](concat.md) | Transformation to create a single vector-valued column from multiple columns.  |
|[categorical](categorical.md) | Create indicator vector using categorical transform with dictionary.  |
|[categoricalHash](categoricalhash.md) | Converts the categorical value into an indicator array by hashing. |
|[featurizeText](featurizetext.md) | Produces a bag of counts of sequences of consecutive words, called n-grams, from a given corpus of text. It offers language detection, tokenization, stopwords removing, text normalization and feature generation.  |
|[getSentiment](getsentiment.md) | Scores natural language text and creates a column that contains probabilities that the sentiments in the text are positive.|
|[ngram](ngram.md) | allows defining arguments for count-based and hash-based feature extraction.|
|[selectColumns](selectcolumns.md) | Selects a set of columns to retrain, dropping all others. |
|[selectFeatures](selectfeatures.md) | Selects features from the specified variables using a specified mode.|
|[loadImage](loadimage.md) | Loads image data.|
|[resizeImage](resizeimage.md) | Resizes an image to a specified dimension using a specified resizing method.|
|[extractPixels](extractpixels.md) | Extracts the pixel values from an image.|
|[featurizeImage](featurizeimage.md) | Featurizes an image using a pre-trained deep neural network model.|


## 3-Scoring and training functions

| Function name | Description |
|---------------|-------------|
|[rxPredict.mlModel](rxpredict.md) | Runs the scoring library either from SQL Server, using the stored procedure, or from R code enabling real-time scoring to provide much faster prediction performance.|
|[rxFeaturize](rxfeaturize.md) | Transforms data from an input data set to an output data set.|
|[mlModel](mlmodel.md) | Provides a summary of a Microsoft R Machine Learning model.|


## 4-Loss functions for classification and regression

| Function name | Description |
|---------------|-------------|
|[expLoss](loss.md) | Specifications for exponential classification loss function. | 
|[logLoss](loss.md) | Specifications for log classification loss function.  |
|[hingeLoss](loss.md) | Specifications for hinge classification loss function. | 
|[smoothHingeLoss](loss.md) | Specifications for smooth hinge classification loss function.  |
| [poissonLoss](loss.md) | Specifications for poisson regression loss function. | 
|[squaredLoss](loss.md) | Specifications for squared regression loss function.   |   

## 5-Feature selection functions

| Function name | Description |
|---------------|-------------|
|[minCount](mincount.md) | Specification for feature selection in count mode. |
|[mutualInformation](mutualinformation.md) | Specification for feature selection in mutual information mode. |

## 6-Ensemble modeling functions

| Function name | Description |
|---------------|-------------|
|[fastTrees](fasttrees.md) | Creates a list containing the function name and arguments to train a Fast Tree model with [rxEnsemble](rxensemble.md).|
|[fastForest](rxfastforest.md) | Creates a list containing the function name and arguments to train a Fast Forest model with [rxEnsemble](rxensemble.md).|
|[fastLinear](fastlinear.md) | Creates a list containing the function name and arguments to train a Fast Linear model with [rxEnsemble](rxensemble.md).|
|[logisticRegression](logisticregression.md) | Creates a list containing the function name and arguments to train a  Logistic Regression model with [rxEnsemble](rxensemble.md).|
|[oneClassSvm](oneclasssvm.md) | Creates a list containing the function name and arguments to train a OneClassSvm model with [rxEnsemble](rxensemble.md).|

## 7-Neural networking functions

| Function name | Description |
|---------------|-------------|
|[optimizer](optimizer.md) | Specifies optimization algorithms for the [rxNeuralNet](rxneuralnet.md) machine learning algorithm.|


## 8-Package state functions

| Function name | Description |
|---------------|-------------|
|[rxHashEnv](rxHashEnv.md) | An environment object used to store package-wide state. |

## Next steps

Add R packages to your computer by running setup for Machine Learning Server or R Client: 

+ [R Client](../../r-client/what-is-microsoft-r-client.md) 
+ [Machine Learning Server](../../what-is-machine-learning-server.md)

Next, refer to these introductory articles and samples to get started:

+ [Cheatsheet: Choosing an algorithm](../../r/how-to-choose-microsoftml-algorithms-cheatsheet.md)  
+ [MicrosoftML samples](../../r/sample-microsoftml.md)

## See also

 [R Package Reference](../introducing-r-server-r-package-reference.md) 





















