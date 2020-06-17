--- 

# required metadata 
title: "rxMarginals function (revoAnalytics) | Microsoft Docs" 
description: " Obtain marginal statistics on rxCrossTabs contingency tables. " 
keywords: "(revoAnalytics), rxMarginals, rxMarginals.rxCrossTabs, category, models" 
author: "dphansen" 
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



 # rxMarginals: Marginal Table Statistics 
 ## Description

Obtain marginal statistics on `rxCrossTabs` contingency tables.


 ## Usage

```   
 ## S3 method for class `rxCrossTabs':
rxMarginals  (x, output = "sums", ...)

```

 ## Arguments



 ### `x`
 object of class rxCrossTabs. 



 ### `output`
 character string specifying the type of output to display.  Choices are `"sums"`, `"counts"` and `"means"`. 



 ### ` ...`
 additional arguments to be passed directly to the underlying print method for the output list object. 



 ## Details

Developed primarily as a method to access marginal statistics from
[rxCrossTabs](rxCrossTabs.md) generated cross-tabulations.


 ## Value

list for each contingency table stored in an rxCrossTabs object. Each element
of the list contains a list of the marginal means (row, column, and grand) if
`output` is `"means"` or marginal sums otherwise.

 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[rxCrossTabs](rxCrossTabs.md)

 ## Examples

 ```

  admissions <- as.data.frame(UCBAdmissions)
  rxMarginals(rxCrossTabs(Freq ~ Gender : Admit, data = admissions, margin = TRUE,
                          means = FALSE))
  rxMarginals(rxCrossTabs(Freq ~ Gender : Admit, data = admissions, margin = TRUE,
                          means = TRUE))
```



