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

# ScaleR Functions

The `RevoScaleR` package includes hundreds of functions you can use for data analysis, for high-powered and distributed computing, and when working with Hadoop. About 180 functions can be called directly from the command-line.

**To see the `RevoScaler` functions that can be called from the commands-line:**

1. With Microsoft R Server or R Client installed, launch an R console.

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


## Data Analysis Functions

|![import](../media/scaler-puzzle1.png)|![import](../media/scaler-puzzle2.png)|![import](../media/scaler-puzzle3.png)|![import](../media/scaler-puzzle4.png)|![import](../media/scaler-puzzle5.png)|  
|-|-|-|-|-|
|**Import & Export**|**Manipulate, <br /> Clean, Chunk**|**Visualize**|**Analyze, Learn, <br /> Predict**|**???**|
|<!--COL-1-->[`rxImport()`](rxClose.md) <br /> [`rxXdfToText()`]() <br /> [`rxGetInfo()`]() <br /> [`rxSetInfo()`]() <br /> [`rxGetVarInfo()`]() <br /> [`rxSetVarInfo()`]() <br /> [`rxGetVarNames()`]() <br /> [`rxCompressXdf()`]() <br /> [`RxXdfData()`]()<br />[`RxTextData()`]() <br /> [`RxSasData()`]() <br /> [`RxSpssData()`]() <br /> [`RxOdbcData()`]()|<!--COL-2-->[`rxDataStep()`]() <br /> [`rxFactors()`]() <br /> [`rxSort()`]() <br /> [`rxMerge()`]() <br /> [`rxSplit()`]()|<!--COL-3--> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() |<!--COL-4-->_Descriptive Statistics <br/>& Cross-Tabulation:_ <br /> [`()`]() <br />[`()`]() <br /> [`()`]() <br /> <br /> _Statistical Modeling:_ <br /> [`()`]() <br /> <br /> _Prediction:_ <br /> [`rxPredict()`]() <br /> [`()`]()|<!--COL-5-->  |

<br>
<br>

|![import](../media/scaler-puzzle1.png)  |![import](../media/scaler-puzzle1.png)|  
|----------------------------------------|--------------------------------------|
|**Data Sources**                        |**Utility Functions**                 |
|<!--COL-1-->[`rxImport()`](rxClose.md)<br />[`rxXdfToText()`]()<br />[`rxGetInfo()`]()<br />[`rxSetInfo()`]()<br />[`rxGetVarInfo()`]()<br />[`rxSetVarInfo()`]()<br />[`rxGetVarNames()`]()<br />[`rxCompressXdf()`]()<br />[`RxXdfData()`]()<br />[`RxTextData()`]()<br />[`RxSasData()`]()<br />[`RxSpssData()`]()<br />[`RxOdbcData()`]()<br />|<!--COL-2-->[`()`]()<br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]()| 


<br>
<br>


## High Powered Computing & Distributed Computing

|![import](../media/scaler-puzzle1.png)|![import](../media/scaler-puzzle1.png)|  
|--------------------------------------|--------------------------------------|
|**Compute Context**                   |**Others**                            |
|<!--COL-1-->[`RxComputeContext()`]() <br /> [`RxForeachDoPar()`]() <br /> [`RxLocalParallel()`]() <br /> [`RxLocalSeq()`]() <br /> [`rxGetComputeContext()`](rxGetComputeContext.md) <br /> [`rxSetComputeContext()`](rxSetComputeContext.md)|<!--COL-2--> [`rxGetAvailableNodes()`]() <br /> [`rxGetNodeInfo()`]() <br /> [`rxPingNodes()`]() <br /> [`rxExec()`]() <br /> [`rxGetJobStatus()`]() <br /> [`rxGetJobResults()`]() <br /> [`rxGetJobOutput()`]() <br /> [`rxGetJobs()`]() <br /> [`rxLocateFile()`]()| 




