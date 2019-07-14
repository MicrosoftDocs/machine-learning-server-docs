--- 

# required metadata 
title: "extractPixels function (MicrosoftML) " 
description: " Extracts the pixel values from an image. " 
keywords: "(MicrosoftML), extractPixels, image, transform" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 




 # extractPixels: Machine Learning Extract Pixel Data Transform 
 ## Description

Extracts the pixel values from an image.


 ## Usage

```   
  extractPixels(vars, useAlpha = FALSE, useRed = TRUE, useGreen = TRUE,
    useBlue = TRUE, interleaveARGB = FALSE, convert = TRUE, offset = NULL,
    scale = NULL)

```

 ## Arguments



 ### `vars`
 A named list of character vectors of input variable names and the name of the output variable. Note that the input variables must be of the same type. For one-to-one mappings between input and output variables, a named character vector can be used. 



 ### `useAlpha`
 Specifies whether to use alpha channel. The default value is `FALSE`. 



 ### `useRed`
 Specifies whether to use red channel. The default value is `TRUE`. 



 ### `useGreen`
 Specifies whether to use green channel. The default value is `TRUE`. 



 ### `useBlue`
 Specifies whether to use blue channel. The default value is `TRUE`. 



 ### `interleaveARGB`
 Whether to separate each channel or interleave in ARGB order. This might be important, for example, if you are training a convolutional neural network, since this would affect the shape of the kernel, stride etc. 



 ### `convert`
 Whether to convert to floating point. The default value is `FALSE`. 



 ### `offset`
 Specifies the offset (pre-scale). This requires `convert = TRUE`.  The default value is `NULL`. 



 ### `scale`
 Specifies the scale factor. This requires `convert = TRUE`.  The default value is `NULL`. 



 ## Details

`extractPixels` extracts the pixel values from an image. The input variables
 are images of the same size, typically the output of a `resizeImage` transform. The
 output is pixel data in vector form that are typically used as features for a learner.


 ## Value

A `maml` object defining the transform.

 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## Examples

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




