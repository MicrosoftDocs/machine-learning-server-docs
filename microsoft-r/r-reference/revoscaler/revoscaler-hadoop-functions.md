---

# required metadata
title: "Spark and Hadoop RevoScaleR functions (Machine Learning Server and Microsoft R) "
description: "Microsoft R RevoScaleR Functions for Apache Hadoop Spark and Hadoop MapReduce."
keywords: "RevoScaleR, RevoScaleR, Microsoft R, Hadoop, Spark"
author: "chuckheinzelman"
ms.author: "charlhe"
manager: "cgronlun"
ms.date: "01/29/2018"
ms.topic: "reference"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# RevoScaleR Functions for Spark on Hadoop

The [RevoScaleR package](revoscaler.md) provides a set of portable, scalable, distributable data analysis functions. This page presents a curated list of functions that might be particularly interesting to Hadoop users. These functions can be called directly from the command line.

The RevoScaleR package supports two Hadoop compute contexts:

+ RxSpark (recommended), a distributed compute context in which computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark. This provides up to a 7x performance boost compared to `RxHadoopMR`. For guidance, see [How to use RevoScaleR on Spark](~/r/how-to-revoscaler-spark.md).

+ RxHadoopMR (deprecated), a distributed compute context on a Hadoop cluster. This compute context can be used on a node (including an edge node) of a Cloudera or Hortonworks cluster with a RHEL operating system, or a client with an SSH connection to such a cluster. For guidance, see [How to use RevoScaleR on Hadoop MapReduce](~/r/how-to-revoscaler-hadoop.md).

On Hadoop Distributed File System (HDFS), the XDF file format stores data in a composite set of files rather than a single file. 

## Data Analysis Functions

#### Import and Export Functions

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
        <td><code>rxDataStep</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from an .xdf file or a data frame.</td>
        <td>
            <center><small><a href="rxdatastep.md" data-raw-source="[**View**](rxdatastep.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td width="160px"><code>RxXdfData</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small><a href="rxxdfdata.md" data-raw-source="[**View**](rxxdfdata.md)"><strong>View</strong></a><center></small></td>
    </tr><br/>    <tr>
        <td><code>RxTextData</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small><a href="rxtextdata.md" data-raw-source="[**View**](rxtextdata.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td><code>rxGetInfo</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Retrieves summary information from a data source or data frame.</td>
        <td>
            <center><small><a href="rxgetinfoxdf.md" data-raw-source="[**View**](rxgetinfoxdf.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td><code>rxGetVarInfo</code></td>
        <td> </td>
        <td>Retrieves variable information from a data source or data frame.</td>
        <td>
            <center><small><a href="rxgetvarinfo.md" data-raw-source="[**View**](rxgetvarinfo.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td><code>rxGetVarNames</code></td>
        <td> </td>
        <td>Retrieves variable names from a data source or data frame.</td>
        <td>
            <center><small><a href="rxgetvarnames.md" data-raw-source="[**View**](rxgetvarnames.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td><code>rxHdfsFileSystem</code></td>
        <td> </td>
        <td>Creates an HDFS file system object.</td>
        <td>
            <center><small><a href="rxhdfsfilesystem.md" data-raw-source="[**View**](rxhdfsfilesystem.md)"><strong>View</strong></a><center></small></td>
    </tr>
</table>

<br />
#### Manipulation, Cleansing, and Transformation Functions

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
        <td width="130px"><code>rxDataStep</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output) from an .xdf file or a data frame.</td>
        <td>
            <center><small><a href="rxdatastep.md" data-raw-source="[**View**](rxdatastep.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxFactors</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Create or recode factor variables in a composite XDF file in HDFS. A new file must be written out.</td>
        <td>
            <center><small><a href="rxfactors.md" data-raw-source="[**View**](rxfactors.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
</table>


<br />
#### Analysis Functions for Descriptive Statistics and Cross-Tabulations

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
        <td width="140px"><code>rxQuantile</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Computes approximate quantiles for .xdf files and data frames without sorting.</td>
        <td>
            <center><small><a href="rxquantile.md" data-raw-source="[**View**](rxquantile.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxSummary</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported.</td>
        <td>
            <center><small><a href="rxsummary.md" data-raw-source="[**View**](rxsummary.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxCrossTabs</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Formula-based cross-tabulation of data.</td>
        <td>
            <center><small><a href="rxcrosstabs.md" data-raw-source="[**View**](rxcrosstabs.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxCube</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Alternative formula-based cross-tabulation designed for efficient representation returning ‘cube’ results. Writing output to .xdf file not supported.</td>
        <td>
            <center><small><a href="rxcube.md" data-raw-source="[**View**](rxcube.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
