---

# required metadata
title: "Get started with MicrosoftML - Machine Learning Server "
description: "MicrosoftML adds state-of-the-art data transforms, machine learning algorithms, and pre-trained models to R and Python functionality. The data transforms provided by MicrosoftML allow you to compose a custom set of transforms in a pipeline that are applied to your data before training or testing. The primary purpose of these transforms is to allow you to format your data. "
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: "09/22/2017"
ms.topic: "conceptual"
ms.prod: "mlserver"

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

# What is MicrosoftML?

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

MicrosoftML adds state-of-the-art data transforms, machine learning algorithms, and pre-trained models to R and Python functionality. The *data transforms* provided by MicrosoftML allow you to compose a custom set of transforms in a pipeline that are applied to your data before training or testing. The primary purpose of these transforms is to allow you to format your data. 

The MicrosoftML functions are provided through the **MicrosoftML** package installed with [Machine Learning Server](../what-is-machine-learning-server.md), Microsoft R Client, and [SQL Server Machine Learning Services](/sql/machine-learning/sql-server-machine-learning-services).

Functions provide fast and scalable *machine learning algorithms* that enable you to tackle common machine learning tasks such as classification, regression, and anomaly detection. These high-performance algorithms are multi-threaded, some of which execute off disk, so that they can scale up to 100s of GBs on a single-node. They are especially suitable for handling a large corpus of text data or high-dimensional categorical data. It enables you to run these functions locally on Windows or Linux machines or on Azure HDInsight (Hadoop/Spark) clusters.

[Pre-trained models](../install/microsoftml-install-pretrained-models.md) for sentiment analysis and image featurization can also be installed and deployed with MicrosoftML. For more information on the pre-trained models and samples, see [R samples for MicrosoftML](sample-microsoftml.md) and [Python samples for MicrosoftML](../python/samples-microsoftml-python.md).

## Match algorithms to machine learning tasks

Matching data transforms and machine learning algorithms to appropriate data science tasks is key to designing successful intelligent applications.

## Machine learning tasks

The **MicrosoftML** package implements algorithms that can perform a variety of machine learning tasks:

- **binary classification**: algorithms that learn to predict which of two classes an instance of data belongs to. These provide supervised learning in which the input of a classification algorithm is a set of labeled examples. Each example is represented as a feature vector, and each label is an integer of value of 0 or 1. The output of a binary classification algorithm is a classifier, which can be used to predict the label of new unlabeled instances.
- **multi-class classification**: algorithms that learn to predict the category of an instance of data. These provide supervised learning in which the input of a classification algorithm is a set of labeled examples. Each example is represented as a feature vector, and each label is an integer between 0 and k-1, where k is the number of classes. The output of a classification algorithm is a classifier, which can be used to predict the label of a new unlabeled instance.
- **regression**: algorithms that learn to predict the value of a dependent variable from a set of related independent variables. Regression algorithms model this relationship to determine how the typical values of dependent variables change as the values of the independent variables are varied. These provide supervised learning in which the input of a regression algorithm is a set of examples with dependent variables of known values. The output of a regression algorithm is a function, which can be used to predict the value of a new data instance whose dependent variables are not known.
- **anomaly detection**: algorithms that identify outliers that do not belong to some target class or conform to an expected pattern. One-class anomaly detection is a type of unsupervised learning as the input data only contains data that is from the target class and does not contain instances of anomalies to learn from.

## Machine learning algorithms

The following table summarizes the MicrosoftML algorithms, the tasks they support, their scalability, and lists some example applications.

Algorithm (R/Python) | ML task supported | Scalability | Application Examples
--------- | ----------------- | ------------ | -----------
**`rxFastLiner()`/<br>`rx_fast-linear()`** <br>Fast Linear model <br>(SDCA) |  binary classification, linear regression | #cols: ~1B;<br> #rows: ~1B;<br> CPU: multi-proc | Mortgage default prediction, Email spam filtering
**`rxOneClassSvm()`/<br>`rx_oneclass-svm()`** <br>OneClass SVM | anomaly detection | cols: ~1K;<br> #rows: RAM-bound;<br> CPU: single-proc | Credit card fraud detection
**`rxFastTrees()`/<br>`rx_fast-trees()`** <br>Fast Tree | binary classification, regression | #cols: ~50K;<br> #rows: RAM-bound;<br> CPU: multi-proc | Bankruptcy prediction
**`rxFastForest()`/<br>`rx_fast-forest()`** <br>Fast Forest | binary classification, regression | #cols: ~50K;<br> #rows: RAM-bound;<br> CPU: multi-proc | Churn Prediction
**`rxNeuralNet()`/<br>`rx_neural_network()`** <br>Neural Network | binary and multiclass classification, regression | #cols: ~10M;<br> #rows: Inf;<br> CPU: multi-proc CUDA GPU | Check signature recognition, OCR, Click Prediction
**`rxLogisticRegression()`/<br>`rx_logistic-regression()`** <br>Logistic regression | binary and multiclass classification |#cols: ~100M; <br>#rows: Inf for single-proc CPU<br> #rows: RAM-bound for multi-proc CPU| Classifying sentiments from feedback

## Data transforms

**MicrosoftML** also provides transforms to help tailor your data for machine learning. They are used to clean, wrangle, train, and score your data. For a description of the transforms, see [Machine learning R transforms](/sql/machine-learning/r/ref-r-microsoftml) and [Machine learning Python transforms](/sql/machine-learning/python/reference/microsoftml/microsoftml-package#ml-transforms) reference documentation.


## Next steps

For reference documentation on the R individual transforms and functions in the product help, see [MicrosoftML: machine learning algorithms](../r-reference/microsoftml/microsoftml-package.md).

For reference documentation on the Python individual transforms and functions in the product help, see [MicrosoftML: machine learning algorithms](/sql/machine-learning/python/ref-py-microsoftml).

For guidance when choosing the appropriate machine learning algorithm from the MicrosoftML package, see the [Cheat Sheet: How to choose a MicrosoftML algorithm](how-to-choose-microsoftml-algorithms-cheatsheet.md).

## See also

[Machine Learning Server](../what-is-machine-learning-server.md)   

[R samples for MicrosoftML](sample-microsoftml.md)

[Python samples for MicrosoftML](../python/samples-microsoftml-python.md) 
