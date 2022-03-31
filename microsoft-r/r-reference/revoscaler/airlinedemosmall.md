--- 

# required metadata 
title: "AirlineDemoSmall data (revoAnalytics) | Microsoft Docs" 
description: " A small sample of airline on-time performance data. " 
keywords: "(revoAnalytics), AirlineDemoSmall, AirlineDemoSmall.xdf, AirlineDemoSmallUC.xdf, AirlineDemoSmall.csv, AirlineDemo1kNoMissing.csv, datasets" 
author: "chuckheinzelman"
ms.author: "charlhe" 
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







 # AirlineDemoSmall: Small Airline Demonstration File 
 ## Description

A small sample of airline on-time performance data.


 ## Format

An .xdf file with 600000 observations on the following 3 variables:


### `ArrDelay`
arrival delay, in minutes (stored as integer).


### `CRSDepTime`
schedule departure time (stored as float32).


### `DayOfWeek`
day of the week (stored as a factor).





 ## Details

This data set is a small subsample of airline on-time performance data
containing only 600,000 observations of 3 variables, so it will fit in 
memory on most systems. It is an .xdf
file, which means that the data are stored in *blocks*. The
AirlineDemoSmall.xdf data file contains 3 blocks.  It is compressed
using zlib compression.  The AirlineDemoSmallUC.xdf contains
the same data, but without compression.

The data are also available in text format in the file
AirlineDemoSmall.csv. A smaller subset, with only 1010 rows and no
missing data, is available in text format in the file
AirlineDemo1kNoMissing.csv.


 ## Source

American Statistical Association Statistical Computing Group, Data Expo '09.
[`http://stat-computing.org/dataexpo/2009/the-data.html`](http://stat-computing.org/dataexpo/2009/the-data.html)



 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## References

U.S. Department of Transportation, Bureau of Transportation Statistics,
Research and Innovative Technology Administration. Airline On-Time Statistics. 
[`http://www.bts.gov/xml/ontimesummarystatistics/src/index.xml`](http://www.bts.gov/xml/ontimesummarystatistics/src/index.xml)




 ## See Also

[AirOnTime87to12](AirOnTime87to12.md)

 ## Examples

 ```

  airlineSmall <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.xdf")
  rxSummary(~ ArrDelay + CRSDepTime, data = airlineSmall)
```


