---

# required metadata
title: "RevoScaleR package for R (Machine Learning Server) "
description: "Function help reference for the RevoScaleR R package of Machine Learning Server and Microsoft R"
keywords: "RevoScaleR, ScaleR"
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "10/09/2017"
ms.topic: "reference"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# RevoScaleR package

The **RevoScaleR** library is a collection of portable, scalable, and distributable R functions for importing, transforming, and analyzing data at scale. You can use it for descriptive statistics, generalized linear models, k-means clustering, logistic regression, classification and regression trees, and decision forests. 

Functions run on the **RevoScaleR** interpreter, built on open source R, engineered to leverage the multithreaded and multinode architecture of the host platform.

| Package details | |
|--------|-|
| Version: |  9.2.1 |
| Runs on: | [Machine Learning Server 9.2.1](../../what-is-machine-learning-server.md) </br>[R Client (Windows and Linux)](../../r-client/what-is-microsoft-r-client.md) <br/>[R Server 9.1 and earlier](../../what-is-microsoft-r-server.md)   <br/>[SQL Server 2016 and later (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) |
| Built on: | R 3.3.x (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|

## How to use RevoScaleR

The **RevoScaleR** library is found in Machine Learning Server and Microsoft R products. You can use any R IDE to write R script calling functions in **RevoScaleR**, but the script must run on a computer having the interpreter and libraries.

**RevoScaleR** is often preloaded into tools that integrate with Machine Learning Server and R Client, which means you can call functions without having to load the library. If the library is not loaded, you can load **RevoScaleR** from the command line by typing `library(RevoScaleR)`.

### Run it locally

This is the default. **RevoScaleR** runs locally on all platforms, including R Client. On a standalone Linux or windows system, data and operations are local to the machine. On Hadoop, a local compute context means that data and operations are local to current execution environment (typically, an edge node). 

### Run in a remote compute context

**RevoScaleR** runs remotely on computers that have a server installation. In a remote compute context, the script running on a local R Client or Machine Learning Server shifts execution to a remote Machine Learning Server. For example, script running on Windows might shift execution to a Spark cluster to process data there. 

On distributed platforms, such as Hadoop processing frameworks (Spark and MapReduce), set the compute context to [RxSpark](RxSpark.md) or [RxHadoopMR](RxHadoopMR.md) and give the cluster name. In this context, if you call a function that can run in parallel, the task is distributed across data nodes in the cluster, where the operation is co-located with the data. 

On SQL Server, set the compute context to [RxInSQLServer](RxInSqlServer.md). There are two primary use cases for remote compute context: 

+ Call R functions in T-SQL script or stored procedures running on SQL Server.  

+ Call **RevoScaleR** functions in R script executing in a SQL Server [compute context](../../r/concept-what-is-compute-context.md). In your script, you can set a compute context to shift execution of **RevoScaleR** operations to a remote SQL Server instance that has the **RevoScaleR** interpreter.

Some functions in **RevoScaleR** are specific to particular compute contexts. A filtered list of functions include the following:
+ [Computing on a Hadoop Cluster](revoscaler-hadoop-functions.md)
+ [Computing on SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/scaler-functions-for-working-with-sql-server-data)

## Typical workflow
<!--<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![import](./media/revoscaler/import.png)
![import](./media/revoscaler/manip.png)
![import](./media/revoscaler/viz.png)&nbsp;
![import](./media/revoscaler/analytics.png)
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![import](./media/revoscaler/data-science-labels.png)


<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![import](./media/revoscaler/import copy.png)
![import](./media/revoscaler/manip copy.png)
![import](./media/revoscaler/viz copy.png)&nbsp;
![import](./media/revoscaler/analytics copy.png)
-->

<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![import](./media/revoscaler/data-science-labels.png)


Whenever you want to perform an analysis using `RevoScaleR` functions, you should specify three distinct pieces of information:
 + The [**analytic function**](#analytics), which specifies the analysis to be performed
 + The [**compute context**](#compute), which specifies where the computations should take place
 + The [**data source**](#data), which is the data to be used

## Functions by category

The library includes data transformation and manipulation, visualization, predictions, and statistical analysis functions. It also includes functions for controlling jobs, serializing data, and performing common utility tasks.

This section lists the functions by category to give you an idea of how each one is used. The table of contents lists functions in alphabetical order.

> [!Note]
> Some function names begin with `rx` and others with `Rx`. The `Rx` function name prefix is used for class constructors for data sources and compute contexts.

<a name="data-source-functions"></a>

## 1-Data source functions

| Function name | Description |
|---------------|-------------|
|[RxXdfData](rxxdfdata.md) |Creates an efficient XDF data source object. |
|[RxTextData](rxtextdata.md) |Creates a comma-delimited text data source object. |
|[RxSasData](rxsasdata.md) |Creates a SAS data source object. |
|[RxSpssData](rxspssdata.md) |Creates an SPSS data source object. |
|[RxOdbcData](rxodbcdata.md) |Creates an ODBC data source object. |
|[RxTeradata](rxteradata.md) |Creates a Teradata data source object. |
|[RxSqlServerData](rxsqlserverdata.md) |Creates a SQL Server data source object. |

<a name="import-export-functions"></a>

## 2-Import and export

| Function name | Description |
|---------------|-------------|
|[rxImport](rximport.md) <sup>*</sup> |Creates an .xdf file or data frame from a data source (for example, text, SAS, SPSS data files, ODBC or Teradata connection, or data frame). | 
|[rxDataStep](rxdatastep.md) <sup>*</sup> |Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from an .xdf file or a data frame. | 
|[rxGetInfo](rxgetinfoxdf.md) <sup>*</sup> |Retrieves summary information from a data source or data frame. | 
|[rxSetInfo](rxsetinfo.md) <sup>*</sup> |Sets a file description in an .xdf file or a description attribute in a data frame. | 
|[rxGetVarInfo](rxgetvarinfoxdf.md) |Retrieves variable information from a data source or data frame. | 
|[rxSetVarInfo](rxsetvarinfoxdf.md)  |Modifies variable information in an .xdf file or data frame. | 
|[rxGetVarNames](rxgetvarnames.md)  |Retrieves variable names from a data source or data frame. | 
|[rxCreateColInfo](rxcreatecolinfo.md)  |Generates a colInfo list from a data source. | 
|[rxCompressXdf](rxcompressxdf.md)  |Compresses an existing .xdf file, or a directory of .xdf files. | 
|[RxXdfData](rxxdfdata.md)  |Creates an efficient XDF data source object. | 
|[RxTextData](rxtextdata.md)  |Creates a comma-delimited text data source object. | 
|[RxSasData](rxsasdata.md)  |Creates a SAS data source object. | 
|[RxSpssData](rxspssdata.md)  |Creates an SPSS data source object. | 
|[RxOdbcData](rxodbcdata.md)  |Creates an ODBC data source object. | 
|[RxTeradata](rxteradata.md)  |Creates a Teradata data source object. | 
|[RxSqlServerData](rxsqlserverdata.md)  |Creates a SQL Server data source object | 
|[rxOpen](rxopen-methods.md)  |Opens a data source for reading. | 
|[rxClose](rxopen-methods.md)  |Closes a data source. | 
|[rxReadNext](rxopen-methods.md)  |Read data from a source. | 
|[rxSetFileSystem](rxsetfilesystem.md)  |Specify a file system type for data for import. | 
|[rxGetFileSystem](rxsetfilesystem.md)  |Retrieve the current file system type. | 
|[rxHdfsFileSystem](rxhdfsfilesystem.md)  |Creates an HDFS file system object. | 
|[rxNativeFileSystem](rxnativefilesystem.md)  |Creates a native file system object. | 

<sup>*</sup> Signifies the most popular functions in this category.

<a name="data-transform-functions"></a>

## 3-Data transformation

| Function name | Description |
|---------------|-------------|
|[rxDataStep](rxdatastep.md) <sup>*</sup> |Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output) from an .xdf file or a data frame. | 
|[rxFactors](rxfactors.md) <sup>*</sup> |Recode a factor variable or convert non-factor variable into a factor in an .xdf file or data frame. | 
|[rxGetFuzzyDist](rxgetfuzzydist.md)  |Get fuzzy distances for a character vector. | 
|[rxGetFuzzyKeys](rxgetfuzzykeys.md)  |Get fuzzy keys for a character vector. | 
|[rxSplit](rxsplitxdf.md)  |Splits an .xdf file or data frame into multiple .xdf files or data frames. | 
|[rxSort](rxsortxdf.md)  |Multi-key sorting of the variables an .xdf file or data frame. | 
|[rxMerge](rxmergexdf.md)  |Merges two .xdf files or data frames using a variety of merge types. | 
|[rxExecuteSQLDDL](rxexecutesqlddl.md)  |SQL Server R Services only. Runs an arbitrary SQL DDL command. | 

<sup>*</sup> Signifies the most popular functions in this category.

<a name="graphing-functions"></a>

## 4-Basic graphing functions

| Function name | Description |
|---------------|-------------|
|[rxHistogram](rxhistogram.md)  |Creates a histogram from data. | 
|[rxLinePlot](rxlineplot.md) |Creates a line plot from data. | 
|[rxLorenz](rxlorenz.md)  |Computes a Lorenz curve which can be plotted. | 
|[rxRocCurve](rxroc.md)  |Computes and plots ROC curves from actual and predicted data. | 

<a name="statistics-functions"></a>

## 5-Descriptive statistics and cross-tabulation

| Function name | Description |
|---------------|-------------|
|[rxQuantile](rxquantile.md) <sup>*</sup> |Computes approximate quantiles for .xdf files and data frames without sorting. | 
|[rxSummary](rxsummary.md) <sup>*</sup> |Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported. | 
|[rxCrossTabs](rxcrosstabs.md) <sup>*</sup> |Formula-based cross-tabulation of data. | 
|[rxCube](rxcube.md) <sup>*</sup> |Alternative formula-based cross-tabulation designed for efficient representation returning cube results. Writing output to .xdf file not supported. | 
|[rxMarginals](rxmarginals.md)  |Marginal summaries of cross-tabulations. | 
|[as.xtabs](as.xtabs.md)  |Converts cross tabulation results to an xtabs object. | 
|[rxChiSquaredTest](rxchisquaredtest.md)  |Performs Chi-squared Test on xtabs object. Used with small data sets and does not chunk data. | 
|[rxFisherTest](rxchisquaredtest.md)  |Performs Fisher's Exact Test on xtabs object. Used with small data sets and does not chunk data. | 
|[rxKendallCor](rxchisquaredtest.md)  |Computes Kendall's Tau Rank Correlation Coefficient using xtabs object. | 
|[rxPairwiseCrossTab](rxpairwisecrosstab.md)  |Apply a function to pairwise combinations of rows and columns of an xtabs object. | 
|[rxRiskRatio](rxriskratio.md)  |Calculate the relative risk on a two-by-two xtabs object. | 
|[rxOddsRatio](rxriskratio.md)  |Calculate the odds ratio on a two-by-two xtabs object. | 

<sup>*</sup> Signifies the most popular functions in this category.

<a name="prediction-functions"></a>

## 6-Prediction functions for statistical modeling

| Function name | Description |
|---------------|-------------|
|[rxLinMod](rxlinmod.md) <sup>*</sup> |Fits a linear model to data. | 
|[rxLogit](rxlogit.md) <sup>*</sup> |Fits a logistic regression model to data. | 
|[rxGlm](rxglm.md) <sup>*</sup> |Fits a generalized linear model to data. | 
|[rxCovCor](rxcovcor.md) <sup>*</sup> |Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables. | 
|[rxDTree](rxdtree.md) <sup>*</sup> |Fits a classification or regression tree to data. | 
|[rxBTrees](rxbtrees.md) <sup>*</sup> |Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm. | 
|[rxDForest](rxdforest.md) <sup>*</sup> |Fits a classification or regression decision forest to data. | 
|[rxPredict](rxPredict.md) <sup>*</sup> |Calculates predictions for fitted models. Output must be an XDF data source. | 
|[rxKmeans](rxkmeans.md) <sup>*</sup> |Performs k-means clustering. | 
|[rxNaiveBayes](rxnaivebayes.md) <sup>*</sup> |Performs Naive Bayes classification. | 
|[rxCov](rxcovcor.md) |Calculate the covariance matrix for a set of variables. | 
|[rxCor](rxcovcor.md)  |Calculate the correlation matrix for a set of variables. | 
|[rxSSCP](rxcovcor.md)  |Calculate the sum of squares / cross-product matrix for a set of variables. | 
|[rxRoc](rxroc.md)  |Receiver Operating Characteristic (ROC) computations using actual and predicted values from binary classifier system. | 

<sup>*</sup> Signifies the most popular functions in this category.

<a name="compute"></a>

## 7-Compute context functions

| Function name | Description |
|---------------|-------------|
|[rxSetComputeContext](rxsetcomputecontext.md) |Sets a compute context. |
|[rxGetComputeContext](rxsetcomputecontext.md) |Gets the current compute context. |
|[RxHadoopMR](rxhadoopmr.md) |Creates an in-data, file-based Hadoop compute context. |
|[RxSpark](rxspark.md) |Creates an in-data, file-based Spark compute context. Computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark. |
|[RxInTeradata](rxinteradata.md) <|Creates an in-database compute context for Teradata. |
|[RxInSqlServer](rxinsqlserver.md) |Creates an in-database compute context for SQL Server. |
|[RxComputeContext](rxcomputecontext.md) |Creates a compute context. |
|[RxLocalSeq](rxlocalseq.md) |Creates a local compute context for rxExec using sequential computations. |
|[RxLocalParallel](rxlocalparallel.md) |Creates a local compute context for rxExec using the ***parallel** package as backend. |
|[RxForeachDoPar](rxforeachdopar.md) |Creates a compute context for rxExec using the current **foreach** parallel backend. |
|[rxInstalledPackages](rxinstalledpackages.md) |Returns the list of installed packages for a compute context. |
|[rxFindPackage](rxfindpackage.md) |Returns the path to one or more packages for a compute context. |

<a name="distributed-computing-functions"></a>

## 8-Distributed computing functions

These functions and many more can be used for high performance computing and distributed computing. Learn more about the entire set of functions in [Distributed Computing](../../r/how-to-revoscaler-distributed-computing.md).

| Function name | Description |
|---------------|-------------|
|[rxExec](rxexec.md) | Run an arbitrary R function on nodes or cores of a cluster.|
|[rxRngNewStream](rxrng.md) |Support for Parallel Random Number Generation. |
|[rxRngDelStream](rxrng.md) |Support for Parallel Random Number Generation. |
|[rxRngGetStream](rxrng.md) |Support for Parallel Random Number Generation.|
|[rxRngSetStream](rxrng.md) |Support for Parallel Random Number Generation. |
|[rxGetAvailableNodes](rxgetavailablenodes.md) |Get all the available nodes on a distributed compute context. |
|[rxGetNodeInfo](rxgetnodeinfo.md) | Get information on nodes specified for a distributed compute context.|
|[rxPingNodes](rxpingnodes.md) |Test round trip from user through computation node(s) in a cluster or cloud. |
|[rxGetJobStatus](rxgetjobresults.md) |Get the status of a non-waiting distributed computing job. |
|[rxGetJobResults](rxgetjobresults.md) |Get the return object(s) of a non-waiting distributed computing job. |
|[rxGetJobOutput](rxgetjoboutput.md) |Get the console output from a non-waiting distributed computing job. |
|[rxGetJobs](rxgetjobs.md) |Get the available distributed computing job information objects. |
|[rxLocateFile](rxlocatefile.md) |Get the first occurrence of a specified input file in a set of specified paths. |

<a name="utility-functions"></a>

## 9-Utility functions

Some of the utility functions are operational in local compute context only. Check the documentation of individual functions to confirm.

| Function name | Description |
|---------------|-------------|
|[rxOptions](rxoptions.md) | Gets or sets a specific option.|
|[rxGetOption](rxoptions.md) |Retrieves a specific RevoScaleR option. |
|[rxGetEnableThreadPool](rxgetenablethreadpool.md) |Gets the current state of the thread pool, which on Linux can be either persistent or on-demand. |
|[rxSetEnableThreadPool](rxgetenablethreadpool.md) |Sets the thread pool state. |
|[rxStepControl](rxstepcontrol.md) | Construct a variable.selection argument for rxLinMod.|
|[rxIsOpen](rxopen-methods.md) | Indicates whether a data source can be accessed.|
|[rxSqlServerDropTable](rxsqlserverdroptable.md) |Execute an SQL statement that drops a table. |
|[rxSqlServerTableExists](rxsqlserverdroptable.md) |Execute an SQL statement that checks for a table's existence. |
|[rxWriteNext](rxopen-methods.md) | Writes the next chunk when moving data between RevoScaleR data sources.|

## Next steps

Add R packages to your computer by running setup: 

+ [Machine Learning Server](../../install/machine-learning-server-install.md)
+ [R Client](../../r-client/what-is-microsoft-r-client.md) 

Next, follow these tutorials for hands on experience:

+ [Explore R and RevoScaleR in 25 functions](../../r/tutorial-r-to-revoscaler.md)  
+ [Quickstart: Run R Code](../../r/quickstart-run-r-code.md)

## See also

 [R Package Reference](../introducing-r-server-r-package-reference.md) 
