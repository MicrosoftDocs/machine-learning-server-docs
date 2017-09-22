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

**Applies to: Microsoft Learning Server 9.2.1**

## Objective

Learn how to use binary classification using the functions in the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md) that ships with Machine Learning Server.
 Data scientists work [locally in their preferred Python IDE](../install/python-libraries-interpreter.md) and favorite version control tools to build scripts and models.  

This example uses the well known breast cancer dataset. The dataset contains characteristics of cell nuclei and has a target label to indicate whether the tumor was benign (0) or malignant (1). The example builds a linear model with the [rx_fast_linear](../python-reference/microsoftml/rx-fast-linear.md) function from the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md). 


## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have an instance of [Machine Learning Server ](../what-is-machine-learning-server.md) installed with the Python option. 

Familiarity with Python is assumed. [Here's a video tutorial on Python programming](https://mva.microsoft.com/en-us/training-courses/introduction-to-programming-with-python-8360?l=lqhuMxFz_8904984382).

## Example code

The example for this quickstart is stored in a Jupyter notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.



This quickstart notebook walks you through how to:
+ Load the example data
+ Import the microsoftml package
+ Add transforms for featurization
+ Train and evaluate the model 
+ Predict using the model on new data

You can try it yourself with the notebook. 

### &#9658; [**Download the Jupyter notebook to try it out**](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/quickstarts/binary-classification/Binary%2BClassification%2BQuickstart.ipynb).



## Next steps

Now that you've tried this example, you can start developing your own solutions using the MicrosoftML packages and APIs for R and Python:

- [`MicrosoftML` R package functions](../r-reference/microsoftml/microsoftml-package.md)
- [`MicrosoftML` Python package functions](../python-reference/microsoftml/microsoftml-package.md)


## See also

For more about Machine Learning Sever in general, see [Overview of Machine Learning Server](../what-is-machine-learning-server.md) 


For more machine learning samples using the microsoftml Python package, see [Python samples for MicrosoftML](samples-microsoftml-python.md).
