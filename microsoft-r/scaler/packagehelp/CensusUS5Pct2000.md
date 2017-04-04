--- 
 
# required metadata 
title: "IPUMS 2000 U.S. Census Data" 
description: " The IPUMS 5% sample of the 2000 U.S. Census data in .xdf format. " 
keywords: "RevoScaleR, CensusUS5Pct2000, CensusUS5Pct2000.xdf, datasets" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 
 
 #`CensusUS5Pct2000`: IPUMS 2000 U.S. Census Data

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
The IPUMS 5% sample of the 2000 U.S. Census data in .xdf format.
 
 
 ##Format
 
An .xdf file with 14,058,983 observations on 264 variables. For a
complete description of the variables, see
[`http://usa.ipums.org/usa-action/variables/group`](http://usa.ipums.org/usa-action/variables/group)
.
 
 
 ##Details
 
The data in CensusUS5Pct2000 comes from the *Integrated Public Use
Microdata Series* (IPUMS) 5% 2000 U.S. Census sample, produced and maintained
by the Minnesota Population Center at the University of Minnesota. The full
data set at the time of initial conversion contained 14,583,731 rows
with 265 variables. This data was converted to the .xdf format in 2006,
using the SPSS dictionary provided on the IPUMS web site to obtain variable
labels, value labels, and value codes. Variable names in the .xdf file
are all lower case but are upper case in the IPUMS dictionary. In this version
of the sample, observations with probability weights (`perwt`) equal to 0
have been removed. The `"q"` variables such as `qage` that take the
values of 0 and 4 in the original data have been converted to logicals for
more efficient storage. It was compressed and updated to a current .xdf
file format in 2013.

This data set is available for download, but is not
included in the standard **RevoScaleR** distribution. It is, however, used
in several documentation examples and demo scripts. You are free to use this
file (and subsamples of it) subject to the restrictions imposed by IPUMS, but
do so at your own risk.
 
 
 ##Source
 
Minnesota Population Center, University of Minnesota. *Integration Public
Use Microdata Series*. [`http://www.ipums.org`](http://www.ipums.org)
.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[CensusWorkers](CensusWorkers.md)
   
 ##Examples

 ```
   
  ## Not run:
 
dataPath = "C:/Microsoft/Data"
bigCensusData <- file.path(dataPath, "CensusUS5Pct2000.xdf") 
censusEarly20s <-  file.path(dataPath, "CensusEarly20s.xdf")
rxDataStep(inData = bigCensusData, outFile = censusEarly20s,
			  rowSelection = age >= 20 & age <= 25,
              varsToKeep = c("age", "incwage", "perwt", "sex", "wkswork1")) 
rxGetInfo(censusEarly20s, getVarInfo = TRUE) 
 ## End(Not run) 
  
 
```
 
 