</table>

<br />


<br />
#### Analysis, Learning, and Prediction Functions for Statistical Modeling

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
        <td width="150px"><code>rxLinMod</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a linear model to data.</td>
        <td>
            <center><small><a href="rxlinmod.md" data-raw-source="[**View**](rxlinmod.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxLogit</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a logistic regression model to data.</td>
        <td>
            <center><small><a href="rxlogit.md" data-raw-source="[**View**](rxlogit.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGlm</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a generalized linear model to data.</td>
        <td>
            <center><small><a href="rxglm.md" data-raw-source="[**View**](rxglm.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxCovCor</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.</td>
        <td>
            <center><small><a href="rxcovcor.md" data-raw-source="[**View**](rxcovcor.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxDTree</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a classification or regression tree to data.</td>
        <td>
            <center><small><a href="rxdtree.md" data-raw-source="[**View**](rxdtree.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxBTrees</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm.</td>
        <td>
            <center><small><a href="rxbtrees.md" data-raw-source="[**View**](rxbtrees.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxDForest</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a classification or regression decision forest to data.</td>
        <td>
            <center><small><a href="rxdforest.md" data-raw-source="[**View**](rxdforest.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxPredict</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Calculates predictions for fitted models. Output must be an XDF data source.</td>
        <td>
            <center><small><a href="../microsoftml/rxpredict.md" data-raw-source="[**View**](../microsoftml/rxpredict.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxKmeans</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Performs k-means clustering.</td>
        <td>
            <center><small><a href="rxkmeans.md" data-raw-source="[**View**](rxkmeans.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxNaiveBayes</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Fit Naive Bayes Classifiers on an .xdf file or data frame for small or large data using parallel external memory algorithm. </td>
        <td>
            <center><small><a href="rxnaivebayes.md" data-raw-source="[**View**](rxnaivebayes.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
</table>

<br />
<a name="compute"></a>

## Compute Context Functions

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
        <td width="180px"><code>RxHadoopMR</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates an in-data, file-based Hadoop compute context.</td>
        <td>
            <center><small><a href="rxhadoopmr.md" data-raw-source="[**View**](rxhadoopmr.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>RxSpark</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates an in-data, file-based Spark compute context. Computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark.</td>
        <td>
            <center><small><a href="rxspark.md" data-raw-source="[**View**](rxspark.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
    <td><code>rxSparkConnect</code></td>
    <td> </td>
    <td>Creates a persistent Spark compute context. </td>
    <td>
        <center><small><a href="rxspark.md" data-raw-source="[**View**](rxspark.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
    <td><code>rxSparkDisconnect</code></td>
    <td> </td>
    <td> Disconnects a Spark session and return to a local compute context.</td>
    <td>
        <center><small><a href="rxspark.md" data-raw-source="[**View**](rxspark.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td><code>rxInstalledPackages</code></td>
        <td> </td>
        <td>Returns the list of installed packages for a compute context.</td>
        <td><center><small><a href="rxinstalledpackages.md" data-raw-source="[**View**](rxinstalledpackages.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxFindPackage</code></td>
        <td> </td>
        <td>Returns the path to one or more packages for a compute context.</td>
        <td><center><small><a href="rxfindpackage.md" data-raw-source="[**View**](rxfindpackage.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
</table>


<br/>
<a name="data"></a>

