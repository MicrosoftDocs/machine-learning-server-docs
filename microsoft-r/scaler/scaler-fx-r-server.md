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

# ScaleR Function Map

The `RevoScaleR` package includes hundreds of functions. 

This topic is presents the most commonly used functions by Microsoft R users. These functions can be called directly from the command-line. Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

>[!IMPORTANT]
>This is not an exhaustive list of functions in the RevoScaleR package. If you want to see the entire set of functions,  [follow these steps.](scaler.md#findmore)


## Functions for Data Analysis

>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

###The Data Science Flow
![import](../media/scaler-puzzle1.png)
![import](../media/scaler-puzzle2.png)
![import](../media/scaler-puzzle3.png)
![import](../media/scaler-puzzle4.png)

<br />
###Import and Export


|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|[`rxImport()`]()       |![top](../media/award.png)|Move data from an ODBC source to the XDF file| See package|
|[`rxXdfToText()`]()    |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxGetInfo()`]()      |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxSetInfo()`]()       |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxGetVarInfo()`]()    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxSetVarInfo()`]()    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxGetVarNames()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxCompressXdf()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxXdfData()`]()       | |Create an XDF data object      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxTextData()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxSasData()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxSpssData()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxOdbcData()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`RxSqlServerData()`    | |      |[See Help](RxSqlServerData.md)|
|[`RxTeradata()`]()     | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxOpen()`     | |Open a data source for reading|[See Help](rxOpen.md)|
|`rxClose()`      | |Close a data source      |[See Help](rxClose.md)|
|`rxReadNext()`      | |Read data from a source      |[See Help](rxReadNext.md)|
|[`rxSetFileSystem()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxGetFileSystem()`]()     | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxHdfsFileSystem()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxNativeFileSystem()`]()       | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|Functions `rxDataStep()`,<br/> `rxXdfToDataFrame()`, <br/>and `rxReadXdf:Reads()`<br/> can also read <br/>an .XDF file into <br/>a data  frame.    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|


<br />
###Manipulate, Clean, and Transform

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|[`rxDataStep()`]()       |![top](../media/award.png)|| See package|
|[`rxFactors()`]()    |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxSplit()`]()    |  |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxSort()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxMerge()`]()       | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxExecuteSQLDDL()`](rxExecuteSQLDDL.md)    |  |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|


<br />
###Visualize

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|[`rxHistogram()`]()       |![top](../media/award.png)|| See package|
|[`rxLinePlot()`]()  |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
| [`rxLorenz()`]()      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxRocCurve()`]()  | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|



<br />
###Analyze, Learn, and Predict

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|[`rxQuantile()`]()  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxSummary()`]()       |![top](../media/award.png)|| See package|
|[`rxCrossTabs()`]()      |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxCube()`]()  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxPredict()`]()   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxLinMod()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxCovCor()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxCov()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxCor()`]()    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxSSCP()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxLogit()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxRoc()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxGlm()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxDTree()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxKmeans()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxDForest()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxHistogram()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxMarginals()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxChiSquaredTest()`]()    |Used with smaller data sets, not XDF files. |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxFisherTest()`]()   |Used with smaller data sets, not XDF files. |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxKendallCor()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxPairwiseCrossTab()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxRiskRatio()`]()    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxOddsRatio()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|





<br />
##Data Sources and Compute Contexts
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`RxComputeContext()`  |![top](../media/award.png) |      |[See Help](RxComputeContext.md)|
|`rxGetComputeContext()`   | |      |[See Help](rxGetComputeContext.md)|
|`rxSetComputeContext()`  | |      |[See Help](rxSetComputeContext.md)|
|  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxForeachDoPar()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxLocalParallel()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxLocalSeq()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`RxInSqlServer()`](RxInSqlServer.md)   | |      |[See help](RxInSqlServer.md)|
|`rxSpark`   |Hadoop specific.  |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|

<br />
##Distributed Computing Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

These functions and many more can be used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../scaler-distributed-computing.md).

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxExec`  | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxRngNewStream()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxRngDelStream()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxRngGetStream()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxRngSetStream()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|

 
<br />
##Utility Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|[`rxOptions()`]()  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxGetOption()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxGetEnableThreadPool()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxSetEnableThreadPool()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|[`rxStepControl()`]()   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|

 
<br />
##Hadoop Convenience Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.



<br>
[`rxIsOpen()`](rxIsOpen.md)
[`rxSqlServerDropTable()`](rxSqlServerDropTable.md)     
[`rxSqlServerTableExists()`](rxSqlServerTableExists.md)
[`rxWriteNext()`](rxWriteNext.md)
<br>