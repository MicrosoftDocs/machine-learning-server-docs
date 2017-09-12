---

# required metadata
title: "How to get started with MicrosoftML"
keywords: ""
author: "bradsev"
ms.author: "bradsev"
manager: "cgronlun"
ms.date: "09/22/2017"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

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

# What is the MicrosoftML package?

**MicrosoftML** for **Microsoft Machine Learning Server** and **SQL Server Machine Learning Services** adds state-of-the-art data transforms, machine learning algorithms, and pretrained models to Microsoft R and Python functionality.  

The **data transforms** provided by MicrosoftML allow you to compose a custom set of transforms in a pipeline that are applied to your data before training or testing. The primary purpose of these transforms is to allow you to format your data. 

The MicrosoftML packages provides fast and scalable **machine learning algorithms** that enable you to tackle common machine learning tasks such as classification, regression, and anomaly detection. These high-performance algorithms are multi-threaded, some of which execute off disk, so that they can scale up to 100s of GBs on a single-node. They are especially suitable for handling a large corpus of text data or high-dimensional categorical data. It enables you to run these functions locally on Windows or Linux machines or on Azure HDInsight (Hadoop/Spark) clusters.

**Pretrained models** for sentiment analysis and image featurization can also be installed and deployed with MicrosoftML. For more information on the pretrained models and samples, see [R samples for MicrosoftML](sample-microsoftml.md) and [Python samples for MicrosoftML](../python/samples-microsoftml-python.md).


<a name="platform-availability"></a>
## Installation and platform availability

MicrosoftML is installed as part of **Microsoft Machine Learning Server 9.2.1**, **Microsoft R Client**, and in the **SQL Server Machine Learning Services** on various platforms. For information on the supported platforms, see [Supported Platforms for Microsoft R Server](../install/r-server-install-supported-platforms.md).

To install pretrained machine learning models for sentiment analysis and image featurization, you must opt in during setup. For details, see [Pre-trained machine learning models](../install/microsoftml-install-pretrained-models.md).


## Match algorithms to machine learning tasks

Matching data transforms and machine learning algorithms to appropriate data science tasks is key designing successful intelligent applications.

### Machine learning tasks

The **MicrosoftML** package implements algorithms that can perform a variety of machine learning tasks:

- **binary classification**: algorithms that learn to predict which of two classes an instance of data belongs to. These provide supervised learning in which the input of a classification algorithm is a set of labeled examples. Each example is represented as a feature vector, and each label is an integer of value of 0 or 1. The output of a binary classification algorithm is a classifier, which can be used to predict the label of new unlabeled instances.
- **multi-class classification**: algorithms that learn to predict the category of an instance of data. These provide supervised learning in which the input of a classification algorithm is a set of labeled examples. Each example is represented as a feature vector, and each label is an integer between 0 and k-1, where k is the number of classes. The output of a classification algorithm is a classifier, which can be used to predict the label of a new unlabeled instance.
- **regression**: algorithms that learn to predict the value of a dependent variable from a set of related independent variables. Regression algorithms model this relationship to determine how the typical values of dependent variables change as the values of the independent variables are varied. These provide supervised learning in which the input of a regression algorithm is a set of examples with dependent variables of known values. The output of a regression algorithm is a function, which can be used to predict the value of a new data instance whose dependent variables are not known.
- **anomaly detection**: algorithms that identify outliers that do not belong to some target class or conform to an expected pattern. One-class anomaly detection is a type of unsupervised learning as the input data only contains data that is from the target class and does not contain instances of anomalies to learn from.

### Machine learning algorithms

The following table summarizes the MicrosoftML algorithms, the tasks they support, their scalability, and lists some example applications.

Algorithm (R/Python) | ML task supported | Scalability | Application Examples
--------- | ----------------- | ------------ | -----------
**`rxFastLiner()`/<br>`rx_fast-linear()`** <br>Fast Linear model <br>(SDCA) |  binary classification, linear regression | #cols: ~1B;<br> #rows: ~1B;<br> CPU: multi-proc | Mortgage default prediction, Email spam filtering
**`rxOneClassSvm()`/<br>`rx_oneclass-svm()`** <br>OneClass SVM | anomaly detection | cols: ~1K;<br> #rows: RAM-bound;<br> CPU: single-proc | Credit card fraud detection
**`rxFastTrees()`/<br>`rx_fast-trees()`** <br>Fast Tree | binary classification, regression | #cols: ~50K;<br> #rows: RAM-bound;<br> CPU: multi-proc | Bankruptcy prediction
**`rxFastForest()`/<br>`rx_fast-forest()`** <br>Fast Forest | binary classification, regression | #cols: ~50K;<br> #rows: RAM-bound;<br> CPU: multi-proc | Churn Prediction
**`rxNeuralNet()`/<br>`rx_neural_network()`** <br>Neural Network | binary and multiclass classification, regression | #cols: ~10M;<br> #rows: Inf;<br> CPU: multi-proc CUDA GPU | Check signature recognition, OCR, Click Prediction
**`rxLogisticRegression()`/<br>`rx_logistic-regression()`** <br>Logistic regression | binary and multiclass classification |#cols: ~100M; <br>#rows: Inf for single-proc CPU<br> #rows: RAM-bound for multi-proc CPU| Classifying sentiments from feedback

### Data transforms

**MicrosoftML** also provides transforms to help tailor your data for machine learning. They are used to clean, wrangle, train, and score your data. For a description of the transforms, see [Machine learning R transforms](~/r-reference/microsoftml/microsoftml-package.md#ml-transforms) and [Machine learning Python transforms](~/python-reference/microsoftml/microsoftml-package.md#ml-transforms) reference documentation.


## What's new?
For the new features included in latest release of MicrosoftML, see [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md).


## What's next?

For reference documentation on the R individual transforms and functions in the product help, see [MicrosoftML: machine learning algorithms](../r-reference/microsoftml/microsoftml-package.md).

For reference documentation on the Python individual transforms and functions in the product help, see [MicrosoftML: machine learning algorithms](../python-reference/microsoftml/microsoftml-package.md).

For guidance when choosing the appropriate machine learning algorithm from the MicrosoftML package, see the [Cheat Sheet: How to choose a MicrosoftML algorithm](how-to-choose-microsoftml-algorithms-cheatsheet.md).

## See also

[About Microsoft R Server](../what-is-microsoft-r-server.md) for general information about R Server.   

[Cheat Sheet: How to choose a MicrosoftML algorithm](how-to-choose-microsoftml-algorithms-cheatsheet.md) provides guidance on how to approach the choice of an ML algorithm for your scenario.

[R samples for MicrosoftML](sample-microsoftml.md) and [Python samples for MicrosoftML](../python/samples-microsoftml-python.md) show how to use pretrained models for sentiment analysis and image featurization.