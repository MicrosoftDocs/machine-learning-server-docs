--- 

# required metadata 
title: "pow function (revoAnalytics) | Microsoft Docs" 
description: " Formula expression functions for **RevoScaleR**. " 
keywords: "(revoAnalytics), pow, models" 
author: "DaniBunny"
ms.author: "dacoelho" 
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


 # pow: Formula Expression Functions for RevoScaleR 
 ## Description

Formula expression functions for **RevoScaleR**.


 ## Usage

```   
  pow(x, exponent)

```

 ## Arguments



 ### `x`
 variable name 


 ### `exponent`
 number to use as an exponent 



 ## Details

Many of the primary analysis functions in **RevoScaleR** require a formula
argument. Arithmetic expressions in those arguments provide flexibility in the
model. This man page lists the functions in **RevoScaleR** that are not
native to R.

`pow` performs the power transformation `x^exponent`.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[rxFormula](rxFormula.md),
[rxTransform](rxTransform.md),
[rxCrossTabs](rxCrossTabs.md),
[rxCube](rxCube.md),
[rxLinMod](rxLinMod.md),
[rxLogit](rxLogit.md),
[rxSummary](rxSummary.md).

 ## Examples

 ```

  # define local data.frame
  admissions <- as.data.frame(UCBAdmissions)

  # cross-tabulation
  rxCrossTabs(pow(Freq, 2) ~ Gender : Admit, data = admissions)
  rxCrossTabs(Freq^2 ~ Gender : Admit, data = admissions)

  # linear modeling
  rxLinMod(pow(Freq, 2) ~ Gender + Admit, data = admissions)
  rxLinMod(Freq^2 ~ Gender + Admit, data = admissions)

  # model summarization
  rxSummary(~ pow(Freq, 2) + Gender + Admit, data = admissions)
  rxSummary(~ I(Freq^2) + Gender + Admit, data = admissions)
```


