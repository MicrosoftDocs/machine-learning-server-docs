--- 
 
# required metadata 
title: "RevoScaleR: Scalable Data Management and Analysis and High Performance Computing from Microsoft Corporation " 
description: "A package that provides high performance, scalable, portable, parallelized, full-featured predictive  and descriptive analytics in R. " 
keywords: "RevoScaleR, RevoScaleR-package, package" 
author: "richcalaway"
ms.author: "richcala" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 #RevoScaleR-package:  RevoScaleR: Scalable Data Management and Analysis and High Performance Computing from Microsoft Corporation 

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

## Input/Output


* [rxImport](rximport.md): Creates an .xdf file or data frame from a data source (e.g. text, SAS, SPSS data files, ODBC or Teradata connection, or data frame).


* [rxDataStep](rxdatastep.md): Allows variable creation and transformations for an input data source, creating an output data source.


* [rxGetInfo](rxgetinfoxdf.md): Retrieves summary information from a data source.


* [rxSetInfo](rxsetinfo.md): Sets a file description in an .xdf file or a description attribute in a data frame.


* [rxGetVarInfo](rxgetvarinfoxdf.md): Retrieves variable information from a data source.


* [rxSetVarInfo](rxsetvarinfoxdf.md): Modifies variable information in an .xdf file or data frame.


* [rxGetVarNames](rxgetvarnames.md): Retrieves variable names from a data source


* [rxCreateColInfo](rxcreatecolinfo.md): Generates a 'colInfo' list from a data source.


* [rxCompressXdf](rxcompressxdf.md): Compresses an existing .xdf file, or a directory of .xdf files.


* [RxXdfData](rxxdfdata.md): Creates an .xdf data source object.


* [RxTextData](rxtextdata.md): Creates a text data source object.


* 
[RxSasData](rxsasdata.md): Creates a SAS data source object.

* 
[RxSpssData](rxspssdata.md): Creates an SPSS data source object.

* 
[RxOdbcData](rxodbcdata.md): Creates an ODBC data source object.

* 
[RxTeradata](rxteradata.md): Creates a Teradata data source object.

* 
[RxSqlServerData](rxsqlserverdata.md): Creates a SQL Server data source object.

* [rxOpen](rxopen-methods.md): Open a data source for reading.


* [rxReadNext](rxopen-methods.md): Read data from a data source.


* [rxWriteNext](rxopen-methods.md): Write data to a data source. EXPERIMENTAL: for text and .xdf only.


* [rxClose](rxopen-methods.md): Close a data source.


* [rxSetFileSystem](rxsetfilesystem.md): Specify a file system type for data for import.
 

* [rxGetFileSystem](rxsetfilesystem.md): Retrieve the current file system type.


* [RxHdfsFileSystem](rxhdfsfilesystem.md): Creates an HDFS file system object.


* [RxNativeFileSystem](rxnativefilesystem.md): Creates a native file system object.




## Data Manipulations/Data Step 


* [rxDataStep](rxdatastep.md): Transform and subset data.


* [rxGetFuzzyDist](rxgetfuzzydist.md): Get fuzzy distances for a character vector.


* [rxGetFuzzyKeys](rxgetfuzzykeys.md): Get fuzzy keys for a character vector.


* [rxFactors](rxfactors.md): Recode a factor variable or convert non-factor variable into a factor in an  .xdf file or data frame.


* [rxSort](rxsortxdf.md): Multi-key sorting of the variables an .xdf file or data frame.


* [rxMerge](rxmergexdf.md): Merges two .xdf files or data frames using a variety of merge types.


* [rxSplit](rxsplitxdf.md): Splits an .xdf file or data frame into multiple .xdf files or data frames.




## Descriptive Statistics and Cross Tabs


* [rxSummary](rxsummary.md): Basic summary statistics of data, including computations by group.


* [rxQuantile](rxquantile.md): Computes approximate quantiles for .xdf files and data frames without sorting.


* [rxCrossTabs](rxcrosstabs.md): Formula-based cross-tabulation of data.


