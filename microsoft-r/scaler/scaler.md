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

# RevoScaleR Functions

The `RevoScaleR` package includes hundreds of functions. While most of these functions are of general application, some are specific to particular compute contexts such as Hadoop, Teradata, or SQL Server and some may not be fully supported in all compute contexts. 

This topic presents a curated list of functions by Microsoft R users. These functions can be called directly from the command line. 

If you are looking for the functions optimized for Hadoop, Teradata, or SQL Server compute contexts, see the relevant function list for that context:
+ [Computing on a Hadoop Cluster](scaler-hadoop-functions.md)
+ Computing on a Teradata Datawarehouse
+ [Computing on SQL Server](functions-for-sql-server-data.md)

>Some function names begin with `rx` and others with `Rx`. The `Rx` function name prefix is used to distinguish the class constructors such as data sources and compute contexts.

>[!IMPORTANT]
>These are not exhaustive lists of functions in the RevoScaleR package. If you want to see the entire set of functions,  [follow these steps.](#findmore)

<br />
## Data Analysis Functions

<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![import](../media/scaler/import.png)
![import](../media/scaler/manip.png)
![import](../media/scaler/viz.png)&nbsp;
![import](../media/scaler/analytics.png)
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![import](../media/scaler/data-science-labels.png)


<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![import](../media/scaler/import copy.png)
![import](../media/scaler/manip copy.png)
![import](../media/scaler/viz copy.png)&nbsp;
![import](../media/scaler/analytics copy.png)
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![import](../media/scaler/data-science-labels.png)


Whenever you want to perform an analysis using `RevoScaleR` functions, you should specify three distinct pieces of information: 
 + The **compute context**, which specifies where the computations should take place
 + The **data source**, which is the data to be used
 + The **analytic function**, which specifies the analysis to be performed

<br />
###Import and Export


|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxImport()`       |![top](../media/award.png)|Creates an .xdf file or data frame from a data source (e.g. text, <br />SAS, SPSS data files, ODBC orTeradata connection,  <br />or data frame).|<small>[In package](scaler.md#findmore)</small>|
|`rxDataStep()`      |![top](../media/award.png)|Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from  <br />an .xdf file or a data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetInfo()`      |![top](../media/award.png)|Retrieves summary information from a data source or data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxSetInfo()`       |![top](../media/award.png)|Sets a file description in an .xdf file or a description attribute <br /> in a data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetVarInfo()`    | |Retrieves variable information from a data source or data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxSetVarInfo()`    | |Modifies variable information in an .xdf file or data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetVarNames()`   | |Retrieves variable names from a data source or data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxCreateColInfo()`   | |Generates a 'colInfo' list from a data source.|<small>[In package](scaler.md#findmore)</small>|
|`rxCompressXdf()`   | |Compresses an existing .xdf file, or a directory of .xdf files.|<small>[In package](scaler.md#findmore)</small>|
|`RxXdfData()`       |![top](../media/award.png)|Creates an efficient XDF data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxTextData()`      |![top](../media/award.png)|Creates a comma delimited text data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxSasData()`      | |Creates a SAS data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxSpssData()`      | |Creates a SPSS data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxOdbcData()`      | |Creates a ODBC data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxSqlServerData()`    | |Creates a SQL Server data source object.|<small>[View](RxSqlServerData.md)</small>|
|`RxTeradata()`     | |Creates a Teradata data source object.|<small>[In package](scaler.md#findmore)</small>|
|`rxOpen()`     | |Opens a data source for reading.|<small>[View](rxOpen.md)</small>|
|`rxClose()`      | |Closes a data source.     |<small>[View](rxClose.md)</small>|
|`rxReadNext()`      | |Read data from a source      |<small>[View](rxReadNext.md)</small>|
|`rxSetFileSystem()`      | |Specify a file system type for data for import.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetFileSystem()`     | |Retrieve the current file system type.|<small>[In package](scaler.md#findmore)</small>|
|`rxHdfsFileSystem()`      | |Creates an HDFS file system object.|<small>[In package](scaler.md#findmore)</small>|
|`rxNativeFileSystem()`       | |Creates a native file system object.|<small>[In package](scaler.md#findmore)</small>|


<br />
###Manipulate, Clean, and Transform

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxDataStep()`      |![top](../media/award.png)|Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from  <br />an .xdf file or a data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxFactors()`    |![top](../media/award.png)|Recode a factor variable or convert non-factor variable into a factor<br /> in an .xdf file or data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetFuzzyDist`      | |Get fuzzy distances for a character vector.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetFuzzyKeys`      | |Get fuzzy keys for a character vector.|<small>[In package](scaler.md#findmore)</small>|
|`rxSplit()`    |  |Splits an .xdf file or data frame into multiple .xdf files or data frames.|<small>[In package](scaler.md#findmore)</small>|
|`rxSort()`      | |Multi-key sorting of the variables an .xdf file or data frame.|<small>[In package](scaler.md#findmore)</small>|
|`rxMerge()`       | |Merges two .xdf files or data frames using avariety of merge types.|<small>[In package](scaler.md#findmore)</small>|
|`rxExecuteSQLDDL()`|  |SQL Server R Services only. Runs an arbitrary SQL DDL command.     |<small>[View](rxExecuteSQLDDL.md)</small>|

<br />
###Visualize

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxHistogram()`       |![top](../media/award.png)|Creates a histogram from data.|<small>[In package](scaler.md#findmore)</small>|
|`rxLinePlot()`  |![top](../media/award.png)|Creates a line plot from data.|<small>[In package](scaler.md#findmore)</small>|
| `rxLorenz()`      | |Computes a Lorenz curve which can be plotted.|<small>[In package](scaler.md#findmore)</small>|
|`rxRocCurve()`  | |Computes and plots ROC curves from actual and predicted data.|<small>[In package](scaler.md#findmore)</small>|


<br />
<br />
###Analyze, Learn, and Predict

####Descriptive Statistics and Cross-Tabulations

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxQuantile()`  |![top](../media/award.png) |Computes approximate quantiles for .xdf files and data frames without sorting. |<small>[In package](scaler.md#findmore)</small>|
|`rxSummary()`       |![top](../media/award.png)|Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported.|<small>[In package](scaler.md#findmore)</small>|
|`rxCrossTabs()`      |![top](../media/award.png) |Formula-based cross-tabulation of data.|<small>[In package](scaler.md#findmore)</small>|
|`rxCube()`  |![top](../media/award.png) |Alternative formula-based cross-tabulation designed for efficient representation  returning ‘cube’ results. Writing output to .xdf file not supported.|<small>[In package](scaler.md#findmore)</small>|
|`rxMarginals()`   | |Marginal summaries of cross-tabulations.|<small>[In package](scaler.md#findmore)</small>|
|`as.xtabs()`   | |Converts cross tabulation results to an `xtabs` object.|<small>[In package](scaler.md#findmore)</small>|
|`rxChiSquaredTest()`| |Performs Chi-squared Test on `xtabs` object. Used with smaller data sets and does not chunk data. |<small>[In package](scaler.md#findmore)</small>|
|`rxFisherTest()`   | |Performs Fisher's Exact Test on `xtabs` object. Used with smaller data sets and does not chunk data. |<small>[In package](scaler.md#findmore)</small>|
|`rxKendallCor()`   | |Computes Kendall's Tau Rank Correlation Coefficient using `xtabs` object.|<small>[In package](scaler.md#findmore)</small>|
|`rxPairwiseCrossTab()`   | |Apply a function to pairwise combinations of rows and columns of an `xtabs` object.|<small>[In package](scaler.md#findmore)</small>|
|`rxRiskRatio()`    | |Calculate the relative risk on a two-by-two `xtabs` object.|<small>[In package](scaler.md#findmore)</small>|
|`rxOddsRatio()`   | |Calculate the odds ratio on a two-by-two `xtabs` object.|<small>[In package](scaler.md#findmore)</small>|

<br />
####Statistical Modeling

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxLinMod()`   |![top](../media/award.png) |Fits a linear model to data.|<small>[In package](scaler.md#findmore)</small>|
|`rxLogit()`   |![top](../media/award.png) |Fits a logistic regression model to data.|<small>[In package](scaler.md#findmore)</small>|
|`rxGlm()`   |![top](../media/award.png) |Fits a generalized linear model to data.|<small>[In package](scaler.md#findmore)</small>|
|`rxCovCor()`   |![top](../media/award.png) |Calculate the covariance, correlation, or sum of squares / cross-product  <br />matrix for a set of variables.|<small>[In package](scaler.md#findmore)</small>|
|`rxDTree()`   |![top](../media/award.png) |Fits a classification or regression tree to data.|<small>[In package](scaler.md#findmore)</small>|
|`rxBTrees()`   |![top](../media/award.png) |Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm.|<small>[In package](scaler.md#findmore)</small>|
|`rxDForest()`   |![top](../media/award.png) |Fits a classification or regression decision forest to data.|<small>[In package](scaler.md#findmore)</small>|
|`rxPredict()`   |![top](../media/award.png) |Calculates predictions for fitted models. Output must be an XDF data source.|<small>[In package](scaler.md#findmore)</small>|
|`rxKmeans()`   |![top](../media/award.png) |Performs k-means clustering.|<small>[In package](scaler.md#findmore)</small>|
|`rxNaiveBayes()`   |![top](../media/award.png) |Performs Naive Bayes classification.|<small>[In package](scaler.md#findmore)</small>|
|`rxCov()`   | |Calculate the covariance matrix for a set of variables.|<small>[In package](scaler.md#findmore)</small>|
|`rxCor()`    | |Calculate the correlation matrix for a set of variables.|<small>[In package](scaler.md#findmore)</small>|
|`rxSSCP()`   | |Calculate the sum of squares / cross-product matrix for a set of variables.|<small>[In package](scaler.md#findmore)</small>|
|`rxRoc()`   | |Receiver Operating Characteristic (ROC) computations using actual  <br />and predicted values from binary classifier system.|<small>[In package](scaler.md#findmore)</small>|


<br />
##Data Sources and Compute Context Functions

<br/>
**Compute Contexts**

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxSetComputeContext()`|![top](../media/award.png)|Sets a compute context.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetComputeContext()`|![top](../media/award.png)|Gets the current compute context.|<small>[In package](scaler.md#findmore)</small>|
|`RxHadoopMR()`|![top](../media/award.png) |Creates an in-data, file-based Hadoop compute context.|<small>[In package](scaler.md#findmore)</small>|
|`RxSpark()`|![top](../media/award.png) |Creates an in-data, file-based Spark compute context.|<small>[In package](scaler.md#findmore)</small>|
|`RxInTeradata()`|![top](../media/award.png) |Creates an in-database compute context for Teradata.|<small>[In package](scaler.md#findmore)</small>|
|`RxInSqlServer()`| |Creates an in-database compute context for SQL Server 2016.|<small>[In package](scaler.md#findmore)</small>|
|`RxComputeContext()`||Creates a compute context.|<small>[In package](scaler.md#findmore)</small>|
|`RxLocalSeq()`| |Creates a local compute context for `rxExec` using sequential computations.|<small>[In package](scaler.md#findmore)</small>|
|`RxLocalParallel()`| |Creates a local compute context for `rxExec` using <br />the `parallel` package as back end.|<small>[In package](scaler.md#findmore)</small>|
|`rxForeachDoPar()`   | |Creates a compute context for `rxExec` using <br />the current `foreach` parallel back end.|<small>[In package](scaler.md#findmore)</small>|


<br/>
**Data Sources**

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`RxXdfData()`       | |Creates an efficient XDF data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxTextData()`      | |Creates a comma delimited text data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxSasData()`      | |Creates a SAS data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxSpssData()`      | |Creates a SPSS data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxOdbcData()`      | |Creates a ODBC data source object.|<small>[In package](scaler.md#findmore)</small>|
|`RxSqlServerData()`    | |Creates a SQL Server data source object.|<small>[View](RxSqlServerData.md)</small>|
|`RxTeradata()`     | |Creates a Teradata data source object.|<small>[In package](scaler.md#findmore)</small>|

<br />
##HPC and Distributed Computing Functions

These functions and many more can be used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../scaler-distributed-computing.md).

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxExec()`  | |Run an arbitrary R function on nodes or cores of a cluster.|<small>[In package](scaler.md#findmore)</small>|
|`rxRngNewStream()`   | |Support for Parallel Random Number Generation.|<small>[In package](scaler.md#findmore)</small>|
|`rxRngDelStream()`   | |Support for Parallel Random Number Generation.|<small>[In package](scaler.md#findmore)</small>|
|`rxRngGetStream()`   | |Support for Parallel Random Number Generation.|<small>[In package](scaler.md#findmore)</small>|
|`rxRngSetStream()`   | |Support for Parallel Random Number Generation.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetAvailableNodes()`| |Get all the available nodes on a distributed compute context.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetNodeInfo()`| |Get information on nodes specified for a distributed compute context.|<small>[In package](scaler.md#findmore)</small>|
|`rxPingNodes()`| |Test round trip from end user through computation node(s) in <br /> a cluster or cloud.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetJobStatus()`| |Get the status of a non-waiting distributed computing job.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetJobResults()`| |Get the return object(s) of a non-waiting distributed computing job.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetJobOutput()`| |Get the console output from a non-waiting distributed computing job.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetJobs()`| |Get the available distributed computing job information objects.|<small>[In package](scaler.md#findmore)</small>|
|`rxLocateFile()`| |Get the first occurrence of a specified input file in a set of specified paths.|<small>[In package](scaler.md#findmore)</small>|
 
<br />
##Utility Functions
>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:||
|`rxOptions()`  |![top](../media/award.png) |Gets or sets `RevoScaleR`-specific options.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetOption()`   |![top](../media/award.png) |Retrieves a specific `RevoScaleR`-option.|<small>[In package](scaler.md#findmore)</small>|
|`rxGetEnableThreadPool()`   | |Gets the current state of the thread pool, which on Linux can be<br/> either persistent or on-demand.|<small>[In package](scaler.md#findmore)</small>|
|`rxSetEnableThreadPool()`   | |Sets the thread pool state.|<small>[In package](scaler.md#findmore)</small>|
|`rxStepControl()`   | |Construct `variable.selection` argument for `rxLinMod`.|<small>[In package](scaler.md#findmore)</small>|
|`rxIsOpen()` | |      |<small>[View](rxIsOpen.md)</small>|
|`rxSqlServerDropTable()`| |      |<small>[View](rxSqlServerDropTable.md)</small>|  
|`rxSqlServerTableExists()`| |      |<small>[View](rxSqlServerTableExists.md)</small>|
|`rxWriteNext()`| |      |<small>[View](rxWriteNext.md)</small>|
 

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

   ![objects](../media/scaler-rconsole-obj.png)

1. At the R prompt, type `objects(<position>)` to reveal the set of functions such as:
   ```
   > objects(5)
   ```

1. Find the name of the function in which you are interested. 

1. At the R prompt, type `?<function_name>` to open the help file for that function, such as:
   ```
   > ?rxXdfData
   ```