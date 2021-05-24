---

# required metadata
title: "Quickstart: Binary classification with microsoftml - Machine Learning Server "
description: "How to deploy an R model as a service"
keywords: quickstart, Machine Learning Server, deploy python models
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 02/16/2018
ms.topic: "quickstart"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: r
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.custom: ""
---

# Quickstart: An example of binary classification with the microsoftml Python package

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

**Applies to: Machine Learning Server 9.x**

Learn how to use binary classification using the functions in the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md) that ships with Machine Learning Server.
 Data scientists work [locally in their preferred Python IDE](../install/python-libraries-interpreter.md) and favorite version control tools to build scripts and models.  

This example uses the well known breast cancer dataset. The dataset contains characteristics of cell nuclei and has a target label to indicate whether the tumor was benign (0) or malignant (1). The example builds a linear model with the [rx_fast_linear](../python-reference/microsoftml/rx-fast-linear.md) function from the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md). 


## Time estimate

After you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

> [!div class="checklist"]
> * An instance of [Machine Learning Server ](../what-is-machine-learning-server.md) installed with the Python option.<br/>&nbsp;
> * Familiarity with Python. [Here's a video tutorial](https://mva.microsoft.com/en-us/training-courses/introduction-to-programming-with-python-8360?l=lqhuMxFz_8904984382).<br/>&nbsp;
> * Familiarity with Jupyter Notebooks. Learn [how to load sample Python notebooks](how-to-revoscalepy-jupyter-nb-config.md). 


## Example code

The example for this quickstart is stored in a Jupyter Notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.



This quickstart notebook walks you through how to:
1. Load the example data

1. Import the microsoftml package

1. Add transforms for featurization

1. Train and evaluate the model 

1. Predict using the model on new data

You can try it yourself with the notebook. 

### &#9658; [**Download the Jupyter Notebook to try it out**](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/microsoftml/quickstarts/binary-classification/Binary%2BClassification%2BQuickstart.ipynb).



## Next steps

Now that you've tried this example, you can start developing your own solutions using the MicrosoftML packages and APIs for R and Python:

- [MicrosoftML R package functions](../r-reference/microsoftml/microsoftml-package.md)
- [microsoftml Python package functions](../python-reference/microsoftml/microsoftml-package.md)


## See also

For more about Machine Learning Server in general, see [Overview of Machine Learning Server](../what-is-machine-learning-server.md) 


For more machine learning samples using the microsoftml Python package, see [Python samples for MicrosoftML](samples-microsoftml-python.md).
