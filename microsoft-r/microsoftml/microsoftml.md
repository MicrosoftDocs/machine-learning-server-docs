---

# required metadata
title: "MicrosoftML functions"
description: "MicrosoftML functions"
keywords: "MicrosoftML"
author: "bradsev"
manager: "jhubbard"
ms.date: "12/09/2016"
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

# MicrosoftML functions

The **MicrosoftML** package provides state of the art, fast, scalable machine learning algorithms and transforms. These functions enable you to tackle common machine learning and data science tasks such as featurization, classification, regression and ranking. The goal is to help developers, data scientists, and an increasing spectrum of information workers to the design and implement intelligent products, services and devices. This topic discusses these tasks and lists the key R functions provided by this package for transforming and modeling data that facilitate the completion of these data science tasks.

## Data transforms

The transform pipelines of **MicrosoftML** allow you to compose a custom set of transforms that are applied to your data before training or testing. The primary purpose of these transforms is to allow you to featurize your data. One advantage of the transform pipelines is that once you've defined a transform pipeline, you can save the pipeline and apply it to additional data.

- **Concatenate**: creates a single vector-valued column from multiple  columns. The concatenation  can significantly speed up the processing of data when the number of columns is as large as hundreds to thousands.
- **Categorical Hash**: converts a categorical value into an indicator array using hashing. Useful when the number of categories is large or highly variable.
- **Categorical**: converts a categorical value into an indicator array using a dictionary. Useful when the number of categories is smaller or fixed.
- **Select Features**: selects features from the specified variables using one of the two modes: count or mutual information.
- **Featurize Text**: produces a bag of counts of n-grams (sequences of consecutive words) from a given text. It offers language detection, tokenization, stopwords removing, text normalization, feature generation, and term weighting using TF, IDF and TF-IDF. It supports the following languages by default: English, French, German, Dutch, Italian, Spanish and Japanese.

## Machine learning algorithms

### Machine learning tasks

The **MicrosoftML** package implements algorithms that perform a variety of machine learning tasks:

- **binary classification**: algorithms that learn to predict which of two classes an instance of data belongs to. This is supervised learning in which the input of a classification algorithm is a set of labeled examples. Each example is represented as a feature vector, and each label is an integer of value of 0 or 1. The output of a the binary classification algorithms is a classifier, which can be used to predict the label of new unlabeled instances.
- **multi-class classification**: algorithms that learn to predict the category of an instance of data. This is supervised learning in which the input of a classification algorithm is a set of labeled examples. Each example is represented as a feature vector, and each label is an integer between 0 and k-1, where k is the number of classes. The output of a classification algorithm is a classifier, which can be used to predict the label of a new unlabeled instance.
- **regression**: algorithms that learn to predict the value of a dependent variable from a set of related independent variables. Regression algorithms model this relationship to determine how the typical values of dependent variables change as the values of the independent variables are varied. This is supervised learning in which the input of a regression algorithm is a set of examples with dependent variables of known values. The output of a regression algorithm is a function, which can be used to predict the value of a new data instance whose dependent variable are not known.
- **anomaly detection**: algorithms that identify outliers that do not belong to some target class or conform to an expected pattern. One-class anomaly detection is a type of unsupervised learning as the input data only contains data that is from the target class and does not contain instances of anomalies to learn from.


### Fast Linear model (SDCA)
The **`rxFastLinear()`** algorithm is based on the Stochastic Dual Coordinate Ascent (SDCA) method, a state-of-the-art optimization technique for convex objective functions. The algorithm can be scaled for use on large out-of-memory data sets due to a semi-asynchronized implementation that supports multithreaded processing. Several choices of loss functions are also provided and elastic net regularization is supported. The SDCA method combines several of the best properties and capabilities of logistic regression and SVM algorithms.

**Tasks supported**: binary classification, linear regression


### OneClass SVM
The **`rxOneClassSvm()`** algorithm is used for one-class anomaly detection. This is a type of unsupervised learning as its training set contains only examples from the target class and not any anomalous instances. It infers what properties are normal for the objects in the target class and from these properties predicts which examples are unlike these normal examples. This is useful as typically there are very few examples of network intrusion, fraud, or other types of anomalous behavior in training data sets.

**Tasks supported**: anomaly detection

### Fast Tree
The **`rxFastTrees()`** algorithm is a high performing, state of the art scalable boosted decision tree that implements FastRank, an efficient implementation of the MART gradient boosting algorithm. MART learns an ensemble of regression trees, which is a decision tree with scalar values in its leaves. For binary classification, the output is converted to a probability by using some form of calibration.

**Tasks supported**: binary classification, regression


### Fast Forest
The **`rxFastForest()`** algorithm is a random forest that provides a learning method for classification that constructs an ensemble of decision trees at training time, outputting the class that is the mode of the classes of the individual trees. Random decision forests can correct for the overfitting to training data sets to which decision trees are prone.

**Tasks supported**: binary classification, regression


### Neural Network
The **`rxNeuralNet()`** algorithm supports a user-defined multilayer network topology with GPU acceleration. A neural network is a class of prediction models inspired by the human brain. It can be represented as a weighted directed graph. Each node in the graph is called a neuron. The neural network algorithm tries to learn the optimal weights on the edges based on the training data. Any class of statistical models can be considered a neural network if they use adaptive weights and can approximate non-linear functions of their inputs. Neural network regression is especially suited to problems where a more traditional regression model cannot fit a solution.

**Tasks supported**: binary and multiclass classification, regression

### Logistic regression
The **`rxLogisticRegression()`** algorithm is used to predict the value of a categorical dependent variable from its relationship to one or more independent variables assumed to have a logistic distribution. If the dependent variable has only two possible values (success/failure), then the logistic regression is binary. If the dependent variable has more than two possible values (blood type given diagnostic test results), then the logistic regression is multinomial.

**Tasks supported**: binary and multiclass classification

##Get help on MicrosoftML functions

To see the **MicrosoftML** functions that can be called:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
1. In the console, open the package help by typing the following at the R prompt: `help(package="MicrosoftML")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.

## See also

[Introduction to MicrosoftML](../microsoftml-introduction.md)

For guidance when choosing the appropriate machine learning algorithm from the MicrosoftML package, see the [MicrosoftML algorithm cheat sheet](../microsoftml-algorithm-cheat-sheet.md).
