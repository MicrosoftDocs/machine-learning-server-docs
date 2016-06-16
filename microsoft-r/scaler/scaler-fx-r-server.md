---

# required metadata
title: "RevoScaleR Functions"
description: "ScaleR Functions"
keywords: "RevoScaleR, ScaleR"
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "06/13/2016"
ms.topic: "article"
ms.prod: "microsoftr"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# ScaleR Function Map for Microsoft R Server

The `RevoScaleR` package includes hundreds of functions. About 180 functions can be called directly from the command-line.

This topic is presents the most commonly used functions by Microsoft R Server users. These functions can be called directly from the command-line. 

>[!IMPORTANT]
>This is not an exhaustive list of functions in the RevoScaleR package. If you want to see the entire set of functions,  [follow these steps.](scaler.md#findmore)


## Functions for Data Analysis

###The Data Science Flow
![import](../media/scaler-puzzle1.png)
![import](../media/scaler-puzzle2.png)
![import](../media/scaler-puzzle3.png)
![import](../media/scaler-puzzle4.png)

<br />
###Import and Export

>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

|Function Name          |   |Description|Help|
|-----------------------|---|-----------------------|-----------------------|
|[`rxImport()`]()       |![top](../media/award.png)|Move data from an ODBC source to the XDF file| See package|
|[`rxXdfToText()`]()    |![top](../media/award.png)|      |[See package](scaler.md#findmore)|
|[`rxGetInfo()`]()      |![top](../media/award.png)|      |[See package](scaler.md#findmore)|
|[`rxSetInfo()`]()       |![top](../media/award.png)|      |[See package](scaler.md#findmore)|
|[`rxGetVarInfo()`]()    |  |      |[See package](scaler.md#findmore)|
|[`rxSetVarInfo()`]()    |  |      |[See package](scaler.md#findmore)|
|[`rxGetVarNames()`]()   |  |      |[See package](scaler.md#findmore)|
|[`rxCompressXdf()`]()   |  |      |[See package](scaler.md#findmore)|
|[`RxXdfData()`]()       |  |Create an XDF data object      |[See package](scaler.md#findmore)|
|[`RxTextData()`]()      |  |      |[See package](scaler.md#findmore)|
|[`RxSasData()`]()      |  |      |[See package](scaler.md#findmore)|
|[`RxSpssData()`]()      |  |      |[See package](scaler.md#findmore)|
|[`RxOdbcData()`]()      |  |      |[See package](scaler.md#findmore)|
|`RxSqlServerData()`    |   |      |[See Help](RxSqlServerData.md)|
|[`RxTeradata()`]()     |  |      |[See package](scaler.md#findmore)|
|`rxOpen()`     |  |Open a data source for reading|[See Help](rxOpen.md)|
|`rxClose()`      |  |Close a data source      |[See Help](rxClose.md)|
|`rxReadNext()`      |  |Read data from a source      |[See Help](rxReadNext.md)|
|[`rxSetFileSystem()`]()      |  |      |[See package](scaler.md#findmore)|
|[`rxGetFileSystem()`]()     |  |      |[See package](scaler.md#findmore)|
|[`RxHdfsFileSystem()`]()      |  |      |[See package](scaler.md#findmore)|
|[`RxNativeFileSystem()`]()       |  |      |[See package](scaler.md#findmore)|
|Functions `rxDataStep()`,<br/> `rxXdfToDataFrame()`, <br/>and `rxReadXdf:Reads()`<br/> can also read <br/>an .XDF file into <br/>a data  frame.    |  |      |[See package](scaler.md#findmore)|
|   | |      |[See package](scaler.md#findmore)|


<br />
###Manipulate, Clean, and Transform

|Function Name          |   |Description|Help|
|-----------------------|---|-----------------------|-----------------------|
|[`rxDataStep()`]()       |![top](../media/award.png)|| See package|
|[`rxFactors()`]()    |![top](../media/award.png)|      |[See package](scaler.md#findmore)|
|[`rxSplit()`]()    |  |      |[See package](scaler.md#findmore)|
|[`rxSort()`]()      | |      |[See package](scaler.md#findmore)|
|[`rxMerge()`]()       | |      |[See package](scaler.md#findmore)|
|[`rxExecuteSQLDDL()`](rxExecuteSQLDDL.md)    |  |      |[See package](scaler.md#findmore)|
|   | |      |[See package](scaler.md#findmore)|


<br />
###Visualize

|Function Name          |   |Description|Help|
|-----------------------|---|-----------------------|-----------------------|
|[`rxHistogram()`]()       |![top](../media/award.png)|| See package|
|[`rxLinePlot()`]()  |![top](../media/award.png)|      |[See package](scaler.md#findmore)|
| [`rxLorenz()`]()      | |      |[See package](scaler.md#findmore)|
|[`rxRocCurve()`]()  | |      |[See package](scaler.md#findmore)|



<br />
###Analyze, Learn, and Predict

|Function Name          |  |Description|Help|
|-----------------------|--|-----------------------|-----------------------|
|[`rxQuantile()`]()  |![top](../media/award.png) |      |[See package](scaler.md#findmore)|
|[`rxSummary()`]()       |![top](../media/award.png)|| See package|
|[`rxCrossTabs()`]()      |![top](../media/award.png) |      |[See package](scaler.md#findmore)|
|[`rxCube()`]()  |![top](../media/award.png) |      |[See package](scaler.md#findmore)|
|[`rxPredict()`]()   |![top](../media/award.png) |      |[See package](scaler.md#findmore)|
|[`rxLinMod()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxCovCor()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxCov()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxCor()`]()    | |      |[See package](scaler.md#findmore)|
|[`rxSSCP()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxLogit()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxRoc()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxGlm()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxDTree()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxKmeans()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxDForest()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxHistogram()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxMarginals()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxChiSquaredTest()`]()    |Used with smaller data sets, not XDF files. |      |[See package](scaler.md#findmore)|
|[`rxFisherTest()`]()   |Used with smaller data sets, not XDF files. |      |[See package](scaler.md#findmore)|
|[`rxKendallCor()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxPairwiseCrossTab()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxRiskRatio()`]()    | |      |[See package](scaler.md#findmore)|
|[`rxOddsRatio()`]()   | |      |[See package](scaler.md#findmore)|
|   | |      |[See package](scaler.md#findmore)|





<br />
##Data Sources and Compute Contexts

|Function Name          |  |Description|Help|
|-----------------------|--|-----------------------|-----------------------|
|`RxComputeContext()`  |![top](../media/award.png) |      |[See Help](RxComputeContext.md)|
|`rxGetComputeContext()`   | |      |[See Help](rxGetComputeContext.md)|
|`rxSetComputeContext()`  | |      |[See Help](rxSetComputeContext.md)|
|  |![top](../media/award.png) |      |[See package](scaler.md#findmore)|
|[`RxForeachDoPar()`]()   | |      |[See package](scaler.md#findmore)|
|[`RxLocalParallel()`]()   | |      |[See package](scaler.md#findmore)|
|[`RxLocalSeq()`]()   | |      |[See package](scaler.md#findmore)|
|[`RxInSqlServer()`](RxInSqlServer.md)   | |      |[See help](RxInSqlServer.md)|
|   | |      |[See package](scaler.md#findmore)|
|   | |      |[See package](scaler.md#findmore)|
|   | |      |[See package](scaler.md#findmore)|
|   | |      |[See package](scaler.md#findmore)|

<br />
##Distributed Computing Functions

These functions and many more can be used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../scaler-distributed-computing.md).

|Function Name          | |Description|Help|
|-----------------------|--|-----------------------|-----------------------|
|`rxExec`  | |      |[See package](scaler.md#findmore)|
|[`rxRngNewStream()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxRngDelStream()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxRngGetStream()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxRngSetStream()`]()   | |      |[See package](scaler.md#findmore)|

 
<br />
##Utility Functions

|Function Name          | |Description|Help|
|-----------------------|--|-----------------------|-----------------------|
|[`rxOptions()`]()  |![top](../media/award.png) |      |[See package](scaler.md#findmore)|
|[`rxGetOption()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxGetEnableThreadPool()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxSetEnableThreadPool()`]()   | |      |[See package](scaler.md#findmore)|
|[`rxStepControl()`]()   | |      |[See package](scaler.md#findmore)|

 
<br />
##Hadoop Convenience Functions



<br>
[`rxIsOpen()`](rxIsOpen.md)
[`rxSqlServerDropTable()`](rxSqlServerDropTable.md)     
[`rxSqlServerTableExists()`](rxSqlServerTableExists.md)
[`rxWriteNext()`](rxWriteNext.md)
<br>