##  <a name="bkmk_DataInputAndOutput"></a> Data Input and Output  
  
|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxImport|Creates an .XDF file or data frame from a data source such as a text file, data file, ODBC or Teradata connection, or data frame\)||  
|rxXdfToText|Creates a text file from an .XDF file||  
|rxGetInfo|Retrieves header information from an .XDF file or summary information from a data frame|`str()`<br /><br /> `names()`<br /><br /> `colNames()`|  
|rxSetInfo|Sets a file description in an .XDF file or a description attribute in a data frame||  
|rxGetVarInfo|Retrieves variable information from an .XDF file or data frame|`names()`<br /><br /> `str()`<br /><br /> `nrow()`<br /><br /> `min()`<br /><br /> `max()`|  
|rxSetVarInfo|Modifies variable information in an .XDF file or data frame||  
|rxGetVarNames|Retrieves variable names from an .XDF file or data frame||  
|rxCompressXdf|Compresses an existing .XDF file, or a directory of .XDF files||  
|RxXdfData|Creates an XDF data source object||  
|RxTextData|Creates a text data source object||  
|RxSasData|Creates a SAS data source object|`foreign::read.ssd()`|  
|RxSpssData|Creates an SPSS data source object|`foreign::read.ssps()`|  
|RxOdbcData|Creates an ODBC data source object||  
|RxTeradata|Creates a Teradata data source object||  
|rxOpen|Opens a data source for reading|`read.table()` etc.|  
|rxReadNext|Reads data from a data source|`read.table()`, etc.|  
|rxClose|Closes a data source||  
|rxSetFileSystem|Specifies a file system type for data for import||  
|rxGetFileSystem|Retrieves the current file system type||  
|RxHdfsFileSystem|Creates an HDFS file system object||  
|RxNativeFileSystem|Creates a native file system object||  
   
>  The functions `rxDataStep`, `rxXdfToDataFrame`, and `rxReadXdf:Reads` can also read an .XDF file into a data  frame.  
  
##  <a name="bkmk_DataManipulation"></a> Data Manipulation  and Chunking  
  
|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxDataStep|Transforms and subsets data in .XDF files or data frames|`transform()`<br /><br /> `with()`<br /><br /> `within()`<br /><br /> `subset()`|  
|rxFactors|Recodes a factor variable, or converts a non\-factor variable into a factor|`factor()`|  
|rxSort|Performs multi\-key sorting of the variables in an .XDF file or data frame|`sort()`<br /><br /> `order()`|  
|rxMerge|Merges two .XDF files or two data frames using a variety of merge types|`merge()`<br /><br /> `rbind()`<br /><br /> `cbind()`|  
|rxSplit|Splits an .XDF file or a data frame into multiple .XDF files or data frames|`split()`|  
  
##  <a name="bkmk_DescriptiveStatistics"></a> Descriptive Statistics and Cross\-Tabulation  
  
|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxSummary|Generates summary statistics for a data frame, including computations by group|`summary()`<br /><br /> `lapply(x, …)`|  
|rxQuantile|Computes approximate quantiles for an .XDF file or data frame without sorting|`quantile()`|  
|rxCrossTabs|Creates a cross\-tabulation  of data based on a formula provided as parameter|`xtabs()`|  
|rxCube|Creates a cross\-tabulation of data based on formula provided as parameter<br /><br /> This function is an alternative to `rxCrossTabs` and is designed for efficient representation.|`xtabs()`|  
|rxMarginals|Creates a marginal summary for an **xtab** object|`addmargins()`<br /><br /> `colSums()`<br /><br /> `rowSums()`|  
|as.crosstabs|Converts cross tabulation results to an **xtab** object|`xtabs()`|  
|rxChiSquaredTest|Performs a chi\-squared test on an **xtab** object|`chisq.test()`|  
|rxFisherTest|Performs Fisher's Exact Test on an **xtab** object|`fisher.test()`|  
|rxKendallCor|Computes Kendall's Tau Rank Correlation Coefficient using an **xtab** object|`cor(…, method="kendall")`|  
|rxPairwiseCrossTab|Applies a function to pair\-wise combinations of rows and columns of an **xtab** object||  
|rxRiskRatio|Calculates the relative risk on a two\-by\-two **xtab** object||  
|rxOddsRatio|Calculates the odds ratio on a two\-by\-two **xtab** object||  
  
