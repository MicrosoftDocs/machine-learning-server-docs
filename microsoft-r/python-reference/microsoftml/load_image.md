--- 
 
# required metadata 
title: "Machine Learning Load Image Transform" 
description: "Loads image data." 
keywords: "transform image" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "" 
ms.topic: "reference" 
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

## load_image


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.modules.image_analytics.load_image(cols: [<class ‘str’>, <class ‘dict’>, <class ‘list’>], **kargs)
```




### Description

Loads image data.


### Details

``load_image`` loads images from paths.


### Arguments


##### cols

A named list of character vectors of input variable names and
the name of the output variable. Note that the input variables must
be of the same type. For one-to-one mappings between input and output
variables, a named character vector can be used.


### Returns

A ``maml`` object defining the transform.


### Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


### Example



```
train <- data.frame(Path = c(system.file("help/figures/RevolutionAnalyticslogo.png", package = "MicrosoftML")), Label = c(TRUE), stringsAsFactors = FALSE)

# Loads the images from variable Path, resizes the images to 1x1 pixels and trains a neural net.
model <- rxNeuralNet(
    Label ~ Features,
    data = train,
    mlTransforms = list(
        loadImage(vars = list(Features = "Path")),
        resizeImage(vars = "Features", width = 1, height = 1, resizing = "Aniso"),
        extractPixels(vars = "Features")
        ),
    mlTransformVars = "Path",
    numHiddenNodes = 1,
    numIterations = 1)

# Featurizes the images from variable Path using the default model, and trains a linear model on the result.
model <- rxFastLinear(
    Label ~ Features,
    data = train,
    mlTransforms = list(
        loadImage(vars = list(Features = "Path")),
        resizeImage(vars = "Features", width = 224, height = 224), # If dnnModel == "AlexNet", the image has to be resized to 227x227.
        extractPixels(vars = "Features"),
        featurizeImage(var = "Features")
        ),
    mlTransformVars = "Path")
```

