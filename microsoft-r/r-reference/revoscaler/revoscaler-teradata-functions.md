---

# required metadata
title: "Teradata RevoScaleR Functions (Machine Learning Server and Microsoft R) "
description: "RevoScaleR Functions for a Teradata compute context"
keywords: ""
author: "chuckheinzelman"
ms.author: "charlhe"
manager: "cgronlun"
ms.date: "01/29/2018"
ms.topic: "reference"
ms.prod: "sql-non-specified"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
ms.technology: machine-learning-server

---

# RevoScaleR Functions for Teradata

Microsoft R Server 9.1 was the last release to include the RxInTeradata and RxInTeradata-class functions for creating a remote compute context on a Teradata appliance. This release is fully supported as described in the [service support policy](../../resources-servicing-support.md), but the Teradata compute context is not available in Machine Learning Server 9.2.1 and later.

Data import using the RxTeradata data source object remains unchanged. If you have existing data in a Teradata appliance, you can create an [RxTeradata object](rxteradata.md) to import your data into Machine Learning Server.

For backwards compatibility purposes, this page presents a curated list of functions specific to the Teradata DB compute contexts, as well as those that may not be fully supported.

These functions can be called directly from the command line. For guidance on using these functions, see the [*RevoScaleR Teradata DB Getting Started Guide*](../../r/how-to-revoscaler-teradata.md).

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
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from an .xdf file or a data frame.</td>
        <td>
            <center><small><a href="rxdatastep.md" data-raw-source="[**View**](rxdatastep.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td width="160px"><code>RxXdfData</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small><a href="rxxdfdata.md" data-raw-source="[**View**](rxxdfdata.md)"><strong>View</strong></a><center></small></td>
    </tr><br/>    <tr>
        <td><code>RxTextData</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small><a href="rxtextdata.md" data-raw-source="[**View**](rxtextdata.md)"><strong>View</strong></a><center></small></td>
    </tr>
    <tr>
        <td><code>rxGetInfo</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
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
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output) from an .xdf file or a data frame.</td>
        <td>
            <center><small><a href="rxdatastep.md" data-raw-source="[**View**](rxdatastep.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxFactors</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
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
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Computes approximate quantiles for .xdf files and data frames without sorting.</td>
        <td>
            <center><small><a href="rxquantile.md" data-raw-source="[**View**](rxquantile.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxSummary</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported.</td>
        <td>
            <center><small><a href="rxsummary.md" data-raw-source="[**View**](rxsummary.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxCrossTabs</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Formula-based cross-tabulation of data.</td>
        <td>
            <center><small><a href="rxcrosstabs.md" data-raw-source="[**View**](rxcrosstabs.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxCube</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
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
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a linear model to data.</td>
        <td>
            <center><small><a href="rxlinmod.md" data-raw-source="[**View**](rxlinmod.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxLogit</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a logistic regression model to data.</td>
        <td>
            <center><small><a href="rxlogit.md" data-raw-source="[**View**](rxlogit.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGlm</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a generalized linear model to data.</td>
        <td>
            <center><small><a href="rxglm.md" data-raw-source="[**View**](rxglm.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxCovCor</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.</td>
        <td>
            <center><small><a href="rxcovcor.md" data-raw-source="[**View**](rxcovcor.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxDTree</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a classification or regression tree to data.</td>
        <td>
            <center><small><a href="rxdtree.md" data-raw-source="[**View**](rxdtree.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxBTrees</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm.</td>
        <td>
            <center><small><a href="rxbtrees.md" data-raw-source="[**View**](rxbtrees.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxDForest</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Fits a classification or regression decision forest to data.</td>
        <td>
            <center><small><a href="rxdforest.md" data-raw-source="[**View**](rxdforest.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxPredict</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Calculates predictions for fitted models. Output must be an XDF data source.</td>
        <td>
            <center><small><a href="../microsoftml/rxpredict.md" data-raw-source="[**View**](../microsoftml/rxpredict.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxKmeans</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Performs k-means clustering.</td>
        <td>
            <center><small><a href="rxkmeans.md" data-raw-source="[**View**](rxkmeans.md)"><strong>View</strong></a></small></center>
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
        <td width="180px"><code>RxTeradata</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates a Teradata data source object.</td>
        <td>
            <center><small><a href="rxteradata.md" data-raw-source="[**View**](rxteradata.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>RxInTeradata</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates an in-database compute context for Teradata.</td>
        <td>
            <center><small><a href="rxinteradata.md" data-raw-source="[**View**](rxinteradata.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td width="180px"><code>rxSetComputeContext</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Sets a compute context.</td>
        <td>
            <center><small><a href="rxsetcomputecontext.md" data-raw-source="[**View**](rxsetcomputecontext.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGetComputeContext</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Gets the current compute context.</td>
        <td>
            <center><small><a href="rxsetcomputecontext.md" data-raw-source="[**View**](rxsetcomputecontext.md)"><strong>View</strong></a></small></center>
        </td>
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

Of course, not all data source types are available on all compute contexts.

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
        <td width="150px"><code>rxTeradataTableExists</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Check for the existence of a database table or object.</td>
        <td>
            <center><small><a href="rxteradatasql.md" data-raw-source="[**View**](rxteradatasql.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxTeradataDropTable</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Drop a table.</td>
        <td>
            <center><small><a href="rxteradatasql.md" data-raw-source="[**View**](rxteradatasql.md)"><strong>View</strong></a></small></center>
        </td>
    </tr><br/>    <tr>
        <td width="150px"><code>RxXdfData</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small><a href="rxxdfdata.md" data-raw-source="[**View**](rxxdfdata.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>RxTextData</code></td>
        <td>
            <center><img src="./media/revoscaler-teradata-functions/award.png" alt="-"/></center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small><a href="rxtextdata.md" data-raw-source="[**View**](rxtextdata.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
</table>


<br />
## High Performance Computing and Distributed Computing Functions

The Teradata compute context has a number of helpful functions used for high performance computing and distributed computing. Learn more about the entire set of functions in the [Distributed Computing guide](../../r/how-to-revoscaler-distributed-computing.md).


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
            <center><small><a href="rxgetjobresults.md" data-raw-source="[**View**](rxgetjobresults.md)"><strong>View</strong></a>)</small></center>
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
    <tr>
        <td><code>rxGetAvailableNodes</code></td>
        <td> </td>
        <td>Get all the available nodes on a distributed compute context.</td>
        <td>
            <center><small><a href="rxgetavailablenodes.md" data-raw-source="[**View**](rxgetavailablenodes.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxGetNodeInfo</code></td>
        <td> </td>
        <td>Get information on nodes specified for a distributed compute context.</td>
        <td>
            <center><small><a href="rxgetnodeinfo.md" data-raw-source="[**View**](rxgetnodeinfo.md)"><strong>View</strong></a></small></center>
        </td>
    </tr>
    <tr>
        <td><code>rxPingNodes</code></td>
        <td> </td>
        <td>Test round trip from user through computation node(s) in a cluster or cloud.</td>
        <td>
            <center><small><a href="rxpingnodes.md" data-raw-source="[**View**](rxpingnodes.md)"><strong>View</strong></a></small></center>
        </td>
    </tr><br/></table>