* [rxCube](rxcube.md): Alternative formula-based cross-tabulation designed for efficient representation.


* [rxMarginals](rxmarginals.md): Marginal summaries of cross-tabulations.


* [as.xtabs](as-xtabs.md): Converts cross tabulation results to an `xtabs` object.


* [rxChiSquaredTest](rxchisquaredtest.md): Performs Chi-squared Test on `xtabs` object.


* [rxFisherTest](rxchisquaredtest.md): Performs Fisher's Exact Test on `xtabs` object.


* [rxKendallCor](rxchisquaredtest.md): Computes Kendall's Tau Rank Correlation Coefficient using `xtabs` object.


* [rxPairwiseCrossTab](rxpairwisecrosstab.md): Apply a function to pairwise combinations of rows and columns of an `xtabs` object.


* [rxRiskRatio](rxriskratio.md): Calculate the relative risk on a two-by-two `xtabs` object.
 

* [rxOddsRatio](rxriskratio.md): Calculate the odds ratio on a two-by-two `xtabs` object.
 



## Statistical Modeling 


* [rxLinMod](rxlinmod.md): Fits a linear model to data.


* [rxCovCor](rxcovcor.md): Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.


* [rxCov](rxcovcor.md): Calculate the covariance matrix for a set of variables.


* [rxCor](rxcovcor.md): Calculate the correlation matrix for a set of variables


* [rxSSCP](rxcovcor.md): Calculate the sum of squares / cross-product matrix for a set of variables.
 

* [rxLogit](rxlogit.md): Fits a logistic regression model to data.


* [rxRoc](rxroc.md): Receiver Operating Characteristic (ROC) computations using actual and predicted values from binary classifier system.


* [rxGlm](rxglm.md): Fits a generalized linear model to data.


* [rxDTree](rxdtree.md):  Fits a classification or regression tree to data.


* [rxDForest](rxdforest.md):  Fits a classification or regression decision forest to data.


* [rxBTrees](rxbtrees.md):  Fits a classification or regression decision forest via stochastic gradient boosting.


* [rxPredict](../microsoftml/rxpredict.md): Calculates predictions for fitted models.


* [rxKmeans](rxkmeans.md): Performs k-means clustering.


* [rxNaiveBayes](rxnaivebayes.md): Performs Naive Bayes classification.


* [as.lm](as-lm.md): Coerce an `rxLinMod` object to an `lm` object for use with the **pmml** package.


* [as.glm](as-glm.md): Coerce an `rxLogit` or `rxGlm` object to a `glm` object for use with the **pmml** package.


* [as.rpart](as-rpart.md): Coerce an `rxDTree` object to an `rpart` object for use with the **pmml** package.


* [as.kmeans](as-kmeans.md): Coerce an `rxKmeans` object to an `kmeans` object for use with the **pmml** package.


* [as.randomForest](as-randomforest.md): Coerce an `rxDForest` object to a `randomForest` object for use with the **pmml** package.


* [as.gbm](as-gbm.md): Coerce an `rxBTrees` object to a `gbm` object for use with the **pmml** package.




## Utility Functions 


* [rxOptions](rxoptions.md): Gets or sets **RevoScaleR**-specific options.


* [rxGetOption](rxoptions.md): Retrieves a specific **RevoScaleR** option.


* [rxRngNewStream](rxrng.md) and related functions: Support for Parallel Random Number Generation.


* [rxGetEnableThreadPool](rxgetenablethreadpool.md): Gets the current state of the thread pool, which on Linux can be either persistent or on-demand.


* [rxSetEnableThreadPool](rxgetenablethreadpool.md): Sets the thread pool state.


* [rxStepControl](rxstepcontrol.md): Construct `variable.selection` argument for `rxLinMod`.




## Basic Graphing Functions 


* [rxHistogram](rxhistogram.md): Creates a histogram from data.


* [rxLinePlot](rxlineplot.md): Creates a line plot from data.
 

* [rxLorenz](rxlorenz.md): Computes a Lorenz curve which can be plotted.


* [rxRocCurve](rxroc.md): Computes and plots ROC curves from actual and predicted data.




