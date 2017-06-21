--- 
 
# required metadata 
title: "Machine Learning Image Featurization Transform" 
description: "Featurizes an image using a pre-trained deep neural network model." 
keywords: "transform image featurize dnn cnn resnet alexnet" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/21/2017" 
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

# featurize_image

microsoftml.modules.image_analytics.featurize_image(cols: [<class ‘dict’>, <class ‘str’>], dnn_model: [‘Resnet18’, ‘Resnet50’, ‘Resnet101’, ‘Alexnet’] = ‘Resnet18’, **kargs)



## Description

Featurizes an image using a pre-trained deep neural network model.


## Details

``featurize_image`` featurizes an image using the specified
pre-trained deep neural network model. The input variables to this transforms must
be extracted pixel values.


## Parameters


### var

Input variable containing extracted pixel values.


### out_var

The prefix of the output variables containing the image features.
If null, the input variable name will be used. The default value is *None*.


### dnn_model

The pre-trained deep neural network. The possible options are:

* ``"resnet18"`` 

* ``"resnet50"`` 

* ``"resnet101"`` 

* ``"alexnet"`` 

The default value is ``"resnet18"``.
See [Deep Residual Learning for Image Recognition](http://www.cv-foundation.org/openaccess/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html)
for details about ResNet.


## Returns

A ``maml`` object defining the transform.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## Example



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

