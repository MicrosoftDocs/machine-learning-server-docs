---

# required metadata
title: "How to install and deploy pretrained models with MicrosoftML"
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

For sentiment analysis and image detection, Machine Learning Server offers two approaches for training the models: you can train the models yourself using your data, or install pre-trained models that come with training data obtained and developed by Microsoft. The advantage of pre-trained models is that you can score new content right away. 

+ Sentiment analysis scores raw unstructured text in positive-negative terms, returning a score between 0 (negative) and 1 (positive), indicating relative sentiment.

+ Image detection identifies features of the image. There are several use cases for this model: image recognition, image classification. For image recognition, the model returns n-grams that possibly describe the image. For image classification, the model evaluates images and returns a classification based on possible classes you provided (for example, is the image a fish or a dog).

Pre-trained models are available for both R and Python development, through the [MicrosoftML R package](../r-reference/microsoftml/microsoftml-package.md) and the [microsoftml Python package](../python-reference/microsoftml/microsoftml-package.md). 

## Get the pretrained models

Pre-trained models are installed through setup as an optional component of the **Machine Learning Server** or **SQL Server Machine Learning**. To install them, you must check the **ML Models** checkbox on the **Configure the installation** page: 

![1](./media/microsoftml-install-pretrained-models/msr-config-install-ml-model.png)

You can also get them through a local tools installation of the Python client libraries (for microsoftml) or through **Microsoft R Client** (for MicrosoftML). 

Models that include training data are local, added to the MicrosoftML and microsftml library, respectively, when you run setup. The files are \mxlibs\<modelname>_updated.model for Python and \mxlibs\x64\<modelname>_updated.model for R.

For samples showing how to use pretrained models, see [Samples for MicrosoftML](../r/sample-microsoftml.md).

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
