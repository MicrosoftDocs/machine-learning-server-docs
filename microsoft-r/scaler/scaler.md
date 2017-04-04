---

# required metadata
title: "ScaleR Functions"
description: "ScaleR Functions"
keywords: "RevoScaleR, ScaleR"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "06/22/2016"
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

# RevoScaleR Functions



The `RevoScaleR` package provides a set of over one hundred portable, scalable, and distributable data analysis functions.

This topic presents a curated list of functions commonly used by Microsoft R users. These functions can be called directly from the command line.  For a list of the entire set of `RevoScaleR` functions, see [ScaleR functions by category](packagehelp/revoAnalytics-package.md)

While most of these functions are of general application, some are specific to particular compute contexts and some may not be fully supported in all compute contexts. If you are looking for the functions optimized for Hadoop, Teradata, or SQL Server compute contexts, see the relevant function list for that context:
+ [Computing on a Hadoop Cluster](scaler-hadoop-functions.md)
+ [Computing on a Teradata Datawarehouse](scaler-teradata-functions.md)
+ [Computing on SQL Server](https://msdn.microsoft.com/en-us/library/mt652103.aspx)

>Some function names begin with `rx` and others with `Rx`. The `Rx` function name prefix is used to distinguish the class constructors such as data sources and compute contexts.

<br />
## Data Analysis Functions
<!--<br />
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
-->

<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![import](../media/scaler/data-science-labels.png)


Whenever you want to perform an analysis using `RevoScaleR` functions, you should specify three distinct pieces of information:
 + The [**analytic function**](#analytics), which specifies the analysis to be performed
 + The [**compute context**](#compute), which specifies where the computations should take place
 + The [**data source**](#data), which is the data to be used

<br />
####Import and Export Functions

<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="160px">`rxImport`</td>
        <td width="25px">
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates an .xdf file or data frame from a data source (e.g. text, SAS, SPSS data files, ODBC or Teradata connection, or data frame).</td>
        <td>
            <center><small>[**View**](packagehelp/rxImport.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxDataStep`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from an .xdf file or a data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxDataStep.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetInfo`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Retrieves summary information from a data source or data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetInfo.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxSetInfo`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Sets a file description in an .xdf file or a description attribute in a data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSetInfo.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetVarInfo`</td>
        <td> </td>
        <td>Retrieves variable information from a data source or data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetVarInfo.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxSetVarInfo`</td>
        <td> </td>
        <td>Modifies variable information in an .xdf file or data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSetVarInfo.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetVarNames`</td>
        <td> </td>
        <td>Retrieves variable names from a data source or data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetVarNames.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxCreateColInfo`</td>
        <td> </td>
        <td>Generates a 'colInfo' list from a data source.</td>
        <td>
            <center><small>[**View**](packagehelp/rxCreateColInfo.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxCompressXdf`</td>
        <td> </td>
        <td>Compresses an existing .xdf file, or a directory of .xdf files.</td>
        <td>
            <center><small>[**View**](packagehelp/rxCompressXdf.md)<center></small></td>
    </tr>
    <tr>
        <td>`RxXdfData`</td>
        <td> </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxXdfData.md)<center></small></td>
    </tr>
    <tr>
        <td>`RxTextData`</td>
        <td> </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxTextData.md)<center></small></td>
    </tr>
    <tr>
        <td>`RxSasData`</td>
        <td> </td>
        <td>Creates a SAS data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxSasData.md)<center></small></td>
    </tr>
    <tr>
        <td>`RxSpssData`</td>
        <td> </td>
        <td>Creates a SPSS data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxSpssData.md)<center></small></td>
    </tr>
    <tr>
        <td>`RxOdbcData`</td>
        <td> </td>
        <td>Creates a ODBC data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxOdbcData.md)<center></small></td>
    </tr>
    <tr>
        <td>`RxTeradata`</td>
        <td> </td>
        <td>Creates a Teradata data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxTeradata.md)<center></small></td>
    </tr>
    <tr>
        <td>`RxSqlServerData`</td>
        <td> </td>
        <td>Creates a SQL Server data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxSqlServerData.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxOpen`</td>
        <td> </td>
        <td>Opens a data source for reading.</td>
        <td>
            <center><small>[**View**](packagehelp/rxOpen-methods.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxClose`</td>
        <td> </td>
        <td>Closes a data source.</td>
        <td>
            <center><small>[**View**](packagehelp/rxOpen-methods.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxReadNext`</td>
        <td> </td>
        <td>Read data from a source</td>
        <td>
            <center><small>[**View**](packagehelp/rxOpen-methods.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxSetFileSystem`</td>
        <td> </td>
        <td>Specify a file system type for data for import.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSetFileSystem.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetFileSystem`</td>
        <td> </td>
        <td>Retrieve the current file system type.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetFileSystem.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxHdfsFileSystem`</td>
        <td> </td>
        <td>Creates an HDFS file system object.</td>
        <td>
            <center><small>[**View**](packagehelp/rxHdfsFileSystem.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxNativeFileSystem`</td>
        <td> </td>
        <td>Creates a native file system object.</td>
        <td>
            <center><small>[**View**](packagehelp/rxNativeFileSystem.md)<center></small></td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>
<br />

<br />
####Manipulation, Cleansing, and Transformation Functions


<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="160px">`rxDataStep`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output) from an .xdf file or a data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxDataStep.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxFactors`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Recode a factor variable or convert non-factor variable into a factor in an .xdf file or data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxFactors.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetFuzzyDist`</td>
        <td> </td>
        <td>Get fuzzy distances for a character vector.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetFuzzyDist.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetFuzzyKeys`</td>
        <td> </td>
        <td>Get fuzzy keys for a character vector.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetFuzzyKeys.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSplit`</td>
        <td> </td>
        <td>Splits an .xdf file or data frame into multiple .xdf files or data frames.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSplit.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSort`</td>
        <td> </td>
        <td>Multi-key sorting of the variables an .xdf file or data frame.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSort.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxMerge`</td>
        <td> </td>
        <td>Merges two .xdf files or data frames using a variety of merge types.</td>
        <td>
            <center><small>[**View**](packagehelp/rxMerge.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxExecuteSQLDDL`</td>
        <td> </td>
        <td>SQL Server R Services only. Runs an arbitrary SQL DDL command.</td>
        <td>
            <center><small>[**View**](packagehelp/rxExecuteSQLDDL.md)</small></center>
        </td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>


<br/>


<br />
####Visualization Functions

|Function Name          | |Description|Help|
|-----------------------|:-:|-----------------------|:--------------:|
|`rxHistogram`       |![-](../media/award.png)|Creates a histogram from data.|<small>[**View**](packagehelp/rxHistogram.md)</small>|
|`rxLinePlot`  |![-](../media/award.png)|Creates a line plot from data.|<small>[**View**](packagehelp/rxLinePlot.md)</small>|
| `rxLorenz`      | |Computes a Lorenz curve which can be plotted.|<small>[**View**](packagehelp/rxLorenz.md)</small>|
|`rxRocCurve`  | |Computes and plots ROC curves from actual and predicted data.|<small>[**View**](packagehelp/rxRocCurve.md)</small>|

<small>![-](../media/award.png) signifies the most popular functions</small>

<br />

<br />
####Analysis Functions for Descriptive Statistics and Cross-Tabulations


<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="150px">`rxQuantile`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Computes approximate quantiles for .xdf files and data frames without sorting.</td>
        <td>
            <center><small>[**View**](packagehelp/rxQuantile.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSummary`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSummary.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCrossTabs`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Formula-based cross-tabulation of data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxCrossTabs.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCube`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Alternative formula-based cross-tabulation designed for efficient representation returning ‘cube’ results. Writing output to .xdf file not supported.</td>
        <td>
            <center><small>[**View**](packagehelp/rxCube.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxMarginals`</td>
        <td> </td>
        <td>Marginal summaries of cross-tabulations.</td>
        <td>
            <center><small>[**View**](packagehelp/rxMarginals.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`as.xtabs`</td>
        <td> </td>
        <td>Converts cross tabulation results to an `xtabs` object.</td>
        <td>
            <center><small>[**View**](packagehelp/as.xtabs.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxChiSquaredTest`</td>
        <td> </td>
        <td>Performs Chi-squared Test on `xtabs` object. Used with small data sets and does not chunk data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxChiSquaredTest.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxFisherTest`</td>
        <td> </td>
        <td>Performs Fisher's Exact Test on `xtabs` object. Used with small data sets and does not chunk data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxFisherTest.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxKendallCor`</td>
        <td> </td>
        <td>Computes Kendall's Tau Rank Correlation Coefficient using `xtabs` object.</td>
        <td>
            <center><small>[**View**](packagehelp/rxKendallCor.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxPairwiseCrossTab`</td>
        <td> </td>
        <td>Apply a function to pairwise combinations of rows and columns of an `xtabs` object.</td>
        <td>
            <center><small>[**View**](packagehelp/rxPairwiseCrossTab.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxRiskRatio`</td>
        <td> </td>
        <td>Calculate the relative risk on a two-by-two `xtabs` object.</td>
        <td>
            <center><small>[**View**](packagehelp/rxRiskRatio.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxOddsRatio`</td>
        <td> </td>
        <td>Calculate the odds ratio on a two-by-two `xtabs` object.</td>
        <td>
            <center><small>[**View**](packagehelp/rxOddsRatio.md)</small></center>
        </td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>

<br />

####Analysis, Learning, and Prediction Functions for Statistical Modeling


<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="150px">`rxLinMod`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a linear model to data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxLinMod.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxLogit`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a logistic regression model to data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxLogit.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGlm`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a generalized linear model to data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGlm.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCovCor`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.</td>
        <td>
            <center><small>[**View**](packagehelp/rxCovCor.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxDTree`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a classification or regression tree to data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxDTree.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxBTrees`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm.</td>
        <td>
            <center><small>[**View**](packagehelp/rxBTrees.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxDForest`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a classification or regression decision forest to data.</td>
        <td>
            <center><small>[**View**](packagehelp/rxDForest.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxPredict`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Calculates predictions for fitted models. Output must be an XDF data source.</td>
        <td>
            <center><small>[**View**](packagehelp/rxPredict.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxKmeans`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Performs k-means clustering.</td>
        <td>
            <center><small>[**View**](packagehelp/rxKmeans.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxNaiveBayes`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Performs Naive Bayes classification.</td>
        <td>
            <center><small>[**View**](packagehelp/rxNaiveBayes.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCov`</td>
        <td> </td>
        <td>Calculate the covariance matrix for a set of variables.</td>
        <td>
            <center><small>[**View**](packagehelp/rxCov.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCor`</td>
        <td> </td>
        <td>Calculate the correlation matrix for a set of variables.</td>
        <td>
            <center><small>[**View**](packagehelp/rxCor.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSSCP`</td>
        <td> </td>
        <td>Calculate the sum of squares / cross-product matrix for a set of variables.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSSCP.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxRoc`</td>
        <td> </td>
        <td>Receiver Operating Characteristic (ROC) computations using actual and predicted values from binary classifier system.</td>
        <td>
            <center><small>[**View**](packagehelp/rxRoc.md)</small></center>
        </td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>

<br />
<a name="compute"></a>
##Compute Context Functions



<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="180px">`rxSetComputeContext`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Sets a compute context.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSetComputeContext.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetComputeContext`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Gets the current compute context.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetComputeContext.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxHadoopMR`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates an in-data, file-based Hadoop compute context.</td>
        <td>
            <center><small>[**View**](packagehelp/RxHadoopMR.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxSpark`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates an in-data, file-based Spark compute context. Computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark.</td>
        <td>
            <center><small>[**View**](packagehelp/RxSpark.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxInTeradata`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates an in-database compute context for Teradata.</td>
        <td>
            <center><small>[**View**](packagehelp/RxInTeradata.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxInSqlServer`
            <td> </td>
            <td>Creates an in-database compute context for SQL Server.</td>
            <td>
                <center><small>[**View**](packagehelp/RxInSqlServer.md)</small></center>
            </td>
    </tr>
    <tr>
        <td>`RxComputeContext`</td>
        <td> </td>
        <td>Creates a compute context.</td>
        <td>
            <center><small>[**View**](packagehelp/RxComputeContext.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxLocalSeq`</td>
        <td> </td>
        <td>Creates a local compute context for `rxExec` using sequential computations.</td>
        <td>
            <center><small>[**View**](packagehelp/RxLocalSeq.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxLocalParallel`</td>
        <td> </td>
        <td>Creates a local compute context for `rxExec` using the `parallel` package as backend.</td>
        <td>
            <center><small>[**View**](packagehelp/RxLocalParallel.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxForeachDoPar`</td>
        <td> </td>
        <td>Creates a compute context for `rxExec`using the current `foreach` parallel backend.</td>
        <td>
            <center><small>[**View**](packagehelp/RxForeachDoPar.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxInstalledPackages`</td>
        <td> </td>
        <td>Returns the list of installed packages for a compute context.</td>
        <td><center><small>[**View**](packagehelp/rxInstalledPackages.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxFindPackage`</td>
        <td> </td>
        <td>Returns the path to one or more packages for a compute context.</td>
        <td><center><small>[**View**](packagehelp/rxFindPackage.md)</small></center>
        </td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>

<br/>
<a name="data"></a>
##Data Source Functions

<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="150px">`RxXdfData`</td>
        <td> </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxXdfData.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxTextData`</td>
        <td> </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxTextData.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxSasData`</td>
        <td> </td>
        <td>Creates a SAS data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxSasData.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxSpssData`</td>
        <td> </td>
        <td>Creates a SPSS data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxSpssData.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxOdbcData`</td>
        <td> </td>
        <td>Creates a ODBC data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxOdbcData.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxTeradata`</td>
        <td> </td>
        <td>Creates a Teradata data source object.</td>
        <td>
            <center><small>[**View**](packagehelp/RxTeradata.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxSqlServerData`</td>
        <td> </td>
        <td>Creates a SQL Server data source object.</td>
        <td><center><small>[**View**](packagehelp/RxSqlServerData.md)</small></center>
        </td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>

<br />

##HPC and Distributed Computing Functions

These functions and many more can be used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../scaler-distributed-computing.md).

<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="180px">`rxExec`</td>
        <td> </td>
        <td>Run an arbitrary R function on nodes or cores of a cluster.</td>
        <td>
            <center><small>[**View**](packagehelp/rxExec.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxRngNewStream`</td>
        <td> </td>
        <td>Support for Parallel Random Number Generation.</td>
        <td>
            <center><small>[**View**](packagehelp/rxRngNewStream.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxRngDelStream`</td>
        <td> </td>
        <td>Support for Parallel Random Number Generation.</td>
        <td>
            <center><small>[**View**](packagehelp/rxRngDelStream.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxRngGetStream`</td>
        <td> </td>
        <td>Support for Parallel Random Number Generation.</td>
        <td>
            <center><small>[**View**](packagehelp/`rxRngGetStream.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxRngSetStream`</td>
        <td> </td>
        <td>Support for Parallel Random Number Generation.</td>
        <td>
            <center><small>[**View**](packagehelp/rxRngSetStream.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetAvailableNodes`</td>
        <td> </td>
        <td>Get all the available nodes on a distributed compute context.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetAvailableNodes.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetNodeInfo`</td>
        <td> </td>
        <td>Get information on nodes specified for a distributed compute context.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetNodeInfo.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxPingNodes`</td>
        <td> </td>
        <td>Test round trip from user through computation node(s) in a cluster or cloud.</td>
        <td>
            <center><small>[**View**](packagehelp/rxPingNodes.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobStatus`</td>
        <td> </td>
        <td>Get the status of a non-waiting distributed computing job.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetJobStatus.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobResults`</td>
        <td> </td>
        <td>Get the return object(s) of a non-waiting distributed computing job.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetJobResults.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobOutput`</td>
        <td> </td>
        <td>Get the console output from a non-waiting distributed computing job.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetJobOutput.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobs`</td>
            <td> </td>
            <td>Get the available distributed computing job information objects.</td>
            <td>
                <center><small>[**View**](packagehelp/rxGetJobs.md)</small></center>
            </td>
    </tr>
    <tr>
        <td>`rxLocateFile`</td>
            <td> </td>
            <td>Get the first occurrence of a specified input file in a set of specified paths.</td>
            <td>
                <center><small>[**View**](packagehelp/rxLocateFile.md)</small></center>
            </td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>

<br />
##Utility Functions

>Not all of these functions will work if you switch your compute context to Hadoop, Teradata, or SQL Server.

<table>
    <tr>
        <th>Function Name</th>
        <th></th>
        <th>Description</th>
        <th>
            <center>Help</center>
        </th>
    </tr>
    <tr>
        <td width="200px">`rxOptions`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Gets or sets `RevoScaleR`-specific options.</td>
        <td>
            <center><small>[**View**](packagehelp/rxOptions.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetOption`</td>
                <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Retrieves a specific `RevoScaleR`-option.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetOption.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetEnableThreadPool`</td>
        <td> </td>
        <td>Gets the current state of the thread pool, which on Linux can be either persistent or on-demand.</td>
        <td>
            <center><small>[**View**](packagehelp/rxGetEnableThreadPool.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSetEnableThreadPool`</td>
        <td> </td>
        <td>Sets the thread pool state.</td>
        <td>
            <center><small>[**View**](packagehelp/rxSetEnableThreadPool.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxStepControl`</td>
        <td> </td>
        <td>Construct `variable.selection` argument for `rxLinMod`.</td>
        <td>
            <center><small>[**View**](packagehelp/rxStepControl.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxIsOpen`</td>
        <td> </td>
        <td>Indicates whether a data source can be accessed.</td>
        <td><center><small>[**View**](packagehelp/rxIsOpen.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSqlServerDropTable`</td>
        <td> </td>
        <td>Execute an SQL statement that drops a table.</td>
        <td><center><small>[**View**](packagehelp/rxSqlServerDropTable.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSqlServerTableExists`</td>
        <td> </td>
        <td>Execute an SQL statement that checks for a table's existance.</td>
        <td><center><small>[**View**](packagehelp/rxSqlServerTableExists.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxWriteNext`</td>
        <td> </td>
        <td>Writes the next chunk when moving data between ScaleR data sources.</td>
        <td><center><small>[**View**](packagehelp/rxOpen-methods.md)</small></center>
        </td>
    </tr>
</table>

<small>![-](../media/award.png) signifies the most popular functions</small>

<br />
<br />

<a name="findmore"></a>
## Get help on RevoScaleR functions from the R console

To see the **RevoScaleR** functions that can be called from the R console:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
2. Load `RevoScaleR` from the command line by typing `library(RevoScaleR)`.
1. In the console, open the package help by typing the following at the R prompt: `help(package="RevoScaleR")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.
 
> [!NOTE]
> To list all public functions, type library(help="RevoScaleR") at the R prompt.
>

**Search for an object**

1. In an R console, return a numbered list of objects by typing the following at the R prompt `>`:
   ```
   > search()
   ```

2. Identify the position of the object you are interested in. You should see RevoScaleR in the list.

   ![objects](../media/scaler-rconsole-obj.png)

3. At the R prompt, type `objects(<position>)` to reveal the set of functions such as:
   ```
   > objects(5)
   ```
4. Use `?<function_name>` to open the help page for that function.
