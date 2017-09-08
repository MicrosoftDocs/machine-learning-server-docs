--- 
 
# required metadata 
title: "AirlineData87to08 function (RevoScaleR) | Microsoft Docs" 
description: " Airline on-time performance data from 1987 to 2008. " 
keywords: "(RevoScaleR), AirlineData87to08, AirlineData87to08.xdf, datasets" 
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
 
 
 
 
 #AirlineData87to08: Airline On-Time Performance Data 
 ##Description
 
Airline on-time performance data from 1987 to 2008.
 
 
 ##Format
 
An .xdf file with 123534969 observations on the following 29 variables.


###`Year`
year of the flight (stored as factor).


###`Month`
month of the flight (stored as factor).


###`DayOfMonth`
day of the month (1 to 31) (stored as integer).


###`DayOfWeek`
day of the week (stored as factor).


###`DepTime`
actual departure time (stored as float).


###`CRSDepTime`
scheduled departure time (stored as float).


###`ArrTime`
actual arrival time (stored as float).


###`CRSArrTime`
scheduled arrival time (stored as float).


###`UniqueCarrier`
carrier ID (stored as factor).


###`FlightNum`
flight number (stored as factor).


###`TailNum`
plane's tail number (stored as factor).


###`ActualElapsedTime`
actual elapsed time of the flight, in minutes  (stored as integer).


###`CRSElapsedTime`
scheduled elapsed time of the flight, in minutes (stored as integer).


###`AirTime`
airborne time for the flight, in minutes (stored as integer).


###`ArrDelay`
arrival delay, in minutes (stored as integer).


###`DepDelay`
departure delay, in minutes (stored as integer).


###`Origin`
originating airport (stored as factor).


###`Dest`
destination airport (stored as factor).


###`Distance`
flight distance (stored as integer).


###`TaxiIn`
 taxi time from wheels down to arrival at the gate, in minutes (stored as integer).


###`TaxiOut`
taxi time from departure from the gate to wheels up, in minutes (stored as integer).


###`Cancelled`
cancellation status (stored as logical).


###`CancellationCode`
cancellation code, if applicable (stored as factor).


###`Diverted`
diversion status (stored as logical).


###`CarrierDelay`
delay, in minutes, attributable to the carrier (stored integer).


###`WeatherDelay`
delay, in minutes, attributable to weather factors (stored as integer).


###`NASDelay`
delay, in minutes, attributable to the National Aviation System (stored as integer).


###`SecurityDelay`
delay, in minutes, attributable to security factors (stored as integer).


###`LateAircraftDelay`
delay, in minutes, attributable to late-arriving aircraft (stored as integer).



 
 
 ##Details
 
This data set contains on-time performance data from 1987 to 2008. It is an
.xdf file, which means that the data are stored in *blocks*. The
AirlineData87to08.xdf data file contains 832 blocks.
 
 
 ##Source
  
American Statistical Association Statistical Computing Group, Data Expo '09.
[`http://stat-computing.org/dataexpo/2009/the-data.html`](http://stat-computing.org/dataexpo/2009/the-data.html)

 
 

 
 
 
 ##References
 
U.S. Department of Transportation, Bureau of Transportation Statistics,
Research and Innovative Technology Administration. Airline On-Time Statistics. 
[`http://www.bts.gov/xml/ontimesummarystatistics/src/index.xml`](http://www.bts.gov/xml/ontimesummarystatistics/src/index.xml)

 
 
 ##See Also
 
[AirlineDemoSmall](AirlineDemoSmall.md)
   
 ##Examples

 ```
   
  ## Not run:
 
airlineDemoBig <- rxDataStep(file = "AirlineData87to08.xdf",
                            varsToKeep = c("ArrDelay", "DepDelay", "DayOfWeek"),
                            startRow = 100000, numRows = 1000) 
summary(airlineDemoBig) 
 ## End(Not run) 
  
 
```
 
 