##  <a name="bkmk_StatisticalModeling"></a> Statistical Modeling  
  
|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxLinMod|Fits a linear model to data|`lm()`|  
|rxCovCor|Calculates the covariance, correlation, or sum of squares \(cross\-product\) matrix for a set of variables|`cor()`<br /><br /> `cov()`<br /><br /> `crossprod()`|  
|rxCov|Calculates the covariance matrix for a set of variables|`cov()`|  
|rxCor|Calculates the correlation matrix for a set of variables|`cov()`|  
|rxSSCP|Calculates the sum of squares \/ cross\-product matrix for a set of variables||  
|rxLogit|Fits a logistic regression model to data|`glm(…, family="binomial")`|  
|rxRoc|Calculates a Receiver Operating Characteristic \(ROC\) using actual and predicted values from a binary classifier||  
|rxGlm|Fits a generalized linear model to data|`glm()`|  
|rxDTree|Fits a classification or regression tree to data|`tree::tree()`<br /><br /> `rpart::rpart()`|  
|rxPredict|Calculates predictions for fitted models|`predict()`|  
|rxKmeans|Performs K\-means clustering|`cluster::kmeans()`|  
  
##  <a name="bkmk_UtilityFunctions"></a> Utility Functions  
  

|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxOptions|Gets or sets options specific to the **RevoScaleR** package and its functions||  
|rxGetOption|Retrieves a specific **RevoScaleR** option||  
|rxRngNewStream<br /><br /> \(see related functions\)|Provides support for parallel random number generation||  
|rxGetEnableThreadPool|Gets the current state of the thread pool, which on Linux can be either persistent or on\-demand||  
|rxSetEnableThreadPool|Sets the thread pool state||  
|rxStepControl|Constructs the *variableselection* argument for `rxLinMod`||  
  
##  <a name="bkmk_BasicGraphing"></a> Basic Graphing  
  

|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxHistogram|Creates a histogram from data|`hist()`|  
|rxLinePlot|Creates a line plot from data|`plot()`<br /><br /> `lines()`|  
|rxLorenz|Computes a Lorenz curve that can be plotted||  
|rxRocCurve|Computes and plots ROC curves from actual and predicted data||  
  
##  <a name="bkmk_HPCAndDistributed"></a> HPC and Distributed Computing   

|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|RxComputeContext|Creates a compute context||  
|RxForeachDoPar|Creates a compute context for `rxExec` using the current **foreach** parallel back end||  
|RxLocalParallel|Creates a local compute context for `rxExec` using the parallel package as back end||  
|RxLocalSeq|Creates a local compute context for `rxExec` using sequential computations||  
|rxSetComputeContext|Sets a compute context||  
|rxGetComputeContext|Gets the current compute context||  
|rxGetAvailableNodes|Gets all the available nodes on a distributed compute context||  
|rxGetNodeInfo|Gets information on nodes specified for a distributed compute context||  
|rxPingNodes|Tests a round trip from the end user through all computation nodes in a cluster or cloud||  
|rxExec|Runs an arbitrary R function on nodes or cores of a cluster||  
|rxGetJobStatus|Gets the status of a non\-waiting distributed computing job||  
|rxGetJobResults|Gets the return objects of a non\-waiting distributed computing job||  
|rxGetJobOutput|Gets the console output from a non\-waiting distributed computing job||  
|rxGetJobs|Gets the available distributed computing job information objects||  
|rxLocateFile|Gets the first occurrence of a specified input file in a set of specified paths||  
  