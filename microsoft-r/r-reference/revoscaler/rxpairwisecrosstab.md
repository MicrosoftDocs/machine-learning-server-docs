--- 

# required metadata 
title: "rxPairwiseCrossTab function (revoAnalytics) | Microsoft Docs" 
description: " Apply a function 'FUN' to all pairwise combinations of the rows and columns of an xtabs object,  stratifying by higher dimensions. " 
keywords: "(revoAnalytics), rxPairwiseCrossTab, print.rxPairwiseCrossTabList, print.rxPairwiseCrossTabTable, htest" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "sql-non-specified"
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
ms.technology: machine-learning-server

--- 





 # rxPairwiseCrossTab:  Apply Function to Pairwise Combinations of an xtabs Object 

 ## Description

Apply a function 'FUN' to all pairwise combinations of the rows and columns of an xtabs object, 
stratifying by higher dimensions.



 ## Usage

```   
  rxPairwiseCrossTab(x, pooled = TRUE, FUN = rxRiskRatio, ...)
 ## S3 method for class `rxPairwiseCrossTabList':
print  (x, ...)
 ## S3 method for class `rxPairwiseCrossTabTable':
print  (x, digits = 3, scientific = FALSE, p.value.digits = 3, ...)

```


 ## Arguments




 ### `x`
 an object of class xtabs, rxCrossTabs, or rxCube for `rxPairwiseCrossTab`. An object of class rxPairwiseCrossTabList or rxPairwiseCrossTabTable for the print methods of rxPairwiseCrossTabList and rxPairwiseCrossTabTable objects. 



 ### `pooled`
 logical value. If `TRUE`, the comparison groups are pooled.  Otherwise all pairwise comparisons are included. 



 ### `FUN`
 function that takes a two by two table and returns an object of class htest. 



 ### ` ...`
 additional arguments. 



 ### `digits`
 number of digits to use in printing numeric values. 



 ### `scientific`
 logical value. If `TRUE`, scientific notation is used to prin tnumeric values. 



 ### `p.value.digits`
 number of digits to use in printing p-values. 




 ## Value
 a rxPairwiseCrossTabList object. 

 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## See Also

[rxChiSquaredTest](rxChiSquaredTest.md),
[rxFisherTest](rxChiSquaredTest.md),
[rxKendallCor](rxChiSquaredTest.md),
[rxMultiTest](rxMultiTest.md),
[rxRiskRatio](rxRiskRatio.md),
[rxOddsRatio](rxRiskRatio.md).


 ## Examples

 ```

  mtcarsDF <- rxFactors(datasets::mtcars, factorInfo = c("gear", "cyl", "vs"), 
                        varsToKeep = c("gear", "cyl", "vs"), sortLevels = TRUE)
  xtabObj <- rxCrossTabs(~ gear : cyl : vs, data = mtcarsDF, returnXtabs = TRUE)

  rxPairwiseCrossTab(xtabObj, FUN = rxRiskRatio)
  rxPairwiseCrossTab(xtabObj, pooled = FALSE, FUN = rxOddsRatio)
```



