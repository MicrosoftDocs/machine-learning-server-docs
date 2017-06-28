--- 
 
# required metadata 
title: "RevoScaleR: Scalable Data Management and Analysis and High Performance Computing from Microsoft Corporation " 
description: "A package that provides high performance, scalable, portable, parallelized, full-featured predictive  and descriptive analytics in R. " 
keywords: "RevoScaleR, RevoScaleR-package, package" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 #`RevoScaleR-package`:  RevoScaleR: Scalable Data Management and Analysis and High Performance Computing from Microsoft Corporation 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
A package that provides high performance, scalable, portable, parallelized, full-featured predictive
 and descriptive analytics in R. Its high performance analytics (HPA) algorithms are threaded and 
 distributable and scale from small to huge data. It also provides high performance computing (HPC) 
 capabilities. 

Its HPA algorithms include descriptive statistics, cross-tabulations, linear regression, 
covariance and correlation matrices, logistic regression, generalized linear models, 
k-means clustering, classification and regression trees, and decision forests. Its HPC 
functionality allows distribution of the execution of essentially any R function across 
cores and nodes, delivering the results back to the user.

Analyses can be distributed automatically on Hadoop clusters (using distributions from Cloudera,
Hortonworks, or MapR), HDInsight clusters with Spark, in SQL Server 2016 databases, and in
Teradata data warehouses. RevoScaleR also provides support for data import, data transformations,
sorting, merging, and reading and writing to the **RevoScaleR** data file format (.xdf)
on appropriate platforms. 
 
 
 ##Details
 
| Col  1 | Col  2 |
| :---| :--- |
|  Package:  |  RevoScaleR |
|  Type:  |  Package |
|  Version:  |  9.1.0 |
|  License:  |  file LICENSE |
|  LazyLoad:  |  yes |
 
