---

# required metadata
title: "Python samples for MicrosoftML - Machine Learning Server | Microsoft Docs"
description: "MicrosoftML samples using the Python language are described and linked here to help you get started quickly with Microsoft Machine Learning Server."
keywords: ""
author: "bradsev"
ms.author: "bradsev"
manager: "cgronlun"
ms.date: "09/25/2017"
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

# Python samples for MicrosoftML

MicrosoftML samples that use the Python language are described and linked here to help you get started quickly with Microsoft Machine Learning Server. We provide samples for tasks that are quick to run.  We also provide some more advanced samples that are described in sections following the simpler task-based samples. 

## Samples for tasks

|Example|Description                                                     |
|--------------|---------------------------------------------------------|
|[plot_binary_classification.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_binary_classification.py)|Find a frontier to separate two clouds of data points. |
|[plot_categorical_features.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_categorical_features.py)|Convert categorical data to numerical data to classify it.  |
|[plot_formula.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_formula.py)|Modeling limitations when feature set is too large.  |
|[plot_image_featurizer_classify.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_image_featurizer_classify.py)|Use image featurization for image classification. |
|[plot_image_featurizer_match.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_image_featurizer_match.py)|Use image featurization for finding similar images.  |
|[plot_iris.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_iris.py)|How to do multi-class classification on the Iris Dataset. |
|[plot_loss_function.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_loss_function.py)|How loss function parameters effect model errors when training a linear classifier.   |
|[plot_mistakes.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_mistakes.py)|Why input schemas for training and testing must be the same. |
|[plot_mutualinformation.py](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/101/plot_mutualinformation.py)|How to select and merge features before training.  |
|[plot_regression_wines.py](plot_regression_wines.py)|Use a regression to predict wine quality.  |

## Sentiment analysis with a pre-trained model

The [Sentiment analysis](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/202/plot_sentiment_analysis.py) sample is a text analytics sample that shows how to use the [`featurize_text`](../python-reference/microsoftml/featurize-text.md) transform to featurize text data. The featurized text data is then used to train a model to predict if a sentence expresses positive or negative sentiments. The type of machine learning task exhibited in this sample is a supervised binary classification problem.

More specifically, the example provided shows how to use the [`featurize_text`](../python-reference/microsoftml/featurize-text.md) transform in the MicrosoftML package to produce a bag of counts of n-grams (sequences of consecutive words) from the text for classification. The sample uses the [Sentiment Labelled Sentences Data Set](https://archive.ics.uci.edu/ml/datasets/Sentiment+Labelled+Sentences) from the UCI repository, which contains sentences that are labeled as positive or negative sentiment.

The sentiment analysis quickstart uses a pre-trained model. Pre-trained models are installed through setup as an optional component of the **Machine Learning Server** or **SQL Server Machine Learning**. To install them, you must check the **ML Models** checkbox on the **Configure the installation** page. For more information on pre-trained models, see [Pre-trained machine learning models](../install/microsoftml-install-pretrained-models.md).


## Optimal hyperparameters to recognize epileptic seizures

The [Grid Search](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/202/plot_grid_search.py) sample analyzes the [Epileptic Seizure Recognition Data Set](https://archive.ics.uci.edu/ml/datasets/Epileptic+Seizure+Recognition) to determine whether epileptic seizures have occurred. The sample uses logistic regression on regrouped data for the binary classification to make predictions.

The sample then shows how to improve the prediction by choosing optimal hyperparameters for the learner. All learners employ [hyperparameters](https://en.wikipedia.org/wiki/Hyperparameter_(machine_learning)) which impact the way a model is trained. Most of the time, they have a default value which works on most of the datasets. But the default values are not usually the best possible value for a particular dataset. This sample show how to find optimal values for this dataset.


## Sentiment analysis with text featurization

Text featurization is a machine learning technique that converts text into numerical values that are used to capture features of interest. The [Plot text featurization](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/202/plot_sentiment_analysis.py) sample is a text analytics example that creates columns *features* containing n-grams probabilities for positive and negative sentiments computed from their sentences. The featurized text data is then used to train a model to predict if a sentence expresses positive or negative sentiments. We repeat the training and prediction with  n-grams of different lengths and compare their speed and performance with an ROC curve. We also show how to use a combination of two sets of features and plot the results. For more information on text featurization, see [text featurization](http://blog.revolutionanalytics.com/2017/08/text-featurization-microsoftml.html).


## Next steps
Now that you've tried some of these examples, you can start developing your own solutions using the MicrosoftML packages and APIs for R and Python:

- [`MicrosoftML` R package functions](../r-reference/microsoftml/microsoftml-package.md)
- [`MicrosoftML` Python package functions](../python-reference/microsoftml/microsoftml-package.md)


