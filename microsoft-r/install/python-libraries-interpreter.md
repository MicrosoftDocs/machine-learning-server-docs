---

# required metadata
title: "How to install Python support in Machine Learning Server | Microsoft Docs"
description: "Installing python interpreter and libraries locally to interact with Machine Learning Server"
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

# How to install custom Python libraries and interpreter locally

Machine Learning Server provides a number of custom Python libraries for training, transformations, text and image analysis, modeling, deploying, and more. The libraries installed include  [revoscalepy, microsoftml, azureml-model-management-sdk](../python-reference/introducing-python-package-reference.md), and one library for the pretrained models. 

You can also work locally with these libraries by installing a Python interpreter and the custom libraries on your machine. Then, from your local machine, you can:

+ Benefit from the best-of-breed machine learning algorithms without any server connection. These algorithms have been battle-tested by Microsoft and are now at your fingertips.
 
+ Push large dataset computations to Machine Learning Server using the compute context functions in the revoscalepy library. By pushing computations onto the server, you can leverage the disk scalability, performance and speed of a production instance of Machine Learning Server on any supported platforms. 
 
This article describes how to install a Python interpreter (Anaconda) and custom libraries locally on a client computer.

## Install on Windows

1. Download the installation shell script from http://aka.ms/py-me.

1. Run the script AS AN ADMINISTRATOR to install the interpreter and packages. Python 3.5.2 and Anaconda 4.2.0 are installed along with all packages listed above.

## Install on Linux

1. Download and install Anaconda Python interpreter with Python 3.5 from https://docs.continuum.io/anaconda/faq#id3 on your local machine. 

   To work with Machine Learning Server, you must install Python 3.5.2 supported with Anaconda 4.2.0.

1. Start up Anaconda.

1. On the command line, install the custom libraries.
   ```
   cd /usr/bin/python
   # upgrade pip if necessary
   pip install -U pip
   # install custom libraries
   pip install --no-index revoscalepy
   pip install --no-index azureml-model-management
   pip install --no-index pretrained model 
   pip install --no-index mml
   ```


## Example code to test the install


### Run locally

```
GET EXAMPLE OF CODE THAT RUNS LOCALLY USING MML PACKAGE
```

### Push compute to Machine Learning Server

```
GET EXAMPLE OF CODE THAT TAKES PREVIOUS EXAMPLE AND PUSHES IT TO MLS ON LINUX
```