---

# required metadata
title: "RevoScaleR package for R | Microsoft Docs"
description: "Function help reference for the RevoScaleR R package of Microsoft R"
keywords: "RevoScaleR, ScaleR"
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "08/22/2016"
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

# RevoScaleR package for R

The **RevoScaleR** library provides a set of over one hundred portable, scalable, and distributable data analysis functions.

| Package details | |
|--------|-|
| Version: |  9.1.0 |
| Supported on: | [Microsoft R Client (Windows and Linux)](../../r-client/what-is-microsoft-r-client.md) <br/>[Microsoft R Server (all platforms)](../../what-is-microsoft-r-server.md)   <br/>[SQL Server 2016 and later (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) |
| Built on: | R 3.3.x (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|


## How to use RevoScaleR

This package is included in installations of...   It is often preloaded into tools that integrate with R Server, which means you can call functions without having to load the library. Run the following commands in the order shown below to load the library, get the version, and run one of its signature commands.

In an R session, load **RevoScaleR** from the command line by typing`library(RevoScaleR)`.

### Scope

While most of these functions are of general application, some are specific to particular compute contexts and some may not be fully supported in all compute contexts. If you are looking for the functions optimized for Hadoop, Teradata, or SQL Server compute contexts, see the relevant function list for that context:
+ [Computing on a Hadoop Cluster](revoscaler-hadoop-functions.md)
+ [Computing on a Teradata Datawarehouse](revoscaler-teradata-functions.md)
+ [Computing on SQL Server](https://msdn.microsoft.com/en-us/library/mt652103.aspx)

> [!Note]
> Some function names begin with `rx` and others with `Rx`. The `Rx` function name prefix is used to distinguish the class constructors such as data sources and compute contexts.

## Data Analysis Functions
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

<br />
####Import and Export Functions

| Function name | Description |
|---------------|-------------|
|[rxImport](rximport.md) <sup>*</sup> |Creates an .xdf file or data frame from a data source (e.g. text, SAS, SPSS data files, ODBC or Teradata connection, or data frame). | 
|[rxDataStep](rxdatastep.md) <sup>*</sup> |Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from an .xdf file or a data frame. | 
|[rxGetInfo](rxgetinfoxdf.md) <sup>*</sup> |Retrieves summary information from a data source or data frame. | 
|[rxSetInfo](rxsetinfo.md) <sup>*</sup> |Sets a file description in an .xdf file or a description attribute in a data frame. | 
|[rxGetVarInfo](rxgetvarinfoxdf.md) |Retrieves variable information from a data source or data frame. | 
|[rxSetVarInfo](rxsetvarinfoxdf.md)  |Modifies variable information in an .xdf file or data frame. | 
|[rxGetVarNames](rxgetvarnames.md)  |Retrieves variable names from a data source or data frame. | 
|[rxCreateColInfo](rxcreatecolinfo.md)  |Generates a colInfo list from a data source. | 
|[rxCompressXdf](rxcompressxdf.md)  |Compresses an existing .xdf file, or a directory of .xdf files. | 
|[RxXdfData](rxxdfdata.md)  |Creates an efficient XDF data source object. | 
|[RxTextData](rxtextdata.md)  |Creates a comma delimited text data source object. | 
|[RxSasData](rxsasdata.md)  |Creates a SAS data source object. | 
|[RxSpssData](rxspssdata.md)  |Creates a SPSS data source object. | 
|[RxOdbcData](rxodbcdata.md)  |Creates a ODBC data source object. | 
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

####Manipulation, Cleansing, and Transformation Functions

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

####Visualization Functions

| Function name | Description |
|---------------|-------------|
|[rxHistogram](rxhistogram.md) <sup>*</sup> |Creates a histogram from data. | 
|[rxLinePlot](rxlineplot.md) <sup>*</sup> |Creates a line plot from data. | 
|[rxLorenz](rxlorenz.md)  |Computes a Lorenz curve which can be plotted. | 
|[rxRocCurve](rxroc.md)  |Computes and plots ROC curves from actual and predicted data. | 

<sup>*</sup> Signifies the most popular functions in this category.

####Analysis Functions for Descriptive Statistics and Cross-Tabulations

| Function name | Description |
|---------------|-------------|
|[rxQuantile](rxquantile.md) <sup>*</sup> |Computes approximate quantiles for .xdf files and data frames without sorting. | 
|[rxSummary](rxsummary.md) <sup>*</sup> |Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported. | 
|[rxCrossTabs](rxcrosstabs.md) <sup>*</sup> |Formula-based cross-tabulation of data. | 
|[rxCube](rxcube.md) <sup>*</sup> |Alternative formula-based cross-tabulation designed for efficient representation returning cube results. Writing output to .xdf file not supported. | 
|[rxMarginals](rxmarginals.md)  |Marginal summaries of cross-tabulations. | 
|[as.xtabs](as-xtabs.md)  |Converts cross tabulation results to an xtabs object. | 
|[rxChiSquaredTest](rxchisquaredtest.md)  |Performs Chi-squared Test on xtabs object. Used with small data sets and does not chunk data. | 
|[rxFisherTest](rxchisquaredtest.md)  |Performs Fisher's Exact Test on xtabs object. Used with small data sets and does not chunk data. | 
|[rxKendallCor](rxchisquaredtest.md)  |Computes Kendall's Tau Rank Correlation Coefficient using xtabs object. | 
|[rxPairwiseCrossTab](rxpairwisecrosstab.md)  |Apply a function to pairwise combinations of rows and columns of an xtabs object. | 
|[rxRiskRatio](rxriskratio.md)  |Calculate the relative risk on a two-by-two xtabs object. | 
|[rxOddsRatio](rxriskratio.md)  |Calculate the odds ratio on a two-by-two xtabs object. | 

<sup>*</sup> Signifies the most popular functions in this category.

####Analysis, Learning, and Prediction Functions for Statistical Modeling

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

##Compute Context Functions

| Function name | Description |
|---------------|-------------|
|[rxSetComputeContext](rxsetcomputecontext.md) <sup>*</sup>|Sets a compute context. |
|[rxGetComputeContext](rxsetcomputecontext.md) <sup>*</sup>|Gets the current compute context. |
|[RxHadoopMR](rxhadoopmr.md) <sup>*</sup>|Creates an in-data, file-based Hadoop compute context. |
|[RxSpark](rxspark.md) <sup>*</sup>|Creates an in-data, file-based Spark compute context. Computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark. |
|[RxInTeradata](rxinteradata.md) <sup>*</sup>|Creates an in-database compute context for Teradata. |
|[RxInSqlServertd](rxinsqlserver.md) |Creates an in-database compute context for SQL Server. |
|[RxComputeContext](rxcomputecontext.md) |Creates a compute context. |
|[RxLocalSeq](rxlocalseq.md) |Creates a local compute context for rxExec using sequential computations. |
|[RxLocalParallel](rxlocalparallel.md) |Creates a local compute context for rxExec using the ***parallel** package as backend. |
|[RxForeachDoPar](rxforeachdopar.md) |Creates a compute context for rxExec using the current **foreach** parallel backend. |
|[rxInstalledPackages](rxinstalledpackages.md) |Returns the list of installed packages for a compute context. |
|[rxFindPackage](rxfindpackage.md) |Returns the path to one or more packages for a compute context. |

<sup>*</sup> Signifies the most popular functions in this category.

<a name="data"></a>

##Data Source Functions

| Function name | Description |
|---------------|-------------|
|[RxXdfData](rxxdfdata.md) |Creates an efficient XDF data source object. |
|[RxTextData](rxtextdata.md) |Creates a comma delimited text data source object. |
|[RxSasData](rxsasdata.md) |Creates a SAS data source object. |
|[RxSpssData](rxspssdata.md) |Creates a SPSS data source object. |
|[RxOdbcData](rxodbcdata.md) |Creates a ODBC data source object. |
|[RxTeradata](rxteradata.md) |Creates a Teradata data source object. |
|[RxSqlServerData](rxsqlserverdata.md) |Creates a SQL Server data source object. |

##HPC and Distributed Computing Functions

These functions and many more can be used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../../r/how-to-revoscaler-distributed-computing.md).

| Function name | Description |
|---------------|-------------|
|[rxExec](rxexec.md) | Run an arbitrary R function on nodes or cores of a cluster.|
|[rxRngNewStream]((rxrng.md) |Support for Parallel Random Number Generation. |
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

##Utility Functions

Some of the utility functions are operational in local compute context only. Check the documentation of individual functions to confirm.

| Function name | Description |
|---------------|-------------|
|[rxOptions](rxoptions.md) <sup>*</sup>| Gets or sets a specific option.|
|[rxGetOption](rxoptions.md) <sup>*</sup>|Retrieves a specific RevoScaleR option. |
|[rxGetEnableThreadPool](rxgetenablethreadpool.md) |Gets the current state of the thread pool, which on Linux can be either persistent or on-demand. |
|[rxSetEnableThreadPool](rxgetenablethreadpool.md) |Sets the thread pool state. |
|[rxStepControl](rxstepcontrol.md) | Construct a variable.selection argument for rxLinMod.|
|[rxIsOpen](rxopen-methods.md) | Indicates whether a data source can be accessed.|
|[rxSqlServerDropTable](rxsqlserverdroptable.md) |Execute an SQL statement that drops a table. |
|[rxSqlServerTableExists](rxsqlserverdroptable.md) |Execute an SQL statement that checks for a table's existance. |
|[rxWriteNext](rxopen-methods.md) | Writes the next chunk when moving data between ScaleR data sources.|

<sup>*</sup> Signifies the most popular functions in this category.

## Next steps

Add R packages to your computer by running setup for R Server or R Client: 

+ [R Client](../../r-client/what-is-microsoft-r-client.md) 
+ [R Server](../../what-is-microsoft-r-server.md)

Next, follow these tutorials for hands on experience::

+ [Explore R and RevoScaleR in 25 functions](../../r/tutorial-r-to-revoscaler.md)  
+ [Quickstart: Run R Code in Microsoft R](../../r/quickstart-run-r-code.md)

## See also

 [R Package Reference](../introducing-r-server-r-package-reference.md) 
