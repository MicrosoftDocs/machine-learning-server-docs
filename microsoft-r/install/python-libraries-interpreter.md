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

1. Run the script AS AN ADMINISTRATOR to install the interpreter and packages. Anaconda 4.2.0, which includes Python 3.5.2, is installed along with all packages listed above.

## Install on Linux

1. Log in to your local machine. 

1. Download and install Anaconda 4.2.12 from https://repo.continuum.io/miniconda/Miniconda3-4.2.12-Linux-x86_64.sh. 

   **To work with Machine Learning Server, you must install Anaconda 4.2.12, which includes Python 3.5.2.**

1. Upgrade pip if necessary before installing the packages themselves.

   In a terminal window, run the following command:
   ```
   pip install -U pip
   ```

1. Install the desired custom packages for Machine Learning Server.

   In the terminal window, run the following commands:
   ```
   pip install --no-index azureml-model-management
   pip install --no-index mml
   pip install --no-index pretrained model 
   pip install --no-index revoscalepy
   ```
## Example code to test the install

Test your install and packages using this example code.

### Use custom package functions locally

```
GET EXAMPLE OF CODE THAT RUNS LOCALLY USING MML PACKAGE
```

### Push your compute to Machine Learning Server

Run your script locally, but push the computation to Machine Learning Server for faster performance.

```
GET EXAMPLE OF CODE THAT TAKES PREVIOUS EXAMPLE AND PUSHES IT TO MLS ON LINUX
```