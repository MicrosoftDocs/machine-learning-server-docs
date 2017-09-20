---

# required metadata
title: "Install Python support in Machine Learning Server | Microsoft Docs"
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

# How to install custom Python packages and interpreter locally on Windows

Machine Learning Server provides custom Python packages for training, transformations, text and image analysis, modeling, deploying, and more. The [packages installed](../python-reference/introducing-python-package-reference.md) include:
+ [revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md)
+ [microsoftml](../python-reference/microsoftml/microsoftml-package.md)
+ [azureml-model-management-sdk](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)
+ [pre-trained models](microsoftml-install-pretrained-models.md)

You can install a Python interpreter along with these custom packages locally on your Windows machine, and:

+ Benefit from the best-of-breed machine learning algorithms without any server connection. These algorithms have been battle-tested by Microsoft.
 
+ Push large dataset computations to Machine Learning Server using the compute context functions in the revoscalepy package. By pushing computations onto the server, you can benefit from the disk scalability and performance of Machine Learning Server on any supported platforms. 
 
This article describes how to install a Python interpreter (Anaconda) and custom packages locally on a client Windows computer.

## Install on Windows

1. Log in to your local Windows machine.

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

On each supported OS, the package manager downloads packages from the repository, determines dependencies, retrieves additional packages, and installs the software. After installation completes, mlserver-python executable is at '/usr/bin'.
 
**On Ubuntu 14.04 - 16.04**

With root or sudo permissions, run the following commands:
```
# Set location of the package repository. For example for 16.04.
wget https://packages.microsoft.com/ubuntu/16.04/prod/microsoft-mlserver-packages-py-9.2.1.deb

# Verification step: look for the mlserver.list configuration file
ls -la /etc/apt/sources.list.d/

# Update packages on your system
apt-get update

# If your system does not have the https apt transport option
apt-get install apt-transport-https

# Install the packages
apt-get install microsoft-mlserver-packages-py-9.2.1
```     

**Red Hat and CentOS 6/7**

With root or sudo permissions, run the following commands:
```
# Set location of the package repository. For example for 7.
rpm -Uvh https://packages.microsoft.com/rhel/7/prod/microsoft-mlserver-packages-py-9.2.1.rpm

# Verification step: look for the mlserver.list configuration file
ls -la /etc/apt/sources.list.d/

# Install the packages
yum install microsoft-mlserver-packages-py-9.2.1
``` 

**SUSE Linux Enterprise Server 11**

With root or sudo permissions, run the following commands:
```
# Set location of the package repository. For example for SLES 11.
rpm -Uvh https://packages.microsoft.com/sles/11/prod/microsoft-mlserver-packages-py-9.2.1.rpm

# Verification step: look for the mlserver.list configuration file
ls -la /etc/apt/sources.list.d/

# Install the packages
zypper install microsoft-mlserver-packages-py-9.2.1
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