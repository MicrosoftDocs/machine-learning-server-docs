---

# required metadata
title: "Split data into multiple files using rxSplit (Machine Learning Server) "
description: "Split data to train and test a model, or subdivide a large dataset into smaller files."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "12/19/2017"
ms.topic: "article"
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

# How to split data for model training and testing

This article explains how to split a dataset in two for training and testing a model, but the same technique applies for any use case where subdividing data is required. For example, to manually partition data across multiple nodes in a cluster, you could use the functions and workflow described in this article for that purpose.

## Functions for splitting data

R developers, use [rxSplit (RevoScaleR)](./r-reference/revoscaler/rxsplitxdf.md). The following example is from [Run R code in R Client and Machine Learning Server](quickstart-run-r-code.md) quickstart.

```R
#Randomly split data (80% for training, 20% for testing).
   rxSplit(inData = outFileFinal,
           outFilesBase = paste0(td, "/modelData"),
           outFileSuffixes = c("Train", "Test"),
           splitByFactor = "splitVar",
           overwrite = TRUE,
           transforms = list(
             splitVar = factor(sample(c("Train", "Test"),
                                       size = .rxNumRows,
                                       replace = TRUE,
                                       prob = c(.80, .20)),
                                levels = c("Train", "Test"))),
            rngSeed = 17,
            consoleOutput = TRUE)
```

Python developers, we recommend [sci-kit](http://scikit-learn.org/stable/) methods for splitting data, as demonstrated in the [Example of binary classification with the microsoftml Python package](./python/quickstart-binary-classification/with-microsoftml.md) quickstart.

```python
from sklearn.model_selection import train_test_split

bc_train, bc_test = train_test_split(bc_df, test_size=0.2)

print("# of rows in training set = ",bc_train.size)
print("# of rows in test set = ",bc_test.size)
```

## See also

 [Machine Learning Server](../what-is-machine-learning-server.md) 
 [Install Machine Learning Server on Windows](../install/machine-learning-server-windows-install.md)  
 [Install Machine Learning Server on Linux](../install/machine-learning-server-linux-install.md)  
 [Install Machine Learning Server on Hadoop](../install/machine-learning-server-hadoop-install.md)
