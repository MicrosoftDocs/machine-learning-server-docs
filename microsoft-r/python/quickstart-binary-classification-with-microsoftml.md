---

# required metadata
title: "Quickstart for binary classification with microsoftml - Machine Learning Server | Microsoft Docs"
description: "How to deploy an R model as a service"
keywords: "quickstart, Machine Learning Server, deploy python models"
author: "bradsev"
ms.author: "bradsev"
manager: "jhubbard"
ms.date: "9/25/2017"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: "r"
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""

---
# An example of binary classification with the microsoftml Python package

**Applies to: Microsoft Learning Server 9.2**

## Objective

Learn how to use binary classification using the functions in the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md) that ships with Machine Learning Server.
 Data scientists work [locally in their preferred Python IDE](../install/python-libraries-interpreter.md) and favorite version control tools to build scripts and models.  

This example uses the well known breast cancer dataset. The dataset contains characteristics of cell nuclei and has a target label to indicate whether the tumor was benign (0) or malignant (1). The example builds a linear model with the [rx_fast_linear](../python-reference/microsoftml/rx_fast_linear.md) function from the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md). 


## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have an instance of [Machine Learning Server ](../what-is-machine-learning-server.md) installed with the Python option.

## Example code

The example for this quickstart is stored in a Jupyter notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.



This quickstart notebook walks you through how to:
+ Load the example data
+ Import the microsoftml package
+ Add transforms for featurization
+ Train and evaluate the model 
+ Predict using the model on new data

You can try it yourself with the notebook. 

### &#9658; [**Download the Jupyter notebook to try it out**]().



## Next steps


## See also

This section provides a quick summary of useful links for data scientists looking to perform machine learning tasks with Machine Learning Server.

**Key Documents**

**Support Channel**
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)


Find more information in the table of contents.
