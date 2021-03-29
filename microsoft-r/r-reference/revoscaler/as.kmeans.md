--- 

# required metadata 
title: "as.kmeans function (revoAnalytics) | Microsoft Docs" 
description: " Converts objects containing RevoScaleR-computed k-means clusters to an R kmeans object. " 
keywords: "(revoAnalytics), as.kmeans, as.kmeans.rxKmeans, category, models" 
author: "dphansen"
ms.author: "davidph" 
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



 # as.kmeans: Conversion of a RevoScaleR rxKmeans object to a kmeans Object 
 ## Description

Converts objects containing RevoScaleR-computed k-means clusters to an R kmeans object.


 ## Usage

```   
 ## S3 method for class `rxKmeans':
as.kmeans  (x, ...)

```

 ## Arguments



 ### `x`
 object of class `"rxKmeans"`. 


 ### ` ...`
 additional arguments (currently not used). 




 ## Details

This function converts an existing object of class `"rxKmeans"` an object of
class `"kmeans"`.
The underlying structure of the output object will be a subset of that produced by an equivalent call to
`kmeans`. Often, this method can be used to coerce an object
for use with the **pmml** package. **RevoScaleR** model objects that contain
`transforms` or a `transformFunc` are not supported.



 ## Value

an object of class `"kmeans"`.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[as.lm](as.lm.md),
[as.glm](as.glm.md),
[as.rpart](as.rpart.md),
[as.xtabs](as.xtabs.md),
[rxKmeans](rxKmeans.md).


 ## Examples

 ```

  ## Not run:

# If the pmml package is installed 
library(pmml)
form <- ~ height + weight
set.seed(17)
irow <- unique(sample.int(nrow(women), 4L, replace = TRUE))[seq(2)]
centers <- women[irow,, drop = FALSE]
rxKmeansObj <- rxKmeans(form, data = women, centers = centers)
pmml(as.kmeans(rxKmeansObj))
 ## End(Not run) 
```




