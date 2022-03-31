--- 

# required metadata 
title: "as.naiveBayes function (revoAnalytics) | Microsoft Docs" 
description: " Converts RevoScaleR rxNaiveBayes objects to a (limited) e1071 naiveBayes object. " 
keywords: "(revoAnalytics), as.naiveBayes, as.naiveBayes.rxNaiveBayes, category, models" 
author: "chuckheinzelman"
ms.author: "charlhe" 
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



 # as.naiveBayes: Conversion of a RevoScaleR rxNaiveBayes object to a e1071 naiveBayes object 
 ## Description

Converts RevoScaleR rxNaiveBayes objects to a (limited) e1071 naiveBayes object.


 ## Usage

```   
 ## S3 method for class `rxNaiveBayes':
as.naiveBayes  (x, ...)

```

 ## Arguments



 ### `x`
 object of class rxNaiveBayes. 


 ### ` ...`
 additional arguments (currently not used). 




 ## Details

This function converts an existing object of class `rxNaiveBayes` to an object of
class `naiveBayes` in the **e1071** package.
The underlying structure of the output object will be a subset of that produced by an equivalent call to
`naiveBayes`. **RevoScaleR** model objects that contain
`transforms` or a `transformFunc` are not supported.



 ## Value

an object of class naiveBayes from **e1071**.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[rxNaiveBayes](rxNaiveBayes.md),
[as.lm](as.lm.md),
[as.kmeans](as.kmeans.md),
[as.glm](as.glm.md),
[as.gbm](as.gbm.md),
[as.xtabs](as.xtabs.md).


 ## Examples

 ```

  ## Not run:

# If the e1071 package is installed 
library(e1071)
rxBayes1 <- rxNaiveBayes(Kyphosis ~ Start + Age + Number + Start, data = kyphosis)
as.naiveBayes(rxBayes1)
 ## End(Not run) 
```




