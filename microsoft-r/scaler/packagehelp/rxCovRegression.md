--- 
 
# required metadata 
title: "Covariance and Correlation Matrices for Linear Model Coefficients and Explanatory Variables" 
description: " Obtain covariance and correlation matrices for the coefficient estimates within `rxLinMod`,  `rxLogit`, and `rxGlm` objects and explanatory variables within `rxLinMod` and `rxLogit` objects. " 
keywords: "RevoScaleR, rxCovCoef, rxCovCoef.rxLinMod, rxCovCoef.rxLogit, rxCovCoef.rxGlm, rxCorCoef, rxCorCoef.rxLinMod, rxCorCoef.rxLogit, rxCorCoef.rxGlm, rxCovData, rxCovData.rxLinMod, rxCovData.rxLogit, rxCorData, rxCorData.rxLinMod, rxCorData.rxLogit, models, regression" 
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
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 #`rxCovCoef`: Covariance and Correlation Matrices for Linear Model Coefficients and Explanatory Variables

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
Obtain covariance and correlation matrices for the coefficient estimates within `rxLinMod`, 
`rxLogit`, and `rxGlm` objects and
explanatory variables within `rxLinMod` and `rxLogit` objects.
 
 
 ##Usage

```   
  rxCovCoef(x)
  rxCorCoef(x)
  rxCovData(x)
  rxCorData(x)
 
```
 
 ##Arguments

   
    
 ### `x`
 object of class `rxLinMod`, `rxLogit`, or `rxGlm` that  satisfies conditions in the Details section. 
  
 
 
 ##Details
 
For `rxCovCoef` and `rxCorCoef`, the rxLinMod, rxLogit, or rxGlm object must
have been fit with `covCoef = TRUE` and `cube = FALSE`. The degrees
of freedom must be greater than 0.
 
For `rxCovData` and `rxCorData`, the rxLinMod or rxLogit object must
have been fit with an intercept term as well as with `covData = TRUE` and
`cube = FALSE`.
 
 
 ##Value
 
If `p` is the number of columns in the model matrix, then

For `rxCovCoef` a `p x p` numeric matrix containing the
covariances of the model coefficients.

For `rxCorCoef` a `p x p` numeric matrix containing the
correlations amongst the model coefficients.

For `rxCovData` a `(p - 1) x (p - 1)`
numeric matrix containing the covariances of the non-intercept terms in the
model matrix.

For `rxCorData` a `(p - 1) x (p - 1)`
numeric matrix containing the correlations amongst the non-intercept terms in
the model matrix.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxLinMod](rxLinMod.md),
[rxLogit](rxLogit.md),
[rxCovCor](rxCovCor.md).
   
 ##Examples

 ```
   
  ## Example 1
  # Get the covariance matrix of the estimated model coefficients
  kyphXdfFileName <- file.path(rxGetOption("sampleDataDir"), "Kyphosis.xdf")
  kyphLogitWithCovCoef <-
    rxLogit(Kyphosis ~ Age + Number + Start, data = kyphXdfFileName,
            covCoef = TRUE, reportProgress = 0)
  rxCovCoef(kyphLogitWithCovCoef)
  
  # Compare results with results from stats::glm function
  data(kyphosis, package = "rpart")
  kyphGlmSummary <-
    summary(glm(Kyphosis ~ Age + Number + Start, data = kyphosis,
                family = binomial()))
  kyphGlmSummary[["cov.scaled"]]
  
  
  ## Example 2
  # Get the covariance matrix of the data
  kyphXdfFileName <- file.path(rxGetOption("sampleDataDir"), "Kyphosis.xdf")
  kyphLogitWithCovData <-
    rxLogit(Kyphosis ~ Age + Number + Start, data = kyphXdfFileName,
            covData = TRUE, reportProgress = 0)
  rxCovData(kyphLogitWithCovData)
  
  # Compare results with stats::cov function
  cov(kyphosis[2:4])
  
  
  ## Example 3
  # Find the correlation matrices for both the coefficient estimates and the
  # explanatory variables
  rxCorCoef(kyphLogitWithCovCoef)
  rxCorData(kyphLogitWithCovData)
 
```
 
 
 
