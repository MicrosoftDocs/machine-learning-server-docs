--- 
 
# required metadata 
title: "claims data (revoAnalytics) | Microsoft Docs" 
description: " Observations on automobile insurance claims. " 
keywords: "(revoAnalytics), claims, claims.xdf, claims.txt, claimsExtra.txt, datasets" 
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
 
 
 
 
 
 
 #claims: Auto Insurance Claims Data 
 ##Description
 
Observations on automobile insurance claims.
 
 
 ##Format
 
An .xdf file with 127 observations on the following 6 variables.


###`RowNum`
integer containing the row numbers.


###`age`
driver's age (stored as a factor).


###`car.age`
car's age (stored as a factor).


###`type`
type of claim (stored as a factor).


###`cost`
cost of each claim (stored as a float32).


###`number`
number of claims per driver (stored as a float32).



 
 
 ##Details
 
The claims.xdf file is an .xdf version of the `claims`
data frame used in the White Book (*Statistical Models in S*). The
claims.txt file is a comma-separated text version of the same data. The
file claimsExtra.txt is a simulated data set with four additional
observations of the original six variables.
 
 
 ##Source
 
Baxter, L.A., Coutts, S.M., and Ross, G.A.F. (1980) Applications of Linear
Models in Motor Insurance. *Proceedings of the 21st International
Congress of Actuaries*, Zurich, 11-29.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##References
 
John M. Chambers and Trevor Hastie, Ed., (1992)
*Statistical Models in S*. Belmont, CA. Wadsworth.
 
 
 ##Examples

 ```
   
  claims <- rxDataStep(inData = file.path(rxGetOption("sampleDataDir"),
                                       "claims.xdf"))
  rxSummary(~ age + car.age, data = claims)
 
```
 
 
