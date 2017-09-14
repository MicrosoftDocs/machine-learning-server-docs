---

# required metadata
title: "How to install and deploy pre-trained models with MicrosoftML"
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

# Pre-trained machine learning models for sentiment analysis and image detection

For sentiment analysis of text and image classification, Machine Learning Server offers two approaches for training the models: you can train the models yourself using your data, or install pre-trained models that come with training data obtained and developed by Microsoft. The advantage of pre-trained models is that you can score and classify new content right away. 

+ Sentiment analysis scores raw unstructured text in positive-negative terms, returning a score between 0 (negative) and 1 (positive), indicating relative sentiment.

+ Image detection identifies features of the image. There are several use cases for this model: image recognition, image classification. For image recognition, the model returns n-grams that possibly describe the image. For image classification, the model evaluates images and returns a classification based on possible classes you provided (for example, is the image a fish or a dog).

Pre-trained models are available for both R and Python development, through the [MicrosoftML R package](../r-reference/microsoftml/microsoftml-package.md) and the [microsoftml Python package](../python-reference/microsoftml/microsoftml-package.md). 

## Benefits of using pre-trained models

Pre-trained models have been made available to support customers who need to perform tasks such as sentiment analysis or image featurization, but do not have the resources to obtain the large datasets or train a complex model. Using pre-trained models lets you get started on text and image processing most efficiently.

Currently the models that are available are deep neural network (DNN) models for sentiment analysis and image classification. All four pre-trained models were trained on CNTK. The configuration of each network was based on the following reference implementations:

+ Resnet-18
+ Resnet-50
+ ResNet-101
+ AlexNet

For more information about deep residual networks and their implementation using CNTK, go the [Microsoft Research](https://www.microsoft.com/research/) web site and search for these articles:

+ Microsoft Researchers’ Algorithm Sets ImageNet Challenge Milestone
+ Microsoft Computational Network Toolkit offers most efficient distributed deep learning computational performance

## How to install the models

Pre-trained models are installed through setup as an optional component of Machine Learning Server or [SQL Server Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/r/install-pretrained-models-sql-server). You can also get the R version of the models through [Microsoft R Client](../r-client/what-is-microsoft-r-client.md).

1. Run a Machine Learning Server setup program for your target platform: [Install Machine Learning Server](r-server-install.md).

2. When specifying components to install, add at least one language (R Server or Python) and the pre-trained models. Language support is required. The models cannot be installed as a standalone component.

3. After setup completes, verify the models are on your computer. Pre-trained models are local, added to the MicrosoftML and microsftml library, respectively, when you run setup. The files are \mxlibs\<modelname>_updated.model for Python and \mxlibs\x64\<modelname>_updated.model for R.

For samples demonstrating use of the pre-trained models, see [Samples for MicrosoftML](../r/sample-microsoftml.md).

## Next steps

Install the models by running the setup program or installation script for the platform you are using. 

**See also:**

+ [Install Machine Learning Server](r-server-install.md)
+ [Install R Client on Windows](../r-client/install-on-windows.md)
+ [Install R Client on Linux](../r-client/install-on-linux.md)
+ [Install Python Client Libraries](python-libraries-interpreter.md)
+ [featurize_image (microsoftml Python)](../python-reference/microsoftml/featurize-image.md)
+ [featurize_text (microsoftml Python)](../python-reference/microsoftml/featurize-text.md)
+ [featurizeImage (MicrosoftML R)](../r-reference/microsoftml/featurizeimage.md)
+ [featurizeText (MicrosoftML R)](../r-reference/microsoftml/featurizetext.md)
