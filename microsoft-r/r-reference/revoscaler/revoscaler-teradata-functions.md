---

# required metadata
title: "Teradata RevoScaleR Functions (Machine Learning Server and Microsoft R) "
description: "RevoScaleR Functions for a Teradata compute context"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "01/29/2018"
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

# RevoScaleR Functions for Teradata

Microsoft R Server 9.1 became the last release to have new features for Teradata as a computing platform for RevoScaleR functions. Although Teradata in-database compute context and supporting functions continue to be available in the 9.3 release as part of the [service support policy](../../resources-servicing-support.md) for Microsoft R Server 9.1, there are currently no plans to enhance the Teradata compute context in future Machine Learning Server releases.

Data import using the RxTeradata data source object remains unchanged. If you have existing data in a Teradata appliance, you can create an RxTeradata object to import your data into Machine Learning Server. There are no deprecation announcements for either RxTeradata or RxTeradata-class.

For backwards compatibility purposes, this page presents a curated list of functions specific to the Teradata DB compute contexts, as well as those that may not be fully supported.

These functions can be called directly from the command line.

For guidance on using these functions, see the [*RevoScaleR Teradata DB Getting Started Guide*](../../r/how-to-revoscaler-teradata.md).

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
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output data) from an .xdf file or a data frame.</td>
        <td>
            <center><small>[**View**](rxdatastep.md)<center></small></td>
    </tr>
    <tr>
        <td width="160px">`RxXdfData`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small>[**View**](rxxdfdata.md)<center></small></td>
    </tr>    
    <tr>
        <td>`RxTextData`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small>[**View**](rxtextdata.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetInfo`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Retrieves summary information from a data source or data frame.</td>
        <td>
            <center><small>[**View**](rxgetinfoxdf.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetVarInfo`</td>
        <td> </td>
        <td>Retrieves variable information from a data source or data frame.</td>
        <td>
            <center><small>[**View**](rxgetvarinfo.md)<center></small></td>
    </tr>
    <tr>
        <td>`rxGetVarNames`</td>
        <td> </td>
        <td>Retrieves variable names from a data source or data frame.</td>
        <td>
            <center><small>[**View**](rxgetvarnames.md)<center></small></td>
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
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Transform and subset data. Creates an .xdf file, a comma-delimited text file, or data frame in memory (assuming you have sufficient memory to hold the output) from an .xdf file or a data frame.</td>
        <td>
            <center><small>[**View**](rxdatastep.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxFactors`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Create or recode factor variables in a composite XDF file in HDFS. A new file must be written out.</td>
        <td>
            <center><small>[**View**](rxfactors.md)</small></center>
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
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Computes approximate quantiles for .xdf files and data frames without sorting.</td>
        <td>
            <center><small>[**View**](rxquantile.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxSummary`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Basic summary statistics of data, including computations by group. Writing by group computations to .xdf file not supported.</td>
        <td>
            <center><small>[**View**](rxsummary.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCrossTabs`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Formula-based cross-tabulation of data.</td>
        <td>
            <center><small>[**View**](rxcrosstabs.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCube`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Alternative formula-based cross-tabulation designed for efficient representation returning ‘cube’ results. Writing output to .xdf file not supported.</td>
        <td>
            <center><small>[**View**](rxcube.md)</small></center>
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
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Fits a linear model to data.</td>
        <td>
            <center><small>[**View**](rxlinmod.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxLogit`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Fits a logistic regression model to data.</td>
        <td>
            <center><small>[**View**](rxlogit.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGlm`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Fits a generalized linear model to data.</td>
        <td>
            <center><small>[**View**](rxglm.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxCovCor`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Calculate the covariance, correlation, or sum of squares / cross-product matrix for a set of variables.</td>
        <td>
            <center><small>[**View**](rxcovcor.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxDTree`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Fits a classification or regression tree to data.</td>
        <td>
            <center><small>[**View**](rxdtree.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxBTrees`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Fits a classification or regression decision forest to data using a stochastic gradient boosting algorithm.</td>
        <td>
            <center><small>[**View**](rxbtrees.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxDForest`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Fits a classification or regression decision forest to data.</td>
        <td>
            <center><small>[**View**](rxdforest.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxPredict`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Calculates predictions for fitted models. Output must be an XDF data source.</td>
        <td>
            <center><small>[**View**](../microsoftml/rxpredict.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxKmeans`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Performs k-means clustering.</td>
        <td>
            <center><small>[**View**](rxkmeans.md)</small></center>
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
        <td width="180px">`RxTeradata`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Creates a Teradata data source object.</td>
        <td>
            <center><small>[**View**](rxteradata.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxInTeradata`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Creates an in-database compute context for Teradata.</td>
        <td>
            <center><small>[**View**](rxinteradata.md)</small></center>
        </td>
    </tr>
    <tr>
        <td width="180px">`rxSetComputeContext`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Sets a compute context.</td>
        <td>
            <center><small>[**View**](rxsetcomputecontext.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetComputeContext`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Gets the current compute context.</td>
        <td>
            <center><small>[**View**](rxsetcomputecontext.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxInstalledPackages`</td>
        <td> </td>
        <td>Returns the list of installed packages for a compute context.</td>
        <td><center><small>[**View**](rxinstalledpackages.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxFindPackage`</td>
        <td> </td>
        <td>Returns the path to one or more packages for a compute context.</td>
        <td><center><small>[**View**](rxfindpackage.md)</small></center>
        </td>
    </tr>
</table>


<br/>
<a name="data"></a>

##Data Source Functions

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
        <td width="150px">`rxTeradataTableExists`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>

        <td>Check for the existence of a database table or object.</td>
        <td>
            <center><small>[**View**](rxteradatasql.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxTeradataDropTable`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Drop a table.</td>
        <td>
            <center><small>[**View**](rxteradatasql.md)</small></center>
        </td>
    </tr>    
    <tr>
        <td width="150px">`RxXdfData`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>

        <td>Creates an efficient XDF data source object.</td>
        <td>
            <center><small>[**View**](rxxdfdata.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`RxTextData`</td>
        <td>
            <center>![-](./media/revoscaler-teradata-functions/award.png)</center>
        </td>
        <td>Creates a comma delimited text data source object.</td>
        <td>
            <center><small>[**View**](rxtextdata.md)</small></center>
        </td>
    </tr>
</table>


<br />
##High Performance Computing and Distributed Computing Functions

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
        <td width="150px">`rxExec`</td>
        <td> </td>
        <td>Run an arbitrary R function on nodes or cores of a cluster.</td>
        <td>
            <center><small>[**View**](rxexec.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobStatus`</td>
        <td> </td>
        <td>Get the status of a non-waiting distributed computing job.</td>
        <td>
            <center><small>[**View**](rxgetjobresults.md))</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobResults`</td>
        <td> </td>
        <td>Get the return object(s) of a non-waiting distributed computing job.</td>
        <td>
            <center><small>[**View**](rxgetjobresults.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobOutput`</td>
        <td> </td>
        <td>Get the console output from a non-waiting distributed computing job.</td>
        <td>
            <center><small>[**View**](rxgetjoboutput.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetJobs`</td>
        <td> </td>
        <td>Get the available distributed computing job information objects.</td>
        <td>
                <center><small>[**View**](rxgetjobs.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetAvailableNodes`</td>
        <td> </td>
        <td>Get all the available nodes on a distributed compute context.</td>
        <td>
            <center><small>[**View**](rxgetavailablenodes.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxGetNodeInfo`</td>
        <td> </td>
        <td>Get information on nodes specified for a distributed compute context.</td>
        <td>
            <center><small>[**View**](rxgetnodeinfo.md)</small></center>
        </td>
    </tr>
    <tr>
        <td>`rxPingNodes`</td>
        <td> </td>
        <td>Test round trip from user through computation node(s) in a cluster or cloud.</td>
        <td>
            <center><small>[**View**](rxpingnodes.md)</small></center>
        </td>
    </tr>    
</table>
