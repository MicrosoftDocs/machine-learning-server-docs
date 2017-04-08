---

# required metadata
title: "Quick start: Parallel processing on partitioned data with rxExecBy"
description: "Quick start for Microsoft R: Parallel processing on partitioned data with rxExecBy"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/30/2017"
ms.topic: "get-started-article"
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

# Quick start: Parallel processing on partitioned data with rxExecBy

You can partition an arbitrary dataset into thousands or millions of partitions, and then iterate over each partition using almost any user-defined or analytical or statistical function from the collection of Microsoft R packages. The new `rxExecBy` function in RevoScaleR streamlines this task by scanning, sorting, and grouping data into partitions, and then calling whatever function you have specified over each partition. Inputs include a data source, a key used to group and partition data, and a function used to perform an operation.

Consider the airline dataset with flight delay data values for multiple airports, over multiple years. In just the small dataset alone, there are over #### data points. Suppose you wanted to understand the flight delays by day of the week, for example, the average delay for Mondays, Tuesdays, and so forth. The script in this tutorial shows you how to accomplish this task using the new `rxExecBy` function.

## Prerequisites

R Server or R Client

Tools and sample data

## Load, process, and return results

TBD

## Practicum

Many of our enterprise customers don’t have a big data, big model problem. They have a small data many models problem. That is, the customers want to train separate models such as ARIMA (for time-series forecasting) or boosted trees within groups such as “smart meters” or “aircraft engines” or “products”. The trained models can then be used for time-series predictions, or to score fresh data for each of those small data partitions. Typical examples include time-series forecasting of smart meters for households, revenue forecasting for product lines, and loan approvals for bank branches.  

### Sample code for small data many models feature 

~~~~
cc <- rxSparkConnect(reset = TRUE) 
 
delayFunc <- function(key, data, params) { 
    df <- rxImport(inData = data) 
    rxLinMod(ArrDelay ~ CRSDepTime, data = df) 
} 
 
airlineData <- 
    RxTextData( 
        "/share/SampleData/AirlineDemoSmall.csv", 
        colInfo = list( 
            ArrDelay = list(type = "numeric"), 
            DayOfWeek = list(type = "factor") 
        ), 
        fileSystem = RxHdfsFileSystem() 
    ) 
 
returnObjs <- rxExecBy(airlineData, c("DayOfWeek"), delayFunc)  
 
rxSparkDisconnect(cc)

~~~~
 
Things to Try 
 
Reading data from a Hive table and passing it directly as input to rxExecBy() 

Return values of different types from the user defined function 
List 
Single value 
R data frame 

Introduce errors in processing certain partitions (using the stop() function) and see the return status appropriately reflected 
Use multiple keys in the "keys" argument

## See Also

TBD