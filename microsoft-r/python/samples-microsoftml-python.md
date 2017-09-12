---

# required metadata
title: "Python samples for MicrosoftML"
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

# Python samples for MicrosoftML

MicrosoftML samples that use the Python language are described and linked here to help you get started quickly with Microsoft Machine Learning Server. The sentiment analysis and image featurization quickstarts both use pretrained models. 

Pre-trained models are installed through setup as an optional component of the **Machine Learning Server** or **SQL Server Machine Learning**. To install them, you must check the **ML Models** checkbox on the **Configure the installation** page. For details, see [How to install and deploy pretrained machine learning models with MicrosoftML](../install/microsoftml-install-pretrained-models.md).

## Sentiment analysis

The [Sentiment analysis](link TBD) sample is a text analytics sample that shows how to use the [`featurize_text`](../python-reference/microsoftml/featurize-text.md) transform to featurize text data. The featurized text data is then used to train a model to predict if a sentence expresses positive or negative sentiments. The type of machine learning task exhibited in this sample is a supervised binary classification problem.

More specifically, the example provided shows how to use the [`featurize_text`](../python-reference/microsoftml/featurize-text.md) transform in the MicrosoftML package to produce a bag of counts of n-grams (sequences of consecutive words) from the text for classification. The sample uses the [Sentiment Labelled Sentences](https://archive.ics.uci.edu/ml/datasets/Sentiment+Labelled+Sentences) dataset from the UCI repository, which contains sentences that are labeled as positive or negative sentiment.


## Image featurization

Image featurization is the process that takes an image as input and produces a numeric vector (aka feature vector) that represents key characteristics (features) of that image. The features are extracted with an [`featurize_image`](../python-reference/microsoftml/featurize-image.md) transform that runs the image data through one of several available pretrained Deep Neural Net (DNN) models. Two samples are provided in the [MicrosoftML GitHub repo](link TBD) that show how to use these DNN models.

- **Sample 1: Find similar images**: Here is the scenario this sample addresses: You have a catalog of images in a repository. When you get a new image, you want to find the image from your catalog that most closely matches this new image.
- **Sample 2: Train a model to classify images**: Here is the scenario this sample addresses: train a model to classify or recognize the type of an image using labeled observations from a training set provided. Specifically, this sample trains a multiclass linear model using the [`rx_logistic_regression`](../python-reference/microsoftml/rx-logistic-regression.md) algorithm to distinguish between fish, helicopter and fighter jet images. The multiclass training task uses the feature vectors of the images from the training set to learn how to classify these images.

For some additional discussion on MicrosoftML support of pre-trained deep neural network models for image featurization, see [Image featurization with a pre-trained deep neural network model](https://blogs.msdn.microsoft.com/rserver/2017/04/12/image-featurization-with-a-pre-trained-deep-neural-network-model/).

##
More samples here - TBD.