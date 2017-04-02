--- 
 
# required metadata 
title: " Relative Risk Ratio and Odds Ratio " 
description: " Calculate the relative risk and odds ratio on a two-by-two table. " 
keywords: "RevoScaleR, rxRiskRatio, rxOddsRatio, htest" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 #`rxRiskRatio`:  Relative Risk Ratio and Odds Ratio 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 
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
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
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
 
 
 
