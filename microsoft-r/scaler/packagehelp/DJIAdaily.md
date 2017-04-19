--- 
 
# required metadata 
title: "Dow Jones Industrial Average Data" 
description: " Daily data for the Dow Jones Industrial Average. " 
keywords: "RevoScaleR, DJIAdaily, DJIAdaily.xdf, datasets" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 #`DJIAdaily`: Dow Jones Industrial Average Data

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Daily data for the Dow Jones Industrial Average.
 
 
 ##Format
 
An .xdf file with 20,636 observations on the following 13 variables.


###`Date`
character string containing the date.


###`Open`
opening value (stored as float).


###`High`
high value of day (stored as float).


###`Low`
low value of day (stored as float).


###`Close`
closing value (stored as float).


###`Volume`
daily volume (stored as float).


###`Adj.Close`
closing value adjusted for stock splits and dividends (stored as float).


###`Year`
integer value for year (1928 - 2010).


###`Month`
integer value for month (1 - 12).


###`DayOfMonth`
integer value for day of month (1 - 31).


###`DayOfWeek`
character string containing day of week.


###`DaysSince1928`
integer for number of days since Jan. 1, 1928.


###`YearFrac`
fractional year (1928 + DaysSince1928/365.25) (stored as float).



 
 
 ##Details
 
The DJIAdaily.xdf file is an .xdf containing daily information on
the Dow Jones Industrial Average.
 
 
 ##Source
 
http://ichart.finance.yahoo.com/table.csv?s=^DJI
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##Examples

 ```
   
  DJIAdaily <-  file.path(rxGetOption("sampleDataDir"), "DJIAdaily.xdf")
  rxGetInfo(DJIAdaily, getVarInfo = TRUE)
 
```
 
 
