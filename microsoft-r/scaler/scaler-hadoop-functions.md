---

# required metadata
title: "ScaleR Functions for Hadoop"
description: "Microsoft R ScaleR Functions for Apache Hadoop MapReduce and Hadoop Spark."
keywords: "RevoScaleR, ScaleR, Microsoft R, Hadoop, Spark"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/07/2016"
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

This page presents a curated list of functions that might be particularly interesting to Hadoop users. These functions can be called directly from the command line.

The `RevoScaleR` package supports two Hadoop compute contexts:

+ `RxHadoopMR`, a distributed compute context on a Hadoop cluster. This compute context can be used on a node (including an edge node) of a Cloudera or Hortonworks cluster with a RHEL operating system, or a client with an SSH connection to such a cluster. For details on `RxHadoopMR` compute contexts, see the [*RevoScaleR Hadoop Getting Started Guide*](../scaler-hadoop-getting-started.md).

+ `RxSpark`, a distributed compute context in which computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark. This provides up to a 7x performance boost compared to `RxHadoopMR`.


>If you are looking for a more general list of `RevoScaleR` functions for Microsoft R, [see here](scaler.md).

As noted in the [RevoScaleR Hadoop MapReduce Getting Started Guide](../scaler-hadoop-getting-started.md#composite), the XDF file format has been modified for Hadoop to store data in a composite set of files rather than a single file. Both of these data sources can be specified for use with the Hadoop Distributed File System (HDFS).

<br />
## Data Analysis Functions

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
        <td>`rxDataStep`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from an .xdf file or a data frame.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
        <td width="160px">`RxXdfData`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>    
    <tr>
        <td>`RxTextData`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetInfo`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Retrieves summary information from a data source or data frame.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetVarInfo`</td>
        <td> </td>
        <td>Retrieves variable information from a data source or data frame.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetVarNames`</td>
        <td> </td>
        <td>Retrieves variable names from a data source or data frame.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
        <td>`rxHdfsFileSystem`</td>
        <td> </td>
        <td>Creates an HDFS file system object.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
    <td>`rxSparkListData` </td>
    <td> </td>
    <td>List cached `RxParquetData` or `RxHiveData` data source objects. </td>
    <td>
        <center><small>[See package](scaler.md#findmore)<center></small></td>   
    </tr>
    <tr>
    <td>`rxSparkRemoveData`</td>
    <td> </td>
    <td>Remove cached `RxParquetData` or `RxHiveData` data source objects.</td>
    <td>
        <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
</table>

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
        <td width="130px">`rxDataStep`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output) from an .xdf file or a data frame.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxFactors`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Create or recode factor variables in a composite XDF file in HDFS. A new file must be written out.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
</table>


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
        <td width="140px">`rxQuantile`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Computes approximate quantiles for .xdf files and data frames without sorting.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSummary`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCrossTabs`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Formula-based cross-tabulation of data.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCube`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Alternative formula-based cross-tabulation designed for efficient representation returning ‘cube’ results. Writing output to .xdf file not supported.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
</table>

<br />


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
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxLogit`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a logistic regression model to data.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGlm`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a generalized linear model to data.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCovCor`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxDTree`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a classification or regression tree to data.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxBTrees`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxDForest`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fits a classification or regression decision forest to data.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxPredict`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Calculates predictions for fitted models. Output must be an XDF data source.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxKmeans`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Performs k-means clustering.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxNaiveBayes`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Fit Naive Bayes Classifiers on an .xdf file or data frame for small or large data using parallel external memory algorithm. </td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
</table>

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
        <td width="180px">`RxHadoopMR`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates an in-data, file-based Hadoop compute context.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxSpark`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates an in-data, file-based Spark compute context. Computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
    <td>`rxSparkConnect`</td>
    <td> </td>
    <td>Create a persistent Spark compute context. </td>
    <td>
        <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
    <td>`rxSparkDisconnect`</td>
    <td> </td>
    <td> Disconnect a Spark session and return to a local compute context.</td>
    <td>
        <center><small>[See package](scaler.md#findmore)<center></small></td>
    </tr>
    <tr>
        <td>`rxInstalledPackages`</td>
        <td> </td>
        <td>Returns the list of installed packages for a compute context.</td>
        <td><center><small>[**View**](rxInstalledPackages.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxFindPackage`</td>
        <td> </td>
        <td>Returns the path to one or more packages for a compute context.</td>
        <td><center><small>[**View**](rxFindPackage.md)</small></center>
        </td>
    </tr>
</table>


<br/>
<a name="data"></a>

##Data Source Functions

Of course, not all data source types are available on all compute contexts. For the Hadoop compute contexts, two types of data sources can be used.

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
        <td>
            <center>![-](../media/award.png)</center>
        </td>

        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxTextData`</td>
        <td>
            <center>![-](../media/award.png)</center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>    <tr>
        <td>`RxHiveData`</td>
        <td> </td>
        <td>Generate a Hive Data Source object.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
        </tr>
        <tr>
        <td>`RxParquetData`</td>
        <td> </td>
        <td>Generate a Parquet Data Source object.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)<center></small></td>
        </tr>
</table>


<br />
##High Performance Computing and Distributed Computing Functions

The Hadoop compute context has a number of helpful functions used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../scaler-distributed-computing.md).


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
        <td width="150px">`rxExec`</td>
        <td> </td>
        <td>Run an arbitrary R function on nodes or cores of a cluster.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobStatus`</td>
        <td> </td>
        <td>Get the status of a non-waiting distributed computing job.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobResults`</td>
        <td> </td>
        <td>Get the return object(s) of a non-waiting distributed computing job.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobOutput`</td>
        <td> </td>
        <td>Get the console output from a non-waiting distributed computing job.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobs`</td>
        <td> </td>
        <td>Get the available distributed computing job information objects.</td>
        <td>
                <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
</table>





<br />
##Hadoop Convenience Functions

RevoScaleR also provides some wrapper functions for accessing Hadoop/HDFS functionality via R. These functions require access to Hadoop, either locally or remotely via the RxHadoopMR or RxSpark compute contexts.



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
        <td width="200px">`rxHadoopCommand`</td>
        <td width="15px"> </td>
        <td>Execute an arbitrary Hadoop command. Allows you to run basic Hadoop commands.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopVersion`</td>
        <td> </td>
        <td>Return the current Hadoop version.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopCopyFromClient`</td>
        <td> </td>
        <td>Copy a file from a remote client to the Hadoop cluster's local file system, and then to HDFS.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopCopyFromLocal`</td>
        <td> </td>
        <td>Copy a file from the native file system to HDFS. Wraps the Hadoop `fs -copyFromLocal` command.</td>
        <td>
            <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopCopy`</td>
        <td> </td>
        <td>Copy a file in the Hadoop Distributed File System (HDFS). Wraps the Hadoop `fs -cp` command.</td>
        <td>
                <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopRemove`</td>
        <td> </td>
        <td>Remove a file in HDFS. Wraps the Hadoop `fs -rm` command.</td>
        <td>
                <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopListFiles`</td>
        <td> </td>
        <td>List files in an HDFS directory. Wraps the Hadoop `fs -ls` or `fs -lsr` command.</td>
        <td>
                <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopMakeDir`</td>
        <td> </td>
        <td>Make a directory in HDFS. Wraps the Hadoop `fs -mkdir` command.</td>
        <td>
                <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopMove`</td>
        <td> </td>
        <td>Move a file in HDFS. Wraps the Hadoop `fs -mv` command.</td>
        <td>
                <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxHadoopRemoveDir`</td>
        <td> </td>
        <td>Remove a directory in HDFS. Wraps the Hadoop `fs -rmr` command.</td>
        <td>
                <center><small>[See package](scaler.md#findmore)</small></center>
        </td>
    </tr>
</table>
