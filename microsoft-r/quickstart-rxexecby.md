---

# required metadata
title: "rxExecBy example: parallel processing on partitioned data in Microsoft R"
description: "Parallel processing of partitioned data with the RevoScaleR function rxExecBy"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/16/2017"
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

# RevoScaleR rxExecBy parallel processing example (Microsoft R)

Many of our enterprise customers don’t have a "big data, big model" problem. They have a "small data, many models" problem, where there is a need to train separate models such as ARIMA (for time-series forecasting) or boosted trees over a large number of small data sets. The trained models could be used for time-series predictions, or to score fresh data for each small data partition. Typical examples include time-series forecasting of smart meters for households, revenue forecasting for product lines, or loan approvals for bank branches.

The new `rxExecBy` function in [RevoScaleR](scaler/scaler.md) is designed for use cases calling for high-volume parallel processing over a large number of small data sets. Given this data profile, you can use `rxExecBy` to read in the data, partition the data, and then call a function to iterate over each partition in parallel.

## How to use rxExecBy

`rxExecBy` takes four inputs and produces an output for each partition, in whatever product the user-defined function computes. The function can be almost any user-defined or analytical or statistical function from the collection of Microsoft R packages, able to [execute jobs in parallel](scaler-distributed-computing-parallel-jobs.md). The data sets can be .csv files loaded via `RxTextData`. The parallel processing occurs when you run the `rxExecby` script on a platform offering distributed computing. In this case, either Spark or SQL Server Machine Learning Services.

Input criteria | Method |
---------------|--------|
Data sources | RxTextData, RxXdfData, RxHiveData, RxParquetData, RxOrcData, rxSparkDataOps |
Keys | Choose one or more fields used to group data, such as an ID.
Compute context | rxSpark, rxInSQLServer |
User-defined functions | rxLinMod, rxLogit, rxPredict, rxGlm, rxCovCor, rxDtree, and others |

## Sample code using Airline data set 

To demonstrate `rxExecBy`, we will use the airline dataset with flight delay data values for multiple airports, over multiple years. In just the small dataset alone, there are over #### data points. 

In our demonstration, our objective is to understand the flight delays by day of the week; in other words, the average delay for Mondays, Tuesdays, and so forth. The script in this tutorial shows you how to accomplish this task using  `rxExecBy`.

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

## See Also

[RevoScaleR](scaler/scaler.md)