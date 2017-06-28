---

# required metadata
title: "Quickstart samples for MicrosoftML"
keywords: ""
author: "bradsev"
manager: "jhubbard"
ms.date: "06/20/2017"
ms.topic: "get-started-article"
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

# Quickstart samples for MicrosoftML

Samples are described and linked here to help you get started quickly using MicrosoftML. The sentiment analysis and image featurization quickstarts both use pretrained models. These models are not install by default with Microsoft R Server. If you want to have access to the pretrained models, you must check the **ML Models** checkbox on the **Configure the installation** page for Microsoft R Client and Microsoft R Server. For details, see [How to install and deploy pretrained machine learning models with MicrosoftML](../install/microsoftml-install-pretrained-models.md).


## Breast cancer prediction using rxFastLinear

This starter [breast cancer prediction sample](https://github.com/Microsoft/microsoft-r/tree/master/microsoft-ml/Samples/101/BinaryClassification/BreastCancerPrediction) builds a model to predict breast cancer. It uses the well-known Breast Cancer UCI dataset with [`rxFastLinear`](../r-reference/microsoftml/rxfastlinear.md)algorithm provided by the MicrosoftML package.

The type of machine learning task exhibited in this sample is a binary classification problem. Specifically, a given set of cell mass characteristics is used to predict whether the mass is benign or malignant.


## Sentiment analysis using featurizeText

The [Sentiment analysis](https://github.com/Microsoft/microsoft-r/tree/master/microsoft-ml/Samples/101/BinaryClassification/SimpleSentimentAnalysis) sample is a text analytics sample that shows how to use the [`featurizeText`](../r-reference/microsoftml/featurizetext.md) transform to featurize text data. The featurized text data is then used to train a model to predict if a sentence expresses positive or negative sentiments. The type of machine learning task exhibited in this sample is a supervised binary classification problem.

More specifically, the example provided shows how to use the [featurizeText](../r-reference/microsoftml/featurizetext.md) transform in the MicrosoftML package to produce a bag of counts of n-grams (sequences of consecutive words) from the text for classification. The sample uses the [Sentiment Labelled Sentences](https://archive.ics.uci.edu/ml/datasets/Sentiment+Labelled+Sentences) dataset from the UCI repository, which contains sentences that are labeled as positive or negative sentiment.


## Image featurization using featurizeImage

Image featurization is the process that takes an image as input and produces a numeric vector (aka feature vector) that represents key characteristics (features) of that image. The features are extracted with an [`featurizeImage`](../r-reference/microsoftml/featurizeimage.md) transform that runs the image data through one of several available pretrained Deep Neural Net (DNN) models. Two samples are provided in the [MicrosoftML GitHub repo](https://github.com/Microsoft/microsoft-r/tree/master/microsoft-ml/Samples/PreTrainedModels/ImageAnalytics/ImageFeaturizer) that show how to use these DNN models.

- **Sample 1: Find similar images**: Here is the scenario this sample addresses: You have a catalog of images in a repository. When you get a new image, you want to find the image from your catalog that most closely matches this new image.
- **Sample2: Train a model to classify images**: Here is the scenario this sample addresses: train a model to classify or recognize the type of an image using labeled observations from a training set provided. Specifically, this sample trains a multiclass linear model using the [`rxLogisticRegression`](../r-reference/microsoftml/logisticregression.md) algorithm to distinguish between fish, helicopter and fighter jet images. The multiclass training task uses the feature vectors of the images from the training set to learn how to classify these images.

For some additional discussion on MicrosoftML support of pre-trained deep neural network models for image featurization, see [Image featurization with a pre-trained deep neural network model](https://blogs.msdn.microsoft.com/rserver/2017/04/12/image-featurization-with-a-pre-trained-deep-neural-network-model/).

## Retail churn tutorial

The [Retail churn tutorial](https://github.com/Microsoft/microsoft-r/tree/master/microsoft-ml/Microsoft%20ML%20Tutorial) guides you through the steps for fitting a model that predicts retail churn. Customer churn is an expensive problem in retail. We focus on predicting which customers are likely to churn, but do not address how to prevent customers from churning.

The tutorial imports data from a retail database, creates a label identifying customers who have churned and features based on customer purchase history, fits a model using multiple learning algorithms, and then compares the performance of these fit models to select the best one. 


## Run R Server with MicrosoftML on HDInsight/Spark clusters

To create an HDInsight (Hadoop) cluster and connect it to R, see [Get started using R Server on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-r-server-get-started)

Microsoft R Server 9.1.0 supports the [sparklyr package from RStudio](https://cran.r-project.org/web/packages/sparklyr/index.html). Microsoft R Server and sparklyr can now be used in tandem within a single Spark session. For a walkthrough on how to use this package, see [Learn how to use R Server with sparklyr](tutorial-sparklyr-revoscaler.md).

For other cloud-based offerings, see [Introducing R Server on HDInsight](../install/r-server-vm-azure-hdinsight.md).