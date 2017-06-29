--- 
 
# required metadata 
title: "Machine Learning Resize Image Transform" 
description: "Resizes an image to a specified dimension using a specified" 
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

## resize_image


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.modules.image_analytics.resize_image(cols: [<class ‘str’>, <class ‘dict’>, <class ‘list’>], width=224, height=224, resizing_option: [‘IsoPad’, ‘IsoCrop’, ‘Aniso’] = ‘IsoCrop’, **kargs)
```




### Description

Resizes an image to a specified dimension using a specified
resizing method.


### Details

``resize_image`` resizes an image to the specified height and width
using a specified resizing method. The input variables to this transforms must
be images, typically the result of the ``load_image`` transform.


### Arguments


##### cols

A named list of character vectors of input variable names and
the name of the output variable. Note that the input variables must
be of the same type. For one-to-one mappings between input and output
variables, a named character vector can be used.


##### width

Specifies the width of the scaled image in pixels. The default value is 224.


##### height

Specifies the height of the scaled image in pixels. The default value is 224.


##### resizing_option

Specified the resizing method to use. Note that all methods
are using bilinear interpolation. The options are:

* ``"IsoPad"``: The image is resized such that the aspect ratio is preserved. If needed, the image is padded with black to fit the new width or height. 

* ``"IsoCrop"``: The image is resized such that the aspect ratio is preserved. If needed, the image is cropped to fit the new width or height. 

* ``"Aniso"``: The image is stretched to the new width and height, without preserving the aspect ratio. 

The default value is ``"IsoPad"``.


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

