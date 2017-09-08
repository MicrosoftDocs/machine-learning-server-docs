--- 
 
# required metadata 
title: "mortDefaultSmall function (RevoScaleR) | Microsoft Docs" 
description: " Simulated data on mortgage defaults. " 
keywords: "(RevoScaleR), mortDefaultSmall, mortDefaultSmall.xdf, mortDefaultSmall2000.csv, mortDefaultSmall2001.csv, mortDefaultSmall2002.csv, mortDefaultSmall2003.csv, mortDefaultSmall2004.csv, mortDefaultSmall2005.csv, mortDefaultSmall2006.csv, mortDefaultSmall2007.csv, mortDefaultSmall2008.csv, mortDefaultSmall2009.csv, datasets" 
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
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 #mortDefaultSmall: Smaller Mortgage Default Demonstration Data 
 ##Description
 
Simulated data on mortgage defaults.
 
 
 ##Format
 
An .xdf file with 50000 observations on the following 6 variables.


###`creditScore`
FICO credit scores (stored as an integer).


###`houseAge`
house age, by range (stored as integer).


###`yearsEmploy`
years employed at current employer (stored as integer).


###`ccDebt`
the amount of debt (stored as integer).


###`year`
year of the observation (stored as integer).


###`default`
1 if the homeowner was in default on the loan, 0 otherwise (stored as an integer).



 
 
 ##Details
 
This data set simulates mortgage default data. The data set is created
by looping over the input data files mortDefaultSmall200x.csv, each of
which contains a year's worth of simulated data.
 
 
 ##Source
  
Microsoft Corporation.
 
 

 
 
 
 ##Examples

 ```
   
  mortDefaultSmall <- rxDataStep(data = file.path(rxGetOption("sampleDataDir"),
                                "mortDefaultSmall.xdf"))
  rxSummary(~ creditScore + houseAge, data = mortDefaultSmall)
 
```
 
 
