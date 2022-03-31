--- 

# required metadata 
title: "Kyphosis data (revoAnalytics) | Microsoft Docs" 
description: " The kyphosis data in .xdf format. " 
keywords: "(revoAnalytics), Kyphosis, kyphosis.xdf, datasets" 
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



 # Kyphosis: Data on a Post-Operative Spinal Defect 
 ## Description

The kyphosis data in .xdf format.


 ## Format

An .xdf file with 81 observations on 4 variables. For a complete
description of the variables, see kyphosis.


 ## Source

John M. Chambers and Trevor Hastie, Ed., (1992)
*Statistical Models in S*. Belmont, CA. Wadsworth.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

kyphosis

 ## Examples

 ```

  Kyphosis <- rxDataStep(inData = file.path(rxGetOption("sampleDataDir"),
                        "kyphosis.xdf"))
  rxSummary(~ Kyphosis + Age + Number + Start, data = Kyphosis)
```


