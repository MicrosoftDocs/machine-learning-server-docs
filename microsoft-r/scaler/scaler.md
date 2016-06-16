---

# required metadata
title: "ScaleR Functions"
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

# RevoScaleR Functions

The `RevoScaleR` package includes hundreds of functions. Not all of these functions are useful to everyone and there are some functions that are very useful when switching your compute context to Hadoop, Teradata, or SQL Server.  

This topic is presents the most **commonly used functions by Microsoft R users**. These functions can be called directly from the command-line. 

If you are looking for the functions optimized for Hadoop, Teradata, or SQL Server compute contexts, see the relevant function list for that context:
+ [Computing on a Hadoop Cluster](scaler-fx-hadoop.md)
+ [Computing on a Teradata Datawarehouse](scaler-fx-teradata.md)
+ [Computing on SQL Server](functions-for-sql-server-data.md)


>[!IMPORTANT]
>These are not exhaustive lists of functions in the RevoScaleR package. If you want to see the entire set of functions,  [follow these steps.](#findmore)

<br />
## Data Analysis Functions

>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![import](../media/scaler-puzzle1.png)
![import](../media/scaler-puzzle2.png)
![import](../media/scaler-puzzle3.png)
![import](../media/scaler-puzzle4.png)

<br />
###Import and Export


|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxImport()`       |![top](../media/award.png)|Move data from an ODBC source to the XDF file|<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxGetInfo()`      |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSetInfo()`       |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxGetVarInfo()`    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSetVarInfo()`    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxGetVarNames()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxCompressXdf()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxXdfData()`       | |Create an XDF data object      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxTextData()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSasData()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSpssData()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxOdbcData()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`RxSqlServerData()`    | |      |<small>[Help](RxSqlServerData.md)</small>|
|`rxTeradata()`     | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxOpen()`     | |Open a data source for reading|<small>[Help](rxOpen.md)</small>|
|`rxClose()`      | |Close a data source      |<small>[Help](rxClose.md)</small>|
|`rxReadNext()`      | |Read data from a source      |<small>[Help](rxReadNext.md)</small>|
|`rxSetFileSystem()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxGetFileSystem()`     | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxHdfsFileSystem()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxNativeFileSystem()`       | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|Functions `rxDataStep()`,<br/> `rxXdfToDataFrame()`, <br/>and `rxReadXdf:Reads()`<br/> can also read <br/>an .XDF file into <br/>a data  frame.    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|


<br />
###Manipulate, Clean, and Transform

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxDataStep()`       |![top](../media/award.png)|| See package|
|`rxFactors()`    |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSplit()`    |  |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSort()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxMerge()`       | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxExecuteSQLDDL()`|  |SQL Server R Services only.      |<small>[Help](rxExecuteSQLDDL.md)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|


<br />
###Visualize

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxHistogram()`       |![top](../media/award.png)|| See package|
|`rxLinePlot()`  |![top](../media/award.png)|      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
| `rxLorenz()`      | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxRocCurve()`  | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|



<br />
###Analyze, Learn, and Predict

####Descriptive Statistics and Cross-Tabulations

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxQuantile()`  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSummary()`       |![top](../media/award.png)||<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxCrossTabs()`      |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxCube()`  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxMarginals()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxChiSquaredTest()`    | |Used with smaller data sets, not XDF files.       |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxFisherTest()`   | |Used with smaller data sets, not XDF files.      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxKendallCor()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxPairwiseCrossTab()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxRiskRatio()`    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxOddsRatio()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|


####Statistical Modeling

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxLinMod()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxLogit()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxGlm()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxCovCor()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxDTree()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxBTrees()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxDForest()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxPredict()`   |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxCov()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxCor()`    | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSSCP()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxRoc()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxKmeans()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxHistogram()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|


<br />
##Data Sources and Compute Context Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`RxComputeContext()`  |![top](../media/award.png) |      |<small>[Help](RxComputeContext.md)</small>|
|`rxGetComputeContext()`   | |      |<small>[Help](rxGetComputeContext.md)</small>|
|`rxSetComputeContext()`  | |      |<small>[Help](rxSetComputeContext.md)</small>|
|  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxForeachDoPar()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxLocalParallel()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxLocalSeq()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxInSqlServer()` | |      |<small>[Help](RxInSqlServer.md)</small>|
|`rxSpark`   |  |Hadoop specific.      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|


<br />
##HPC and Distributed Computing Functions

These functions and many more can be used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../scaler-distributed-computing.md).

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxExec`  | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxRngNewStream()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxRngDelStream()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxRngGetStream()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxRngSetStream()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|

 
<br />
##Utility Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxOptions()`  |![top](../media/award.png) |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxGetOption()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxGetEnableThreadPool()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxSetEnableThreadPool()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxStepControl()`   | |      |<small>[Find help in<br /> package](scaler.md#findmore)</small>|
|`rxIsOpen()` | |      |<small>[Help](rxIsOpen.md)</small>|
|`rxSqlServerDropTable()`| |      |<small>[Help](rxSqlServerDropTable.md)</small>|  
|`rxSqlServerTableExists()`| |      |<small>[Help](rxSqlServerTableExists.md)</small>|
|`rxWriteNext()`| |      |<small>[Help](rxWriteNext.md)</small>|
 
<br />
##Hadoop Convenience Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.



<br>

<br>

<a name="findmore"></a>
##See All Functions and Help Files

See the list of public functions and see the associated help pages using the following steps.

**To see the `RevoScaleR` functions that can be called from the commands-line:**

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe`, R Tools for Visual Studio, RStudio, or another IDE. 

1. In the console, return the number of objects by typing the following at the R prompt `>`:
   ```
   > search()
   ```

1. Identify the position of the object you are interested in. In the case of our example, RevoScaleR is in the fifth position.

1. At the R prompt, type `objects(<position>)` to reveal the set of functions such as:
   ```
   > objects(5)
   ```

   ![objects](../media/scaler-rconsole-obj.png)

1. At the R prompt, type `?<function_name>` to open the help file for that function, such as:
   ```
   > ?rxXdfData
   ```
