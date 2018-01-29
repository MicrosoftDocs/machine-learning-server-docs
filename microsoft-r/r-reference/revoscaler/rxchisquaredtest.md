--- 
 
# required metadata 
title: "rxChiSquaredTest function (revoAnalytics) | Microsoft Docs" 
description: " Chi-squared Test, Fisher's Exact Test, and Kendall's  Tau Rank Correlation Coefficient " 
keywords: "(revoAnalytics), rxChiSquaredTest, rxFisherTest, rxKendallCor, htest" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 
 
 
 #rxChiSquaredTest:  Chi-squared Test, Fisher's Exact Test, and Kendall's  Tau Rank Correlation Coefficient  
 
 ##Description
 
Chi-squared Test, Fisher's Exact Test, and Kendall's  Tau Rank Correlation Coefficient
 
 
 
 ##Usage

```   
  rxChiSquaredTest(x, ...)
  rxFisherTest(x, ...)
  rxKendallCor(x, type = "b", seed = NULL, ...)
 
```
 
 
 ##Arguments

   
    
 ### `x`
 an object of class xtabs, rxCrossTabs, or rxCube. 
  
  
    
 ### `type`
 character string specifying the version of Kendall's correlation. Supported types are `"a"`, `"b"` or `"c"`. 
  
  
    
 ### `seed`
 seed for random number generator. If `NULL`, the seed is not set. 
  
  
    
 ### ` ...`
 additional arguments to the underlying function. 
  
 
 
 
 ##Value
 
an object of class rxMultiTest.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxMultiTest](rxMultiTest.md),
[rxPairwiseCrossTab](rxPairwiseCrosstab.md),
[rxRiskRatio](rxRiskRatio.md),
[rxOddsRatio](rxRiskRatio.md).
   
 
 ##Examples

 ```
   
  
  # Create a data frame with admissions data
  Admit <- factor(c(rep('Admitted', 46), rep('Rejected', 668)))
  Gender <- factor(c(rep('M', 22), rep('F', 24), rep('M', 351), rep('F', 317)))
  Admissions <- data.frame(Admit, Gender)
  
  # For most efficient computations, return an xtabs object from rxCrossTabs
  adminXtabs <- rxCrossTabs(~Admit:Gender, data = Admissions, returnXtabs = TRUE)
  rxChiSquaredTest(adminXtabs)
  rxFisherTest(adminXtabs)
  rxKendallCor(adminXtabs, type = "b")
 
```
 
 
 
