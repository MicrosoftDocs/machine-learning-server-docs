---

# required metadata
title: "How to install and deploy pretrained models with MicrosoftML"
keywords: ""
author: "bradsev"
manager: "jhubbard"
ms.date: "04/13/2017"
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

# How to install and deploy pre-trained machine learning models with MicrosoftML

Pre-trained deep neural network models for sentiment analysis and image featurization are available to use with MicrosoftML. These models require large databases and are time-consuming to train. Using pre-trained models allow users to efficiently extract relevant features from text and images.

## Installation

The **MicrosoftML package** is installed by default with Microsoft R Server various platforms. For a list  of these platforms, see the [Platform availability](microsoftml-get-started.md#platform-availability) section in the getting started topic.

But the pretrained ML models are not installed by default. To install them, you must check the **ML Models** checkbox on the **Configure the installation** page for Microsoft R Server. To see this box you must click the dropdown menu for **Microsoft R Server** in **The following components will be included** section.
is a step during this install


![MicrosoftML-install-pretrained-models](./media/deploy-pretrainted-microsoftml-models/msr-config-install-ml-model.png)

For quickstarts that show how to use pretrained models for  sentiment analysis and image featurization, see [Quickstarts for MicrosoftML](microsoftml-quickstarts.md).
