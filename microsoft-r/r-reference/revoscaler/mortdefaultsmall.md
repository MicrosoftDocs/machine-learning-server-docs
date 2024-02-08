--- 

# required metadata 
title: "mortDefaultSmall data (revoAnalytics) | Microsoft Docs" 
description: " Simulated data on mortgage defaults. " 
keywords: "(revoAnalytics), mortDefaultSmall, mortDefaultSmall.xdf, mortDefaultSmall2000.csv, mortDefaultSmall2001.csv, mortDefaultSmall2002.csv, mortDefaultSmall2003.csv, mortDefaultSmall2004.csv, mortDefaultSmall2005.csv, mortDefaultSmall2006.csv, mortDefaultSmall2007.csv, mortDefaultSmall2008.csv, mortDefaultSmall2009.csv, datasets" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.service: mlserver
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














 # mortDefaultSmall: Smaller Mortgage Default Demonstration Data 
 ## Description

Simulated data on mortgage defaults.


 ## Format

An .xdf file with 50000 observations on the following 6 variables:


### `creditScore`
FICO credit scores (stored as an integer).


### `houseAge`
house age, by range (stored as integer).


### `yearsEmploy`
years employed at current employer (stored as integer).


### `ccDebt`
the amount of debt (stored as integer).


### `year`
year of the observation (stored as integer).


### `default`
1 if the homeowner was in default on the loan, 0 otherwise (stored as an integer).





 ## Details

This data set simulates mortgage default data. The data set is created
by looping over the input data files mortDefaultSmall200x.csv, each of
which contains a year's worth of simulated data.


 ## Source

Microsoft Corporation.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## Examples

 ```

  mortDefaultSmall <- rxDataStep(data = file.path(rxGetOption("sampleDataDir"),
                                "mortDefaultSmall.xdf"))
  rxSummary(~ creditScore + houseAge, data = mortDefaultSmall)
```


