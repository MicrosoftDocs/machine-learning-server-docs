--- 
 
# required metadata 
title: " Multiple Variable Independence Tests " 
description: " Collects a list of tests for variable independence into a table. " 
keywords: "RevoScaleR, rxMultiTest, print.rxMultiTest, htest" 
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
 
 
 
 
 #`rxMultiTest`:  Multiple Variable Independence Tests 

 Applies to version 9.1.0 of package RevoScaleR.
 
 
 ##Description
 
Collects a list of tests for variable independence into a table.
 
 
 
 ##Usage

```   
  rxMultiTest(tests, title = NULL)
  
 ## S3 method for class `rxMultiTest':
print  (x, ...)
 
```
 
 
 ##Arguments

   
    
 ### `tests`
 a list of objects of class htest. 
  
  
    
 ### `title`
 a title for the multiTest table. If `NULL`, the title is determined from  the method slot of the htest. 
  
  
    
 ### `x`
 object of class rxMultiTest. 
  
  
    
 ### ` ...`
 additional arguments to the print method. 
  
 
 
 
 ##Value
 
an object of class rxMultiTest.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxChiSquaredTest](rxchisquaredtest.md),
[rxFisherTest](rxchisquaredtest.md),
[rxKendallCor](rxchisquaredtest.md),
[rxPairwiseCrossTab](../../scaler/packagehelp/rxpairwisecrosstab.md),
[rxRiskRatio](../../scaler/packagehelp/rxriskratio.md),
[rxOddsRatio](../../scaler/packagehelp/rxriskratio.md).
   
 
 ##Examples

 ```
   
  data(mtcars) 
  tests <- list(t.test(mpg ~ vs, data = mtcars),
   		     t.test(hp ~ vs, data = mtcars))
  rxMultiTest(tests)
 
```
 
 
 
