--- 
 
# required metadata 
title: "Conversion of a RevoScaleR Cross Tabulation Object to an xtabs Object" 
description: " Converts objects containing cross tabulation results to an xtabs object. " 
keywords: "RevoScaleR, as.xtabs, as.xtabs.rxCrossTabs, as.xtabs.rxCube, category, models" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 
 
 #`as.xtabs`: Conversion of a RevoScaleR Cross Tabulation Object to an xtabs Object

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
Converts objects containing cross tabulation results to an xtabs object.
 
 
 ##Usage

```   
 ## S3 method for class `rxCrossTabs':
as.xtabs  (x, ...)
 ## S3 method for class `rxCube':
as.xtabs  (x, ...)
 
```
 
 ##Arguments

   
    
 ### `x`
 object of class rxCrossTabs or rxCube. 
  
    
 ### ` ...`
 additional arguments (currently not used). 
  
 
 
 
 ##Details
 
This function converts an existing object of class rxCrossTabs or rxCube into an xtabs object.
The underlying structure of the output object will be the same as that produced by an equivalent call to
xtabs. However, you should expect the `"call"` attribute to be different, 
since it is a copy of the original call stored in the rxCrossTabs or rxCube input object.
In many cases, this method can be used to coerce an object
for use with the **pmml** package. **RevoScaleR** model objects that contain
`transforms` or a `transformFunc` are not supported.
 
 
 
 ##Value
 
an object of class xtabs.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxCrossTabs](rxCrossTabs.md),
[rxCube](rxCube.md),
xtabs,
[as.lm](as.lm.md),
[as.kmeans](as.kmeans.md),
[as.rpart](as.rpart.md),
[as.glm](as.glm.md).
   
 
 ##Examples

 ```
   
  # Define function to compare xtabs and as.xtabs output
  "as.xtabs.check" <- function(convertedXtabsObject, xtabsObject)
  {
      attr(convertedXtabsObject, "call") <- attr(xtabsObject, "call") <- NULL
      all.equal(convertedXtabsObject, xtabsObject)
  }
  
  # Create a data set
  set.seed(100)
  divs <- letters[1:5]
  glads <- c("spartacus", "crixus")
  romeDF <- data.frame( division = rep(divs, 5L), 
                        score = runif(25, min = 0, max = 10), 
                        rank = runif(25, min = 1, max = 100), 
                        gladiator = c(rep(glads[1L], 12L), rep(glads[2L], 13L)),
                        arena = sample(c("colosseum", "ludus", "market"), 25L, replace = TRUE))
  
  # Compare xtabs and as.xtabs(rxCrossTabs(..., returnXtabs = FALSE))  
  # results for a 3-way interaction with no dependent variables
  z1 <- rxCrossTabs(~ division : gladiator : arena, data = romeDF, returnXtabs = FALSE)
  z2 <- xtabs(~ division + gladiator + arena, romeDF)
  as.xtabs.check(as.xtabs(z1), z2) # TRUE
  
  # Compare xtabs and as.xtabs(rxCube(...)) results for a 3-way interaction
  # with no dependent variable
  z1 <- rxCube(~ division : gladiator : arena, data = romeDF, means = FALSE)
  z2 <- xtabs(~ division + gladiator + arena, romeDF)
  as.xtabs.check(as.xtabs(z1), z2) # TRUE
 
```
 
 
 
 
