--- 
 
# required metadata 
title: " RevoScaleR: Scalable Data Management and Analysis and High Performance Computing from Microsoft Corporation " 
description: "A package that provides high performance, scalable, portable, parallelized, full-featured predictive  and descriptive analytics in R. " 
keywords: "RevoScaleR, RevoScaleR-package, package" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
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
|  Version:  |  PRODUCT_VERSION |
|  License:  |  file LICENSE |
|  LazyLoad:  |  yes |
 
To get started, read the manual "RevoScaleR Getting Started Guide"
([`https://msdn.microsoft.com/en-us/microsoft-r/scaler-getting-started`](https://msdn.microsoft.com/en-us/microsoft-r/scaler-getting-started)
). 
The key functions/concepts in the package
are as follows (to list all public functions, type library(help="RevoScaleR")
at the R prompt):

**Input/Output**


* [rxImport](rxImport.md): Creates an .xdf file or data frame from a data source (e.g. text, SAS, SPSS data files, ODBC or Teradata connection, or data frame).


* [rxDataStep](rxDataStep.md): Allows variable creation and transformations for an input data source, creating an output data source.


* [rxGetInfo](rxGetInfoXdf.md): Retrieves summary information from a data source.


* [rxSetInfo](rxSetInfo.md): Sets a file description in an .xdf file or a description attribute in a data frame.


* [rxGetVarInfo](rxGetVarInfoXdf.md): Retrieves variable information from a data source.


* [rxSetVarInfo](rxSetVarInfoXdf.md): Modifies variable information in an .xdf file or data frame.


* [rxGetVarNames](rxGetVarNames.md): Retrieves variable names from a data source


* [rxCreateColInfo](rxCreateColInfo.md): Generates a 'colInfo' list from a data source.


* [rxCompressXdf](rxCompressXdf.md): Compresses an existing .xdf file, or a directory of .xdf files.


* [RxXdfData](RxXdfData.md): Creates an .xdf data source object.


* [RxTextData](RxTextData.md): Creates a text data source object.


* 
[RxSasData](RxSasData.md): Creates a SAS data source object.

* 
[RxSpssData](RxSpssData.md): Creates an SPSS data source object.

* 
[RxOdbcData](RxOdbcData.md): Creates an ODBC data source object.

* 
[RxTeradata](RxTeradata.md): Creates a Teradata data source object.

* 
[RxSqlServerData](RxSqlServerData.md): Creates a SQL Server data source object.

* [rxOpen](rxOpen-methods.md): Open a data source for reading.


* [rxReadNext](rxOpen-methods.md): Read data from a data source.


* [rxWriteNext](rxOpen-methods.md): Write data to a data source. EXPERIMENTAL: for text and .xdf only.


* [rxClose](rxOpen-methods.md): Close a data source.


* [rxSetFileSystem](rxSetFileSystem.md): Specify a file system type for data for import.
 

* [rxGetFileSystem](rxSetFileSystem.md): Retrieve the current file system type.


* [RxHdfsFileSystem](RxHdfsFileSystem.md): Creates an HDFS file system object.


* [RxNativeFileSystem](RxNativeFileSystem.md): Creates a native file system object.




**Data Manipulations/Data Step**


* [rxDataStep](rxDataStep.md): Transform and subset data.


* [rxGetFuzzyDist](rxGetFuzzyDist.md): Get fuzzy distances for a character vector.


* [rxGetFuzzyKeys](rxGetFuzzyKeys.md): Get fuzzy keys for a character vector.


* [rxFactors](rxFactors.md): Recode a factor variable or convert non-factor variable into a factor in an  .xdf file or data frame.


* [rxSort](rxSortXdf.md): Multi-key sorting of the variables an .xdf file or data frame.


* [rxMerge](rxMergeXdf.md): Merges two .xdf files or data frames using a variety of merge types.


* [rxSplit](rxSplitXdf.md): Splits an .xdf file or data frame into multiple .xdf files or data frames.




**Descriptive Statistics and Cross Tabs**


* [rxSummary](rxSummary.md): Basic summary statistics of data, including computations by group.


* [rxQuantile](rxQuantile.md): Computes approximate quantiles for .xdf files and data frames without sorting.


* [rxCrossTabs](rxCrossTabs.md): Formula-based cross-tabulation of data.


* [rxCube](rxCube.md): Alternative formula-based cross-tabulation designed for efficient representation.


* [rxMarginals](rxMarginals.md): Marginal summaries of cross-tabulations.


* [as.xtabs](as.xtabs.md): Converts cross tabulation results to an `xtabs` object.


* [rxChiSquaredTest](rxChiSquaredTest.md): Performs Chi-squared Test on `xtabs` object.


* [rxFisherTest](rxChiSquaredTest.md): Performs Fisher's Exact Test on `xtabs` object.


* [rxKendallCor](rxChiSquaredTest.md): Computes Kendall's Tau Rank Correlation Coefficient using `xtabs` object.


* [rxPairwiseCrossTab](rxPairwiseCrosstab.md): Apply a function to pairwise combinations of rows and columns of an `xtabs` object.


* [rxRiskRatio](rxRiskRatio.md): Calculate the relative risk on a two-by-two `xtabs` object.
 

* [rxOddsRatio](rxRiskRatio.md): Calculate the odds ratio on a two-by-two `xtabs` object.
 



**Statistical Modeling**


* [rxLinMod](rxLinMod.md): Fits a linear model to data.


* [rxCovCor](rxCovCor.md): Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.


* [rxCov](rxCovCor.md): Calculate the covariance matrix for a set of variables.


* [rxCor](rxCovCor.md): Calculate the correlation matrix for a set of variables


* [rxSSCP](rxCovCor.md): Calculate the sum of squares / cross-product matrix for a set of variables.
 

* [rxLogit](rxLogit.md): Fits a logistic regression model to data.


* [rxRoc](rxRoc.md): Receiver Operating Characteristic (ROC) computations using actual and predicted values from binary classifier system.


* [rxGlm](rxGLM.md): Fits a generalized linear model to data.


* [rxDTree](rxDTree.md):  Fits a classification or regression tree to data.


* [rxDForest](rxDForest.md):  Fits a classification or regression decision forest to data.


* [rxBTrees](rxBTrees.md):  Fits a classification or regression decision forest via stochastic gradient boosting.


* [rxPredict](rxPredict.md): Calculates predictions for fitted models.


* [rxKmeans](rxKmeans.md): Performs k-means clustering.


* [rxNaiveBayes](rxNaiveBayes.md): Performs Naive Bayes classification.


* [as.lm](as.lm.md): Coerce an `rxLinMod` object to an `lm` object for use with the **pmml** package.


* [as.glm](as.glm.md): Coerce an `rxLogit` or `rxGlm` object to a `glm` object for use with the **pmml** package.


* [as.rpart](as.rpart.md): Coerce an `rxDTree` object to an `rpart` object for use with the **pmml** package.


* [as.kmeans](as.kmeans.md): Coerce an `rxKmeans` object to an `kmeans` object for use with the **pmml** package.


* [as.randomForest](as.randomForest.md): Coerce an `rxDForest` object to a `randomForest` object for use with the **pmml** package.


* [as.gbm](as.gbm.md): Coerce an `rxBTrees` object to a `gbm` object for use with the **pmml** package.




**Utility Functions**


* [rxOptions](rxOptions.md): Gets or sets **RevoScaleR**-specific options.


* [rxGetOption](rxOptions.md): Retrieves a specific **RevoScaleR** option.


* [rxRngNewStream](rxRng.md) and related functions: Support for Parallel Random Number Generation.


* [rxGetEnableThreadPool](rxGetEnableThreadPool.md): Gets the current state of the thread pool, which on Linux can be either persistent or on-demand.


* [rxSetEnableThreadPool](rxGetEnableThreadPool.md): Sets the thread pool state.


* [rxStepControl](rxStepControl.md): Construct `variable.selection` argument for `rxLinMod`.




**Basic Graphing Functions**


* [rxHistogram](rxHistogram.md): Creates a histogram from data.


* [rxLinePlot](rxLinePlot.md): Creates a line plot from data.
 

* [rxLorenz](rxLorenz.md): Computes a Lorenz curve which can be plotted.


* [rxRocCurve](rxRoc.md): Computes and plots ROC curves from actual and predicted data.




**HPC and Distributed Computing**


* [RxComputeContext](RxComputeContext.md): Creates a compute context.


* [RxHadoopMR](RxHadoopMR.md): Creates an in-data, file-based Hadoop compute context.


* [RxSpark](RxSpark.md): Creates an in-data, file-based Spark compute context.


* [RxInTeradata](RxInTeradata.md): Creates an in-database compute context for Teradata.


* [RxInSqlServer](RxInSqlServer.md): Creates an in-database compute context for SQL Server 2016.


* [RxForeachDoPar](RxForeachDoPar.md): Creates a compute context for [rxExec](rxExec.md) using the current `foreach` parallel back end.


* [RxLocalParallel](RxLocalParallel.md): Creates a local compute context for [rxExec](rxExec.md) using the **parallel** package as back end.


* [RxLocalSeq](RxLocalSeq.md): Creates a local compute context for [rxExec](rxExec.md) using sequential computations.


* [rxSetComputeContext](rxSetComputeContext.md): Sets a compute context.


* [rxGetComputeContext](rxSetComputeContext.md): Gets the current compute context.


* [rxGetAvailableNodes](rxGetAvailableNodes.md): Get all the available nodes on a distributed compute context.


* [rxGetNodeInfo](rxGetNodeInfo.md): Get information on nodes specified for a distributed compute context.


* [rxPingNodes](rxPingNodes.md): Test round trip from end user through computation node(s) in a cluster or cloud.


* [rxExec](rxExec.md): Run an arbitrary R function on nodes or cores of a cluster.


* [rxGetJobStatus](rxGetJobResults.md): Get the status of a non-waiting distributed computing job.


* [rxGetJobResults](rxGetJobResults.md): Get the return object(s) of a non-waiting distributed computing job.


* [rxGetJobOutput](rxGetJobOutput.md): Get the console output from a non-waiting distributed computing job.


* [rxGetJobs](rxGetJobs.md): Get the available distributed computing job information objects.


* [rxLocateFile](rxLocateFile.md): Get the first occurrence of a specified input file in a set of specified paths.




**Package Management Functions**


* [rxInstalledPackages](rxInstalledPackages.md): Find (or retrieve) details of installed packages for a compute context.
,

* [rxInstallPackages](rxInstallPackages.md): Install Packages from Repositories or Local Files for a compute context.
,   

* [rxRemovePackages](rxRemovePackages.md): Removes installed packages from a compute context.
,

* [rxFindPackage](rxFindPackage.md): Find the path for one or more packages for a compute context.
,    

* [rxSqlLibPaths](rxSqlLibPaths.md): Gets the search path for the library trees for packages while executing inside the SQL server.
,



**Hadoop Convenience Functions**


* [rxHadoopCommand](rxHadoopCommand.md): Execute an arbitrary Hadoop command.


* [rxHadoopCopy](rxHadoopCommand.md): Copy a file in the Hadoop Distributed File System (HDFS).


* [rxHadoopCopyFromLocal](rxHadoopCommand.md): Copy a file from the native file system to HDFS.


* [rxHadoopCopyFromClient](rxHadoopCommand.md): Copy a file from a remote client to the Hadoop cluster's local file system, and then to HDFS.


* [rxHadoopCopyToLocal](rxHadoopCommand.md): Copy a file from HDFS to the local file system.


* [rxHadoopListFiles](rxHadoopCommand.md): List files in an HDFS directory.


* [rxHadoopMakeDir](rxHadoopCommand.md): Make a directory in HDFS.


* [rxHadoopMove](rxHadoopCommand.md): Move a file in HDFS.


* [rxHadoopRemove](rxHadoopCommand.md): Remove a file in HDFS.


* [rxHadoopRemoveDir](rxHadoopCommand.md): Remove a directory in HDFS.


* [rxHadoopVersion](rxHadoopCommand.md): Return the current Hadoop version.




 
 
 ##See Also
 [rxFormula](rxFormula.md), [rxTransform](rxTransform.md), [rxPackage](rxPackage.md)   
 ##Author(s)
 Microsoft Corporation 
 
 
