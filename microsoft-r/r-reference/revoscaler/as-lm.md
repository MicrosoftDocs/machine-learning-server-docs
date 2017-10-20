--- 
 
# required metadata 
title: "Conversion of a RevoScaleR rxLinMod object to an lm Object" 
description: " Converts objects containing linear model results to an lm object. " 
keywords: "RevoScaleR, as.lm, as.lm.rxLinMod, category, models" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 
 
 #as.lm: Conversion of a RevoScaleR rxLinMod object to an lm Object

 
 
 ##Description
 
Converts objects containing linear model results to an lm object.
 
 
 ##Usage

```   
 ## S3 method for class `rxLinMod':
as.lm  (x, ...)
 
```
 
 ##Arguments

   
    
 ### x
 object of class rxLinMod. 
  
    
 ###  ...
 additional arguments (currently not used). 
  
 
 
 
 ##Details
 
This function converts an existing object of class rxLinMod an object of
class lm.
The underlying structure of the output object will be a subset of that produced by an equivalent call to
lm. In many cases, this method can be used to coerce an object
for use with the **pmml** package.  **RevoScaleR** model objects that contain
`transforms` or a `transformFunc` are not supported.
 
 
 
 ##Value
 
an object of class lm.
 
 

 
 
 
 ##See Also
 
[rxLinMod](rxlinmod.md),
lm,
[as.glm](as.glm.md),
[as.kmeans](as.kmeans.md),
[as.rpart](as.rpart.md),
[as.xtabs](as.xtabs.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
# If the pmml package is installed 
library(pmml)
rxObj <- rxLinMod(Sepal.Length ~ Sepal.Width + Petal.Length, data=iris)
pmml(as.lm(rxObj))
 ## End(Not run) 
  
 
```
 
 
 
 
