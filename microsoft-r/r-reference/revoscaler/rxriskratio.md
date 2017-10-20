--- 
 
# required metadata 
title: "rxRiskRatio function (RevoScaleR) " 
description: " Calculate the relative risk and odds ratio on a two-by-two table. " 
keywords: "(RevoScaleR), rxRiskRatio, rxOddsRatio, htest" 
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
 
 
 
 #rxRiskRatio:  Relative Risk Ratio and Odds Ratio  
 
 ##Description
 
Calculate the relative risk and odds ratio on a two-by-two table.
 
 
 
 ##Usage

```   
  rxRiskRatio(tbl, conf.level = 0.95, alternative = "two.sided")
  rxOddsRatio(tbl, conf.level = 0.95, alternative = "two.sided")
 
```
 
 
 ##Arguments

   
    
 ### `tbl`
 two-by-two table. 
  
    
 ### `conf.level`
 level of the confidence interval. 
  
    
 ### `alternative`
 character string defining alternative hypothesis. Supported values are `"two.sided"`, `"less"`, or `"greater"`. 
  
 
 
 
 ##Value
 
an object of class htest.
 
 

 
 
 
 
 ##See Also
 
[rxChiSquaredTest](rxChiSquaredTest.md),
[rxFisherTest](rxChiSquaredTest.md),
[rxKendallCor](rxChiSquaredTest.md),
[rxMultiTest](rxMultiTest.md),
[rxPairwiseCrossTab](rxPairwiseCrosstab.md)
   
 
 ##Examples

 ```
   
  mtcarsDF <- rxFactors(datasets::mtcars, factorInfo = c("gear", "cyl", "vs"), 
                        varsToKeep = c("gear", "cyl", "vs"), sortLevels = TRUE)
  gearVsXtabs <- rxCrossTabs(~ gear : vs, data = mtcarsDF, returnXtabs = TRUE)
  rxRiskRatio(gearVsXtabs[1:2,])
  rxOddsRatio(gearVsXtabs[1:2,])
 
```
 
 
 
