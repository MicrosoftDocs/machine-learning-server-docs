--- 
 
# required metadata 
title: "as.glm function (RevoScaleR) " 
description: " Converts objects containing generalized linear model results to a glm object. " 
keywords: "(RevoScaleR), as.glm, as.glm.rxLogit, as.glm.rxGlm" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 
 
 #as.glm: Conversion of a RevoScaleR rxLogit or rxGlm object to a glm Object 
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
 
This function converts an existing object of class [rxLogit](rxLogit.md) or 
[rxGlm](rxGLM.md) to an object of class glm.
The underlying structure of the output object will be a subset of that produced by an equivalent call to
glm. In many cases, this method can be used to coerce an object
for use with the **pmml** package. **RevoScaleR** model objects that contain
`transforms` or a `transformFunc` are not supported.
 
 
 
 ##Value
 
an object of class lm.
 
 

 
 
 
 ##See Also
 
[rxLogit](rxLogit.md),
[rxGlm](rxGLM.md),
glm,
[as.lm](as.lm.md),
[as.kmeans](as.kmeans.md),
[as.rpart](as.rpart.md),
[as.xtabs](as.xtabs.md).
   
 
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
 