To get started, read the manual "RevoScaleR Getting Started Guide"
([`https://msdn.microsoft.com/en-us/microsoft-r/scaler-getting-started`](https://msdn.microsoft.com/en-us/microsoft-r/scaler-getting-started)
). 
The key functions/concepts in the package
are as follows (to list all public functions, type library(help="RevoScaleR")
at the R prompt):

**Input/Output**


* [rxImport](../../scaler/packagehelp/rximport.md): Creates an .xdf file or data frame from a data source (e.g. text, SAS, SPSS data files, ODBC or Teradata connection, or data frame).


* [rxDataStep](../../scaler/packagehelp/rxdatastep.md): Allows variable creation and transformations for an input data source, creating an output data source.


* [rxGetInfo](../../scaler/packagehelp/rxgetinfoxdf.md): Retrieves summary information from a data source.


* [rxSetInfo](../../scaler/packagehelp/rxsetinfo.md): Sets a file description in an .xdf file or a description attribute in a data frame.


* [rxGetVarInfo](../../scaler/packagehelp/rxgetvarinfoxdf.md): Retrieves variable information from a data source.


* [rxSetVarInfo](../../scaler/packagehelp/rxsetvarinfoxdf.md): Modifies variable information in an .xdf file or data frame.


* [rxGetVarNames](../../scaler/packagehelp/rxgetvarnames.md): Retrieves variable names from a data source


* [rxCreateColInfo](../../scaler/packagehelp/rxcreatecolinfo.md): Generates a 'colInfo' list from a data source.


* [rxCompressXdf](../../scaler/packagehelp/rxcompressxdf.md): Compresses an existing .xdf file, or a directory of .xdf files.


* [RxXdfData](../../scaler/packagehelp/rxxdfdata.md): Creates an .xdf data source object.


* [RxTextData](../../scaler/packagehelp/rxtextdata.md): Creates a text data source object.


* 
[RxSasData](../../scaler/packagehelp/rxsasdata.md): Creates a SAS data source object.

* 
[RxSpssData](../../scaler/packagehelp/rxspssdata.md): Creates an SPSS data source object.

* 
[RxOdbcData](../../scaler/packagehelp/rxodbcdata.md): Creates an ODBC data source object.

* 
[RxTeradata](../../scaler/packagehelp/rxteradata.md): Creates a Teradata data source object.

* 
[RxSqlServerData](../../scaler/packagehelp/rxsqlserverdata.md): Creates a SQL Server data source object.

* [rxOpen](../../scaler/packagehelp/rxopen-methods.md): Open a data source for reading.


* [rxReadNext](../../scaler/packagehelp/rxopen-methods.md): Read data from a data source.


* [rxWriteNext](../../scaler/packagehelp/rxopen-methods.md): Write data to a data source. EXPERIMENTAL: for text and .xdf only.


* [rxClose](../../scaler/packagehelp/rxopen-methods.md): Close a data source.


* [rxSetFileSystem](../../scaler/packagehelp/rxsetfilesystem.md): Specify a file system type for data for import.
 

* [rxGetFileSystem](../../scaler/packagehelp/rxsetfilesystem.md): Retrieve the current file system type.


* [RxHdfsFileSystem](../../scaler/packagehelp/rxhdfsfilesystem.md): Creates an HDFS file system object.


* [RxNativeFileSystem](../../scaler/packagehelp/rxnativefilesystem.md): Creates a native file system object.




**Data Manipulations/Data Step**


* [rxDataStep](../../scaler/packagehelp/rxdatastep.md): Transform and subset data.


* [rxGetFuzzyDist](../../scaler/packagehelp/rxgetfuzzydist.md): Get fuzzy distances for a character vector.


* [rxGetFuzzyKeys](../../scaler/packagehelp/rxgetfuzzykeys.md): Get fuzzy keys for a character vector.


* [rxFactors](../../scaler/packagehelp/rxfactors.md): Recode a factor variable or convert non-factor variable into a factor in an  .xdf file or data frame.


* [rxSort](../../scaler/packagehelp/rxsortxdf.md): Multi-key sorting of the variables an .xdf file or data frame.


* [rxMerge](../../scaler/packagehelp/rxmergexdf.md): Merges two .xdf files or data frames using a variety of merge types.


* [rxSplit](../../scaler/packagehelp/rxsplitxdf.md): Splits an .xdf file or data frame into multiple .xdf files or data frames.




**Descriptive Statistics and Cross Tabs**


* [rxSummary](../../scaler/packagehelp/rxsummary.md): Basic summary statistics of data, including computations by group.


* [rxQuantile](../../scaler/packagehelp/rxquantile.md): Computes approximate quantiles for .xdf files and data frames without sorting.


* [rxCrossTabs](../../scaler/packagehelp/rxcrosstabs.md): Formula-based cross-tabulation of data.


* [rxCube](../../scaler/packagehelp/rxcube.md): Alternative formula-based cross-tabulation designed for efficient representation.


* [rxMarginals](../../scaler/packagehelp/rxmarginals.md): Marginal summaries of cross-tabulations.


* [as.xtabs](as-xtabs.md): Converts cross tabulation results to an `xtabs` object.


* [rxChiSquaredTest](../../scaler/packagehelp/rxchisquaredtest.md): Performs Chi-squared Test on `xtabs` object.


* [rxFisherTest](../../scaler/packagehelp/rxchisquaredtest.md): Performs Fisher's Exact Test on `xtabs` object.


* [rxKendallCor](../../scaler/packagehelp/rxchisquaredtest.md): Computes Kendall's Tau Rank Correlation Coefficient using `xtabs` object.


* [rxPairwiseCrossTab](../../scaler/packagehelp/rxpairwisecrosstab.md): Apply a function to pairwise combinations of rows and columns of an `xtabs` object.


* [rxRiskRatio](../../scaler/packagehelp/rxriskratio.md): Calculate the relative risk on a two-by-two `xtabs` object.
 

* [rxOddsRatio](../../scaler/packagehelp/rxriskratio.md): Calculate the odds ratio on a two-by-two `xtabs` object.
 



**Statistical Modeling**


* [rxLinMod](../../scaler/packagehelp/rxlinmod.md): Fits a linear model to data.


* [rxCovCor](../../scaler/packagehelp/rxcovcor.md): Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.


* [rxCov](../../scaler/packagehelp/rxcovcor.md): Calculate the covariance matrix for a set of variables.


* [rxCor](../../scaler/packagehelp/rxcovcor.md): Calculate the correlation matrix for a set of variables


* [rxSSCP](../../scaler/packagehelp/rxcovcor.md): Calculate the sum of squares / cross-product matrix for a set of variables.
 

* [rxLogit](../../scaler/packagehelp/rxlogit.md): Fits a logistic regression model to data.


* [rxRoc](../../scaler/packagehelp/rxroc.md): Receiver Operating Characteristic (ROC) computations using actual and predicted values from binary classifier system.


* [rxGlm](../../scaler/packagehelp/rxglm.md): Fits a generalized linear model to data.


* [rxDTree](rxdtree.md):  Fits a classification or regression tree to data.


* [rxDForest](../../scaler/packagehelp/rxdforest.md):  Fits a classification or regression decision forest to data.


* [rxBTrees](../../scaler/packagehelp/rxbtrees.md):  Fits a classification or regression decision forest via stochastic gradient boosting.


* [rxPredict](../microsoftml/rxpredict.md): Calculates predictions for fitted models.


* [rxKmeans](../../scaler/packagehelp/rxkmeans.md): Performs k-means clustering.


* [rxNaiveBayes](../../scaler/packagehelp/rxnaivebayes.md): Performs Naive Bayes classification.


* [as.lm](as-lm.md): Coerce an `rxLinMod` object to an `lm` object for use with the **pmml** package.


* [as.glm](as-glm.md): Coerce an `rxLogit` or `rxGlm` object to a `glm` object for use with the **pmml** package.


* [as.rpart](as-rpart.md): Coerce an `rxDTree` object to an `rpart` object for use with the **pmml** package.


* [as.kmeans](as-kmeans.md): Coerce an `rxKmeans` object to an `kmeans` object for use with the **pmml** package.


* [as.randomForest](as-randomforest.md): Coerce an `rxDForest` object to a `randomForest` object for use with the **pmml** package.


* [as.gbm](as-gbm.md): Coerce an `rxBTrees` object to a `gbm` object for use with the **pmml** package.




**Utility Functions**


* [rxOptions](../../scaler/packagehelp/rxoptions.md): Gets or sets **RevoScaleR**-specific options.


* [rxGetOption](../../scaler/packagehelp/rxoptions.md): Retrieves a specific **RevoScaleR** option.


* [rxRngNewStream](../../scaler/packagehelp/rxrng.md) and related functions: Support for Parallel Random Number Generation.


* [rxGetEnableThreadPool](../../scaler/packagehelp/rxgetenablethreadpool.md): Gets the current state of the thread pool, which on Linux can be either persistent or on-demand.


* [rxSetEnableThreadPool](../../scaler/packagehelp/rxgetenablethreadpool.md): Sets the thread pool state.


* [rxStepControl](../../scaler/packagehelp/rxstepcontrol.md): Construct `variable.selection` argument for `rxLinMod`.




**Basic Graphing Functions**


* [rxHistogram](../../scaler/packagehelp/rxhistogram.md): Creates a histogram from data.


* [rxLinePlot](../../scaler/packagehelp/rxlineplot.md): Creates a line plot from data.
 

* [rxLorenz](../../scaler/packagehelp/rxlorenz.md): Computes a Lorenz curve which can be plotted.


* [rxRocCurve](../../scaler/packagehelp/rxroc.md): Computes and plots ROC curves from actual and predicted data.




**HPC and Distributed Computing**


* [RxComputeContext](../../scaler/packagehelp/rxcomputecontext.md): Creates a compute context.


* [RxHadoopMR](../../scaler/packagehelp/rxhadoopmr.md): Creates an in-data, file-based Hadoop compute context.


* [RxSpark](../../scaler/packagehelp/rxspark.md): Creates an in-data, file-based Spark compute context.


* [RxInTeradata](../../scaler/packagehelp/rxinteradata.md): Creates an in-database compute context for Teradata.


* [RxInSqlServer](../../scaler/packagehelp/rxinsqlserver.md): Creates an in-database compute context for SQL Server 2016.


* [RxForeachDoPar](../../scaler/packagehelp/rxforeachdopar.md): Creates a compute context for [rxExec](../../scaler/packagehelp/rxexec.md) using the current `foreach` parallel back end.


* [RxLocalParallel](../../scaler/packagehelp/rxlocalparallel.md): Creates a local compute context for [rxExec](../../scaler/packagehelp/rxexec.md) using the **parallel** package as back end.


* [RxLocalSeq](../../scaler/packagehelp/rxlocalseq.md): Creates a local compute context for [rxExec](../../scaler/packagehelp/rxexec.md) using sequential computations.


* [rxSetComputeContext](../../scaler/packagehelp/rxsetcomputecontext.md): Sets a compute context.


* [rxGetComputeContext](../../scaler/packagehelp/rxsetcomputecontext.md): Gets the current compute context.


* [rxGetAvailableNodes](../../scaler/packagehelp/rxgetavailablenodes.md): Get all the available nodes on a distributed compute context.


* [rxGetNodeInfo](../../scaler/packagehelp/rxgetnodeinfo.md): Get information on nodes specified for a distributed compute context.


* [rxPingNodes](../../scaler/packagehelp/rxpingnodes.md): Test round trip from end user through computation node(s) in a cluster or cloud.


* [rxExec](../../scaler/packagehelp/rxexec.md): Run an arbitrary R function on nodes or cores of a cluster.


* [rxGetJobStatus](../../scaler/packagehelp/rxgetjobresults.md): Get the status of a non-waiting distributed computing job.


* [rxGetJobResults](../../scaler/packagehelp/rxgetjobresults.md): Get the return object(s) of a non-waiting distributed computing job.


* [rxGetJobOutput](../../scaler/packagehelp/rxgetjoboutput.md): Get the console output from a non-waiting distributed computing job.


* [rxGetJobs](../../scaler/packagehelp/rxgetjobs.md): Get the available distributed computing job information objects.


* [rxLocateFile](../../scaler/packagehelp/rxlocatefile.md): Get the first occurrence of a specified input file in a set of specified paths.




**Package Management Functions**


* [rxInstalledPackages](../../scaler/packagehelp/rxinstalledpackages.md): Find (or retrieve) details of installed packages for a compute context.
,

* [rxInstallPackages](../../scaler/packagehelp/rxinstallpackages.md): Install Packages from Repositories or Local Files for a compute context.
,   

* [rxRemovePackages](../../scaler/packagehelp/rxremovepackages.md): Removes installed packages from a compute context.
,

* [rxFindPackage](../../scaler/packagehelp/rxfindpackage.md): Find the path for one or more packages for a compute context.
,    

* [rxSqlLibPaths](../../scaler/packagehelp/rxsqllibpaths.md): Gets the search path for the library trees for packages while executing inside the SQL server.
,



**Hadoop Convenience Functions**


* [rxHadoopCommand](../../scaler/packagehelp/rxhadoopcommand.md): Execute an arbitrary Hadoop command.


* [rxHadoopCopy](../../scaler/packagehelp/rxhadoopcommand.md): Copy a file in the Hadoop Distributed File System (HDFS).


* [rxHadoopCopyFromLocal](../../scaler/packagehelp/rxhadoopcommand.md): Copy a file from the native file system to HDFS.


* [rxHadoopCopyFromClient](../../scaler/packagehelp/rxhadoopcommand.md): Copy a file from a remote client to the Hadoop cluster's local file system, and then to HDFS.


* [rxHadoopCopyToLocal](../../scaler/packagehelp/rxhadoopcommand.md): Copy a file from HDFS to the local file system.


* [rxHadoopListFiles](../../scaler/packagehelp/rxhadoopcommand.md): List files in an HDFS directory.


* [rxHadoopMakeDir](../../scaler/packagehelp/rxhadoopcommand.md): Make a directory in HDFS.


* [rxHadoopMove](../../scaler/packagehelp/rxhadoopcommand.md): Move a file in HDFS.


* [rxHadoopRemove](../../scaler/packagehelp/rxhadoopcommand.md): Remove a file in HDFS.


* [rxHadoopRemoveDir](../../scaler/packagehelp/rxhadoopcommand.md): Remove a directory in HDFS.


* [rxHadoopVersion](../../scaler/packagehelp/rxhadoopcommand.md): Return the current Hadoop version.




 
 
 ##See Also
 [rxFormula](../../scaler/packagehelp/rxformula.md), [rxTransform](../../scaler/packagehelp/rxtransform.md), [rxPackage](../../scaler/packagehelp/rxpackage.md)   
 ##Author(s)
 Microsoft Corporation 
 
 
