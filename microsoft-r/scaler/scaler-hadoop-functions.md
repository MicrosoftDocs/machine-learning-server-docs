---

# required metadata
title: "ScaleR Functions"
description: "ScaleR Functions"
keywords: "RevoScaleR, ScaleR"
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "06/22/2016"
ms.topic: "article"
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

# RevoScaleR Functions for Hadoop

The `RevoScaleR` package provides a set of portable, scalable, distributable data analysis functions. While most of these functions are of general application, some are specific to the Hadoop compute contexts and some may not be fully supported in these compute contexts. 

This guide presents a curated list of functions that might be particularly interesting to Hadoop users. These functions can be called directly from the command line. 

>If you are looking for a more general list of `RevoScaleR` functions for Microsoft R, [see here](scaler.md).

As noted in the [RevoScaleR Hadoop MapReduce Getting Started Guide](../scaler-hadoop-getting-started.md#composite), the XDF file format has been modified for Hadoop to store data in a composite set of files rather than a single file. Both of these data sources can be specified for use with the Hadoop Distributed File System (HDFS).

## Data Analysis Functions
<br />
####Import and Export

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`RxXdfData`       |![top](../media/award.png)|Creates an efficient XDF data source object.|<small>[See package](scaler.md#findmore)</small>|
|`RxTextData`      |![top](../media/award.png)|Creates a comma delimited text data source object.|<small>[See package](scaler.md#findmore)</small>|
|`rxDataStep`      |![top](../media/award.png)|Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from  an .xdf file or a data frame.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetInfo`      |![top](../media/award.png)|Retrieves summary information from a data source or data frame.|<small>[See package](scaler.md#findmore)</small>|
|`rxSetInfo`       |![top](../media/award.png)|Sets a file description in an .xdf file or a description attribute in a data frame.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetVarInfo`    | |Retrieves variable information from a data source or data frame.|<small>[See package](scaler.md#findmore)</small>|
|`rxSetVarInfo`    | |Modifies variable information in an .xdf file or data frame.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetVarNames`   | |Retrieves variable names from a data source or data frame.|<small>[See package](scaler.md#findmore)</small>|
|`rxHdfsFileSystem`      | |Creates an HDFS file system object.|<small>[See package](scaler.md#findmore)</small>|



<br />
####Manipulate, Clean, and Transform 

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxDataStep`      |![top](../media/award.png)|Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from  an .xdf file or a data frame.|<small>[See package](scaler.md#findmore)</small>|
|`rxFactors`    |![top](../media/award.png)|Create or recode factor variables in a composite XDF file in HDFS. A new file must be written out.|<small>[See package](scaler.md#findmore)</small>|


<br />
####Visualize

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxHistogram`       |![top](../media/award.png)|Creates a histogram from data.|<small>[See package](scaler.md#findmore)</small>|
|`rxLinePlot`  |![top](../media/award.png)|Creates a line plot from data.|<small>[See package](scaler.md#findmore)</small>|
| `rxLorenz`      | |Computes a Lorenz curve which can be plotted.|<small>[See package](scaler.md#findmore)</small>|
|`rxRocCurve`  | |Computes and plots ROC curves from actual and predicted data.|<small>[See package](scaler.md#findmore)</small>|


<br />
####Analyze with Descriptive Statistics and Cross-Tabulations

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxQuantile`  |![top](../media/award.png) |Computes approximate quantiles for .xdf files and data frames without sorting. |<small>[See package](scaler.md#findmore)</small>|
|`rxSummary`       |![top](../media/award.png)|Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported.|<small>[See package](scaler.md#findmore)</small>|
|`rxCrossTabs`      |![top](../media/award.png) |Formula-based cross-tabulation of data.|<small>[See package](scaler.md#findmore)</small>|
|`rxCube`  |![top](../media/award.png) |Alternative formula-based cross-tabulation designed for efficient representation  returning ‘cube’ results. Writing output to .xdf file not supported.|<small>[See package](scaler.md#findmore)</small>|

<br />
####Analyze, Learn, and Predict with Statistical Modeling

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxLinMod`   |![top](../media/award.png) |Fits a linear model to data.|<small>[See package](scaler.md#findmore)</small>|
|`rxLogit`   |![top](../media/award.png) |Fits a logistic regression model to data.|<small>[See package](scaler.md#findmore)</small>|
|`rxGlm`   |![top](../media/award.png) |Fits a generalized linear model to data.|<small>[See package](scaler.md#findmore)</small>|
|`rxCovCor`   |![top](../media/award.png) |Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.|<small>[See package](scaler.md#findmore)</small>|
|`rxDTree`   |![top](../media/award.png) |Fits a classification or regression tree to data.|<small>[See package](scaler.md#findmore)</small>|
|`rxBTrees`   |![top](../media/award.png) |Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm.|<small>[See package](scaler.md#findmore)</small>|
|`rxDForest`   |![top](../media/award.png) |Fits a classification or regression decision forest to data.|<small>[See package](scaler.md#findmore)</small>|
|`rxPredict`   |![top](../media/award.png) |Calculates predictions for fitted models. Output must be an XDF data source.|<small>[See package](scaler.md#findmore)</small>|
|`rxKmeans`   |![top](../media/award.png) |Performs k-means clustering.|<small>[See package](scaler.md#findmore)</small>|


<br />
<a name="compute"></a>

##Compute Context Functions

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`RxHadoopMR`|![top](../media/award.png) |Creates an in-data, file-based Hadoop compute context.|<small>[See package](scaler.md#findmore)</small>|
|`RxSpark`|![top](../media/award.png) |Creates an in-data, file-based Spark compute context.|<small>[See package](scaler.md#findmore)</small>|
|`rxInstalledPackages`   | |Returns the list of installed packages for a compute context.|<small>[View](rxInstalledPackages.md)</small>|
|`rxFindPackage`   | |Returns the path to one or more packages for a compute context.|<small>[View](rxFindPackage.md)</small>|


<br/>
<a name="data"></a>

##Data Source Functions

Of course, not all data source types are available on all compute contexts. For the Hadoop compute contexts, two types of data sources can be used. 

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`RxXdfData`       | |Creates an efficient XDF data source object.|<small>[See package](scaler.md#findmore)</small>|
|`RxTextData`      | |Creates a comma delimited text data source object.|<small>[See package](scaler.md#findmore)</small>|


<br />
##High Performance Computing and Distributed Computing Functions

The Hadoop compute context has a number of helpful functions used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../scaler-distributed-computing.md).

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxExec`  | |Run an arbitrary R function on nodes or cores of a cluster.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetJobStatus`| |Get the status of a non-waiting distributed computing job.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetJobResults`| |Get the return object(s) of a non-waiting distributed computing job.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetJobOutput`| |Get the console output from a non-waiting distributed computing job.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetJobs`| |Get the available distributed computing job information objects.|<small>[See package](scaler.md#findmore)</small>|



<br />
##Utility Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxOptions`  |![top](../media/award.png) |Gets or sets `RevoScaleR`-specific options.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetOption`   |![top](../media/award.png) |Retrieves a specific `RevoScaleR`-option.|<small>[See package](scaler.md#findmore)</small>|
|`rxGetEnableThreadPool`   | |Gets the current state of the thread pool, which on Linux can be either persistent or on-demand.|<small>[See package](scaler.md#findmore)</small>|
|`rxSetEnableThreadPool`   | |Sets the thread pool state.|<small>[See package](scaler.md#findmore)</small>|
|`rxStepControl`   | |Construct `variable.selection` argument for `rxLinMod`.|<small>[See package](scaler.md#findmore)</small>|
|`rxIsOpen` | |      |<small>[View](rxIsOpen.md)</small>|
|`rxSqlServerDropTable`| |      |<small>[View](rxSqlServerDropTable.md)</small>|  
|`rxSqlServerTableExists`| |      |<small>[View](rxSqlServerTableExists.md)</small>|
|`rxWriteNext`| |      |<small>[View](rxWriteNext.md)</small>|
 
<br />
##Hadoop Convenience Functions

RevoScaleR also provides some wrapper functions for accessing Hadoop/HDFS functionality via R. These functions require access to Hadoop, either locally or remotely via the RxHadoopMR or RxSpark compute contexts.

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxHadoopCommand`| |Execute an arbitrary Hadoop command. Allows you to run basic Hadoop commands.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopVersion`| |Return the current Hadoop version.|<small>[See package](scaler.md#findmore)</small>| 
|`rxHadoopCopyFromClient`| |Copy a file from a remote client to the Hadoop cluster's local file system, and then to HDFS.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopCopyFromLocal`| |Copy a file from the native file system to HDFS. Wraps the Hadoop `fs -copyFromLocal` command.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopCopyToLocal`| |Copy a file from HDFS to the local file system. Wraps the Hadoop `fs -copyToLocal` command.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopCopy`| |Copy a file in the Hadoop Distributed File System (HDFS). Wraps the Hadoop `fs -cp` command.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopRemove`| |Remove a file in HDFS. Wraps the Hadoop `fs -rm` command.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopListFiles`| |List files in an HDFS directory. Wraps the Hadoop `fs -ls` or `fs -lsr` command.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopMakeDir`| |Make a directory in HDFS. Wraps the Hadoop `fs -mkdir` command.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopMove`| |Move a file in HDFS. Wraps the Hadoop `fs -mv` command.|<small>[See package](scaler.md#findmore)</small>|
|`rxHadoopRemoveDir`| |Remove a directory in HDFS. Wraps the Hadoop `fs -rmr` command.|<small>[See package](scaler.md#findmore)</small>|