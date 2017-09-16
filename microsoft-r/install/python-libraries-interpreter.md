---

# required metadata
title: "How to install Python support in Machine Learning Server | Microsoft Docs"
description: "Installing python interpreter and packages locally to interact with Machine Learning Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "9/25/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
  - r-server
#ms.custom: ""

---

# How to install custom Python packages and interpreter locally

Machine Learning Server provides custom Python packages for training, transformations, text and image analysis, modeling, deploying, and more. The [packages installed](../python-reference/introducing-python-package-reference.md) include:
+ [revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md)
+ [microsoftml](../python-reference/microsoftml/microsoftml-package.md)
+ [azureml-model-management-sdk](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)
+ [pre-trained models](microsoftml-install-pretrained-models.md)

You can also install a Python interpreter along with these custom packages locally on your machine, and:

+ Benefit from the best-of-breed machine learning algorithms without any server connection. These algorithms have been battle-tested by Microsoft.
 
+ Push large dataset computations to Machine Learning Server using the compute context functions in the revoscalepy package. By pushing computations onto the server, you can benefit from the disk scalability, performance, and speed of a production instance of Machine Learning Server on any supported platforms. 
 
This article describes how to install a Python interpreter (Anaconda) and custom packages locally on a client computer.

## Install on Windows

1. Download the installation shell script from http://aka.ms/mls-py. This script installs Anaconda 4.2.12, which includes Python 3.5.2, along with all packages listed previously.

1. Launch a new PowerShell window with elevated administrator permissions ('as administrator').

1. Go to the folder in which you downloaded the installer and run the script:
   ```
   cd {{download-directory}}
   .\Install-PyForMLS.ps1
   ```

   Note: , use the -InstallFolder command-line argument followed by the new path. For example: 
   ```
   .\Install-PyForMLS.ps1 -InstallFolder C:\path-to-python-for-mls”)
   ```

## Install on Linux

1. Log in to your local machine. 

1. Download and install Anaconda 4.2.12, which includes Python 3.5.2, from https://repo.continuum.io/miniconda/Miniconda3-4.2.12-Linux-x86_64.sh. 

1. Upgrade pip if necessary before installing the packages themselves.

   In a terminal window, run the following command:
   ```
   pip install -U pip
   ```

1. Install the desired custom packages for Machine Learning Server.

   In the terminal window, run the following commands:
   ```
   pip install <<revoscalepy-URL>>
   pip install <<mml-URL>>
   pip install <<pretrained model-URL>>
   pip install azureml-model-management
   ```

## Example to test the install

Test your install and packages using this example code. 

In this example, you can use some functions from the [microsoftml python package](../python-reference/microsoftml/microsoftml-package.md) for logistic regression.

1. Let's build some fake data. We just need to make our data to start out. Let's create a single label and 2000 random features.

   ```Python
   from numpy.random import randn
   matrix = randn(2000, 2001)
​   
   import pandas
   data = pandas.DataFrame(data=matrix, columns=["Label"] + ["f%s" % i for i in range(1, matrix.shape[1])])
   data["Label"] = (data["Label"] > 0.5).apply(lambda x: 1.0 if x else 0.0)
​   
   print("problem dimension:", data.shape)
   print(data[["Label", "f1", "f2", data.columns[-1]]].head())
   ```
   
1. Train a logistic regression using rx_logistic_regression from the microsoftml Python package.

   ```Python
   formula = "Label ~ {0}".format(" + ".join(data.columns[1:]))
   print(formula[:50] + " + ...")
​   
   from microsoftml import rx_logistic_regression
​   
   try:
       logregml = rx_logistic_regression(formula, data=data)
   except Exception as e:
       # The error is expected because patsy cannot handle
       # so many features.
       print(e)
   ```

1. Manually define the formula. Let's skip patsy's parser to manually define the formula with object ModelDesc <http://patsy.readthedocs.io/en/latest/API-reference.html?highlight=lookupfactor#patsy.ModelDesc>_.

   ```Python
   from patsy.desc import ModelDesc, Term
   from patsy.user_util import LookupFactor
​   
   patsy_features = [Term([LookupFactor(n)]) for n in data.columns[1:]][:10]
   model_formula = ModelDesc([Term([LookupFactor("Label")])], [Term([])] + patsy_features)
​   
   print(model_formula.describe() + " + ...")
   logregml = rx_logistic_regression(model_formula, data=data)
   ```

1. Examine the results. They should resemble the following output:

    ```Python
    Label ~ f1 + f2 + f3 + f4 + f5 + f6 + f7 + f8 + f9 + f10 + ...
    Automatically adding a MinMax normalization transform, use 'norm=Warn' or 'norm=No' to turn this behavior off.
    Beginning processing data.
    Rows Read: 2000, Read Time: 0.046, Transform Time: 0
    Beginning processing data.
    Beginning processing data.
    Rows Read: 2000, Read Time: 0.051, Transform Time: 0
    Beginning processing data.
    Beginning processing data.
    Rows Read: 2000, Read Time: 0.058, Transform Time: 0
    Beginning processing data.
    Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
    LBFGS multi-threading will attempt to load dataset into memory. In case of out-of-memory issues, turn off multi-threading by setting trainThreads to 1.
    Beginning optimization
    num vars: 11
    improvement criterion: Mean Improvement
    L1 regularization selected 11 of 11 weights.
    Not training a calibrator because it is not needed.
    ``` 