--- 
 
# required metadata 
title: "CensusWorkers function (RevoScaleR) | Microsoft Docs" 
description: " A small subset of the IPUMS 5% sample of the 2000 U.S. Census data containing information on the working population from Connecticut, Indiana, and Washington. " 
keywords: "(RevoScaleR), CensusWorkers, CensusWorkers.xdf, datasets" 
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
 
 
 
 
 #CensusWorkers: Subset of IPUMS 2000 U.S. Census Data 
 ##Description
 
A small subset of the IPUMS 5% sample of the 2000 U.S. Census data containing
information on the working population from Connecticut, Indiana, and
Washington.
 
 
 ##Format
 
An .xdf file with 351121 observations on the following 6 variables.


###`age`
integer variable containing ages.


###`incwage`
integer variable containing wage and salary income. The value 999999 from the original data has been converted to missing.


###`perwt`
integer variable containing person weights, that is, a frequency weight for each person record. This is treated as integer data in R.


###`sex`
factor variable sex with levels `Male` and `Female`.


###`wkswork1`
integer variable containing the weeks worked the previous year.


###`state`
factor variable containing the state with levels `Connecticut`, `Indiana`, and `Washington`.



 
 
 ##Details
 
The 5% 2000 U.S. Census sample is a stratified 5% sample of households,
produced and maintained by the Minnesota Population Center at the University
of Minnesota as part of the *Integrated Public Use Microdata Series*, or
IPUMS. The full data set contains 14,583,731 rows with 265 variables. This
data was converted to the .xdf format, using  the SPSS dictionary
provided on the IPUMS web site to obtain variable labels, value labels, and
value codes. The subsample was taken using the following rowSelection:
`(age >= 20) & (age <= 65) & (wkswork1 > 20) &
``(stateicp == "Connecticut" | stateicp == "Washington" | stateicp == "Indiana")`
 
 
 ##Source
  
Minnesota Population Center, University of Minnesota.
*Integration Public Use Microdata Series*. [`http://www.ipums.org`](http://www.ipums.org)
.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[CensusUS5Pct2000](CensusUS5Pct2000.md)
   
 ##Examples

 ```
   
  censusWorkers <- file.path(rxGetOption("sampleDataDir"),
                             "CensusWorkers.xdf")
  rxSummary(~ age + incwage, data = censusWorkers)
 
```
 
 