## Data Source Functions

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
        <td width="150px"><code>RxXdfData</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small><a href="rxxdfdata.md" data-raw-source="[**View**](rxxdfdata.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>RxTextData</code></td>
        <td>
            <center><img src="./media/revoscaler-hadoop-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small><a href="rxtextdata.md" data-raw-source="[**View**](rxtextdata.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>    <tr>
        <td><code>RxHiveData</code></td>
        <td> </td>
        <td>Generates a Hive Data Source object.</td>
        <td>
            <center><small><a href="rxsparkdata.md" data-raw-source="[**View**](rxsparkdata.md)"><strong>View</strong></a><center></small></td>
        </tr>
        <tr>
        <td><code>RxParquetData</code></td>
        <td> </td>
        <td>Generates a Parquet Data Source object.</td>
        <td>
            <center><small><a href="rxsparkdata.md" data-raw-source="[**View**](rxsparkdata.md)"><strong>View</strong></a><center></small></td>
        </tr>
        <tr>
        <td><code>rxSparkDataOps</code> </td>
        <td> </td>
        <td>Lists cached <code>RxParquetData</code> or <code>RxHiveData</code> data source objects. </td>
        <td>
            <center><small><a href="rxsparkdataops.md" data-raw-source="[**View**](rxsparkdataops.md)"><strong>View</strong></a><center></small></td><br/>        </tr>
        <tr>
        <td><code>rxSparkRemoveData</code></td>
        <td> </td>
        <td>Removes cached <code>RxParquetData</code> or <code>RxHiveData</code> data source objects.</td>
        <td>
            <center><small><a href="rxsparkdataops.md" data-raw-source="[**View**](rxsparkdataops.md)"><strong>View</strong></a><center></small></td>
        </tr>
</table>


<br />
## High Performance Computing and Distributed Computing Functions

The Hadoop compute context has a number of helpful functions used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../../r/how-to-revoscaler-distributed-computing.md).


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
        <td width="150px"><code>rxExec</code></td>
        <td> </td>
        <td>Run an arbitrary R function on nodes or cores of a cluster.</td>
        <td>
            <center><small><a href="rxexec.md" data-raw-source="[**View**](rxexec.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGetJobStatus</code></td>
        <td> </td>
        <td>Get the status of a non-waiting distributed computing job.</td>
        <td>
            <center><small><a href="rxgetjobresults.md" data-raw-source="[**View**](rxgetjobresults.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGetJobResults</code></td>
        <td> </td>
        <td>Get the return object(s) of a non-waiting distributed computing job.</td>
        <td>
            <center><small><a href="rxgetjobresults.md" data-raw-source="[**View**](rxgetjobresults.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGetJobOutput</code></td>
        <td> </td>
        <td>Get the console output from a non-waiting distributed computing job.</td>
        <td>
            <center><small><a href="rxgetjoboutput.md" data-raw-source="[**View**](rxgetjoboutput.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGetJobs</code></td>
        <td> </td>
        <td>Get the available distributed computing job information objects.</td>
        <td>
                <center><small><a href="rxgetjobs.md" data-raw-source="[**View**](rxgetjobs.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
</table>





<br />
## Hadoop Convenience Functions

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
        <td width="200px"><code>rxHadoopCommand</code></td>
        <td width="15px"> </td>
        <td>Execute an arbitrary Hadoop command. Allows you to run basic Hadoop commands.</td>
        <td>
            <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopVersion</code></td>
        <td> </td>
        <td>Return the current Hadoop version.</td>
        <td>
            <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopCopyFromClient</code></td>
        <td> </td>
        <td>Copy a file from a remote client to the Hadoop cluster&#39;s local file system, and then to HDFS.</td>
        <td>
            <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopCopyFromLocal</code></td>
        <td> </td>
        <td>Copy a file from the native file system to HDFS. Wraps the Hadoop <code>fs -copyFromLocal</code> command.</td>
        <td>
            <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopCopy</code></td>
        <td> </td>
        <td>Copy a file in the Hadoop Distributed File System (HDFS). Wraps the Hadoop <code>fs -cp</code> command.</td>
        <td>
                <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopRemove</code></td>
        <td> </td>
        <td>Remove a file in HDFS. Wraps the Hadoop <code>fs -rm</code> command.</td>
        <td>
                <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopListFiles</code></td>
        <td> </td>
        <td>List files in an HDFS directory. Wraps the Hadoop <code>fs -ls</code> or <code>fs -lsr</code> command.</td>
        <td>
                <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopMakeDir</code></td>
        <td> </td>
        <td>Make a directory in HDFS. Wraps the Hadoop <code>fs -mkdir</code> command.</td>
        <td>
                <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopMove</code></td>
        <td> </td>
        <td>Move a file in HDFS. Wraps the Hadoop <code>fs -mv</code> command.</td>
        <td>
                <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxHadoopRemoveDir</code></td>
        <td> </td>
        <td>Remove a directory in HDFS. Wraps the Hadoop <code>fs -rmr</code> command.</td>
        <td>
                <center><small><a href="rxhadoopcommand.md" data-raw-source="[**View**](rxhadoopcommand.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
</table>
