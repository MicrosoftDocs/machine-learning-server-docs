---

# required metadata
title: "Python samples for MicrosoftML(Machine Learning Server) | Microsoft Docs"
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

MicrosoftML samples that use the Python language are described and linked here to help you get started quickly with Microsoft Machine Learning Server. The sentiment analysis and image featurization quickstarts both use pre-trained models. 

Pre-trained models are installed through setup as an optional component of the **Machine Learning Server** or **SQL Server Machine Learning**. To install them, you must check the **ML Models** checkbox on the **Configure the installation** page. For more information on pre-trained models, see [Pre-trained machine learning models](../install/microsoftml-install-pretrained-models.md).

## Sentiment analysis

The [Sentiment analysis](link TBD) sample is a text analytics sample that shows how to use the [`featurize_text`](../python-reference/microsoftml/featurize-text.md) transform to featurize text data. The featurized text data is then used to train a model to predict if a sentence expresses positive or negative sentiments. The type of machine learning task exhibited in this sample is a supervised binary classification problem.

More specifically, the example provided shows how to use the [`featurize_text`](../python-reference/microsoftml/featurize-text.md) transform in the MicrosoftML package to produce a bag of counts of n-grams (sequences of consecutive words) from the text for classification. The sample uses the [Sentiment Labelled Sentences](https://archive.ics.uci.edu/ml/datasets/Sentiment+Labelled+Sentences) dataset from the UCI repository, which contains sentences that are labeled as positive or negative sentiment.






