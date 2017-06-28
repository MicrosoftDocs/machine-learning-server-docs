--- 
 
# required metadata 
title: "Conversion of a RevoScaleR rxLogit or rxGlm object to a glm Object" 
description: " Converts objects containing generalized linear model results to a glm object. " 
keywords: "RevoScaleR, as.glm, as.glm.rxLogit, as.glm.rxGlm" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 #`as.glm`: Conversion of a RevoScaleR rxLogit or rxGlm object to a glm Object

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Converts objects containing generalized linear model results to a glm object.
 
 
 ##Usage

```   
 ## S3 method for class `rxLogit':
as.glm  (x, ...)
 ## S3 method for class `rxGlm':
as.glm  (x, ...)
 
```
 
 ##Arguments

   
    
 ### `x`
 object of class rxLogit or rxGlm. 
  
    
 ### ` ...`
 additional arguments (currently not used). 
  
 
 
 
 ##Details
 
This function converts an existing object of class [rxLogit](../../scaler/packagehelp/rxlogit.md) or 
[rxGlm](../../scaler/packagehelp/rxglm.md) to an object of class glm.
The underlying structure of the output object will be a subset of that produced by an equivalent call to
glm. In many cases, this method can be used to coerce an object
for use with the **pmml** package. **RevoScaleR** model objects that contain
`transforms` or a `transformFunc` are not supported.
 
 
 
 ##Value
 
an object of class lm.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxLogit](../../scaler/packagehelp/rxlogit.md),
[rxGlm](../../scaler/packagehelp/rxglm.md),
glm,
[as.lm](../../scaler/packagehelp/as.lm.md),
[as.kmeans](../../scaler/packagehelp/as.kmeans.md),
[as.rpart](../../scaler/packagehelp/as.rpart.md),
[as.xtabs](../../scaler/packagehelp/as.xtabs.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
# If the pmml package is installed 
library(pmml)
form <- case ~ age + parity + spontaneous + induced
rxObj <- rxLogit(form, data=infert, reportProgress = 0)
pmml(as.glm(rxObj))
 ## End(Not run) 
  
 
```
 
