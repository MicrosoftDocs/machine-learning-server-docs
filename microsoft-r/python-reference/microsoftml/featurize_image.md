--- 
 
# required metadata 
title: "Machine Learning Image Featurization Transform" 
description: "Featurizes an image using a pre-trained deep neural network model." 
keywords: "transform, image, featurize, dnn, cnn, resnet, alexnet" 
author: "HeidiSteen" 
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

## ``featurize_image``: Convert an image into features


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.featurize_image(cols: [<class ‘dict’>, <class ‘str’>], dnn_model: [‘Resnet18’, ‘Resnet50’, ‘Resnet101’, ‘Alexnet’] = ‘Resnet18’, **kargs)
```




### Description

Featurizes an image using a pre-trained deep neural network model.


### Details

``featurize_image`` featurizes an image using the specified
pre-trained deep neural network model. The input variables to this transforms must
be extracted pixel values.


### Arguments


##### cols

Input variable containing extracted pixel values. If
``dict``, the keys represent the names of new variables to be created.


##### dnn_model

The pre-trained deep neural network. The possible options are:

* ``"Resnet18"`` 

* ``"Resnet50"`` 

* ``"Resnet101"`` 

* ``"Alexnet"`` 

The default value is ``"Resnet18"``.
See [Deep Residual Learning for Image Recognition](http://www.cv-foundation.org/openaccess/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html.md)
for details about ResNet.


##### kargs

Additional arguments sent to compute engine.


### Returns

An object defining the transform.


### See also

[``load_image``](load_image.md),
[``resize_image``](resize_image.md),
[``extract_pixels``](extract_pixels.md).


### Example



```
'''
Example with images.
'''
import numpy
import pandas
from microsoftml import rx_neural_network, rx_predict, rx_fast_linear
from microsoftml import load_image, resize_image, extract_pixels
from microsoftml.datasets.image import get_RevolutionAnalyticslogo

train = pandas.DataFrame(data=dict(Path=[get_RevolutionAnalyticslogo()], Label=[True]))

# Loads the images from variable Path, resizes the images to 1x1 pixels
# and trains a neural net.
model1 = rx_neural_network("Label ~ Features", data=train, 
            ml_transforms=[            
                    load_image(cols=dict(Features="Path")), 
                    resize_image(cols="Features", width=1, height=1, resizing="Aniso"), 
                    extract_pixels(cols="Features")], 
            ml_transform_vars=["Path"], 
            num_hidden_nodes=1, num_iterations=1)

# Featurizes the images from variable Path using the default model, and trains a linear model on the result.
# If dnnModel == "AlexNet", the image has to be resized to 227x227.
model2 = rx_fast_linear("Label ~ Features ", data=train, 
            ml_transforms=[            
                    load_image(cols=dict(Features="Path")), 
                    resize_image(cols="Features", width=224, height=224), 
                    extract_pixels(cols="Features")], 
            ml_transform_vars=["Path"], max_iterations=1)

# We predict even if it does not make too much sense on this single image.
print("\nrx_neural_network")
prediction1 = rx_predict(model1, data=train)
print(prediction1)

print("\nrx_fast_linear")
prediction2 = rx_predict(model2, data=train)
print(prediction2)
```

