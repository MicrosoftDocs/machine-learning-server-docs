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
# Predict income using binary classification with microsoftml

**Applies to: Microsoft Learning Server 9.2**

## Objective

Learn how to use binary classification with using the functions in the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md) that ships with Machine Learning Server.
 Data scientists work [locally in their preferred Python IDE](../install/python-libraries-interpreter.md) and favorite version control tools to build scripts and models.  


This example attempts to predict if someone will exceed a given income threshold using a linear model build with the [rx_fast_linear](../python-reference/microsoftml/rx_fast_linear.md) function from the [microsoftml package](../python-reference/microsoftml/microsoftml-package.md). 


## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

+ An instance of [Anaconda and the microsoftml package installed](../install/python-libraries-interpreter.md) on your local machine.  This package requires a connection to Machine Learning Server.   

+ An instance of [Machine Learning Server ](../what-is-machine-learning-server.md) installed with the Python option.

## Example code

The example for this quickstart is stored in a Jupyter notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.



This quickstart notebook walks you through how to:
+ Load the example data
+ Import the microsoftml package
+ Add transforms for featurization
+ Train and evaluate the model 
+ Predict using the model on new data

You can try it yourself with the notebook. 

### &#9658; [**Download the Jupyter notebook to try it out**](https://notebooks.azure.com/jmartens/libraries/pyservice17).



## Next steps


## See also

This section provides a quick summary of useful links for data scientists looking to perform machine learning tasks with Machine Learning Server.

**Key Documents**

**Support Channel**
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)


Find more information in the table of contents.
