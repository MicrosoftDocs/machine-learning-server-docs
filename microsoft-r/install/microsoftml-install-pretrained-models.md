---

# required metadata
title: "How to install and deploy pretrained models with MicrosoftML"
keywords: ""
author: "bradsev"
ms.author: "bradsev"
manager: "jhubbard"
ms.date: "05/05/2017"
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

+ Sentiment analysis scores raw unstructured text in positive-negative terms, returning a score between 0 and 1.

+ Image detection identifies features of the image, returning

Pre-trained models are available for both R and Python development, through the [MicrosoftML R package](../r-reference/microsoftml/microsoftml-package.md) and the [microsoftml Python package](../python-reference/microsoftml/microsoftml-package.md). 

## Get the pretrained models

Pre-trained models are installed through Setup as an optional component of the **Machine Learning Server** or **SQL Server Machine Learning**. You can also get them through a local tools installation of the Python client libraries or through **Microsoft R Client**. 

For samples showing how to use pretrained models for sentiment analysis and image featurization, see [Samples for MicrosoftML](../r/sample-microsoftml.md).

## Next steps

Install the models by running the setup program or installation script for the platform you are using. 

**See also:**

+ [Install Machine Learning Server](r-server-install.md)
+ [featurize_image (microsoftml Python)](../python-reference/microsoftml/featurize-image.md)
+ [featurize_text (microsoftml Python)](../python-reference/microsoftml/featurize-text.md)
+ [featurizeImage (MicrosoftML R)](../r-reference/microsoftml/featurizeimage.md)
+ [featurizeText (MicrosoftML R)](../r-reference/microsoftml/featurizetext.md)
+ [Install R Client on Windows](../r-client/install-on-windows.md)
+ [Install R Client on Linux](../r-client/install-on-linux.md)
+ [Install Python Client Libraries](python-libraries-interpreter.md)