## HPC and Distributed Computing 


* [RxComputeContext](rxcomputecontext.md): Creates a compute context.


* [RxHadoopMR](rxhadoopmr.md): Creates an in-data, file-based Hadoop compute context.


* [RxSpark](rxspark.md): Creates an in-data, file-based Spark compute context.


* [RxInTeradata](rxinteradata.md): Creates an in-database compute context for Teradata.


* [RxInSqlServer](rxinsqlserver.md): Creates an in-database compute context for SQL Server 2016.


* [RxForeachDoPar](rxforeachdopar.md): Creates a compute context for [rxExec](rxexec.md) using the current `foreach` parallel back end.


* [RxLocalParallel](rxlocalparallel.md): Creates a local compute context for [rxExec](rxexec.md) using the **parallel** package as back end.


* [RxLocalSeq](rxlocalseq.md): Creates a local compute context for [rxExec](rxexec.md) using sequential computations.


* [rxSetComputeContext](rxsetcomputecontext.md): Sets a compute context.


* [rxGetComputeContext](rxsetcomputecontext.md): Gets the current compute context.


* [rxGetAvailableNodes](rxgetavailablenodes.md): Get all the available nodes on a distributed compute context.


* [rxGetNodeInfo](rxgetnodeinfo.md): Get information on nodes specified for a distributed compute context.


* [rxPingNodes](rxpingnodes.md): Test round trip from end user through computation node(s) in a cluster or cloud.


* [rxExec](rxexec.md): Run an arbitrary R function on nodes or cores of a cluster.


* [rxGetJobStatus](rxgetjobresults.md): Get the status of a non-waiting distributed computing job.


* [rxGetJobResults](rxgetjobresults.md): Get the return object(s) of a non-waiting distributed computing job.


* [rxGetJobOutput](rxgetjoboutput.md): Get the console output from a non-waiting distributed computing job.


* [rxGetJobs](rxgetjobs.md): Get the available distributed computing job information objects.


* [rxLocateFile](rxlocatefile.md): Get the first occurrence of a specified input file in a set of specified paths.




## Package Management Functions 


* [rxInstalledPackages](rxinstalledpackages.md): Find (or retrieve) details of installed packages for a compute context.
,

* [rxInstallPackages](rxinstallpackages.md): Install Packages from Repositories or Local Files for a compute context.
,   

* [rxRemovePackages](rxremovepackages.md): Removes installed packages from a compute context.
,

* [rxFindPackage](rxfindpackage.md): Find the path for one or more packages for a compute context.
,    

* [rxSqlLibPaths](rxsqllibpaths.md): Gets the search path for the library trees for packages while executing inside the SQL server.
,


## Hadoop Convenience Functions


* [rxHadoopCommand](rxhadoopcommand.md): Execute an arbitrary Hadoop command.


* [rxHadoopCopy](rxhadoopcommand.md): Copy a file in the Hadoop Distributed File System (HDFS).


* [rxHadoopCopyFromLocal](rxhadoopcommand.md): Copy a file from the native file system to HDFS.


* [rxHadoopCopyFromClient](rxhadoopcommand.md): Copy a file from a remote client to the Hadoop cluster's local file system, and then to HDFS.


* [rxHadoopCopyToLocal](rxhadoopcommand.md): Copy a file from HDFS to the local file system.


* [rxHadoopListFiles](rxhadoopcommand.md): List files in an HDFS directory.


* [rxHadoopMakeDir](rxhadoopcommand.md): Make a directory in HDFS.


* [rxHadoopMove](rxhadoopcommand.md): Move a file in HDFS.


* [rxHadoopRemove](rxhadoopcommand.md): Remove a file in HDFS.


* [rxHadoopRemoveDir](rxhadoopcommand.md): Remove a directory in HDFS.


* [rxHadoopVersion](rxhadoopcommand.md): Return the current Hadoop version.




 
 
 ##See Also
 [rxFormula](rxformula.md), [rxTransform](rxtransform.md), [rxPackage](rxpackage.md)   
 ##Author(s)
 Microsoft Corporation 
 
 
