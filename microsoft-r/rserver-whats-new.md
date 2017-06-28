---

# required metadata
title: "What's New in Microsoft R Server 9.1"
description: "Updates, improvements, and changes in this release of Microsoft R Server."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/20/2017"
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

# What's New in R Server 9.1

This release of R Server, built on open source R 3.3.3, includes new and updated packages, extending R Server through machine learning capabilities, operationalization enhancements with real-time scoring and dynamic scaling of VMs, and integration with sparklyr.  


Also, check out our [blog announcement post](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/) for this release.

<a name="machinelearning"></a>

## Machine Learning enhancements

In 9.1, the MicrosoftML algorithms are portable and distributed to run on Linux, Windows, and the most popular distributions of Hadoop (Cloudera, Hortonworks, MapR). Here are highlights of what you can do with this release:

-  Use **pre-trained deep neural network models** for **sentiment analysis** and **image featurization**. For instructions on how to install these models, see [How to install and deploy pre-trained machine learning models with MicrosoftML](deploy-pretrained-microsoftml-models.md). For quickstarts that show how to use pretrained models for sentiment analysis and image featurization, see [Samples for MicrosoftML](microsoftml-quickstarts.md).
-  Run MicrosoftML transforms and algorithms with **Apache Spark on a HDInsight cluster** for scalable and extremely high performance data management, analysis, and visualization. For installation instructions, see [Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](rserver-install-cloudera.md). For a tutorial walking you through the process, see [Practice data import and exploration on Apache Spark](scaler-spark-getting-started.md).
-  Deploy **Ensemble methods** that use a combination of learning algorithms to provide better predictive performance than the algorithms could individually. The approach is used primarily in the Hadoop/Spark environment for training across a multi-node cluster. But it can also be used in a single-node/local context.
-  Perform **real-time scoring in SQL Server** to execute R scripts from T-SQL without having to call an R interpreter. Scoring a model in this way reduces the overhead of multiple process interactions and provides much faster prediction performance in enterprise production scenarios. 
-	Create **text classification** models for problems such as sentiment analysis and support ticket classification. 
-	Train deep neural nets with **GPU acceleration** in order to solve complex problems such as retail image classification and handwriting analysis.
-	Work with **high-dimensional categorical data** for scenarios like online advertising click-through prediction.
-	Solve many other **common machine learning tasks** such as churn prediction, loan risk analysis, and demand forecasting using state-or-the-art, fast and accurate algorithms.
- **Train models 2x faster** than logistic regression with the Fast Linear Algorithm (SDCA).
- **Train multilayer custom nets** on GPUs up to 8x faster with GPU acceleration for Neural Nets.
- Reduce training time up to 10x while still retaining model accuracy using **feature selection**.


<a name="sparkinterop"></a>

## Interoperability with sparklyr 

Within the same R script, you can mix and match functions from RevoScaleR and Microsoft ML packages with popular open source packages like sparklyr and through it, H2O. To learn more, see [Use R Server with sparklyr (step-by-step examples)](microsoft-r-get-started-spark-interop.md).

<a name="rxexecby"></a>

## "Pleasingly Parallel" processing with rxExecBy on Spark and SQL Server

Demand is growing for the ability to efficiently handle a large number of small models at scale. In this use case, modeling (or processing) occurs over data collected for singular entities (such as devices, people, products, days) where the per-entity data sets are relatively small in comparison with big data scenarios so often typical of R workloads. 

In this release, you can leverage the new `rxExecBy` function against unordered data, have it sorted and grouped into partitions (one partition per entity), and then processed in parallel using whatever function or operation you want to run. For example, to project the health outcomes of individuals in a fitness study, you could run a prediction model over data collected about each person. Supported compute context includes `RxSpark` and `RxInSQLServer`.  

To learn more, see [Quickstart: Parallel processing on partitioned data with rxExecBy](quickstart-rxexecby.md).

## Operationalizing analytics 
 
**Role-based access control** to analytical web services: Administrators can define authorization roles to give web service permissions to groups of users with authorization roles.  These roles determine who can publish, update, and delete their own web services, those who can also update and delete the web services published by other users, and who can only list and consume web services. Users are assigned to roles using the security groups defined in your organization's Active Directory /LDAP or Azure Active Directory server.  Learn more about [roles](./operationalize/security-roles.md).
 
**Scoring perform boosts with real time scoring**: Web services that are published with a supported R model object on Windows platforms can now benefit from an extra realtime performance boost and lower latency. Simply use a supported model object and set the  `serviceType = Realtime` argument at publish time. Expanded platform support in future releases. Learn more about [`Realtime` web services](./operationalize/data-scientist-manage-services.md#realtime).
 
**Asynchronously batch processing for large input data**: Web services can now be consumed asynchronously via batch execution. The Asynchronous Batch approach involves the execution of code without manual intervention using multiple asynchronous API calls on a specific web service sent as a single request to R Server. Previously, web services could only be consumed using [the Request-Response method](./operationalize/howto-consume-web-service-interact-in-r.md#consume-service). Learn more about [asynchronous batch consumption](./operationalize/data-scientist-batch-mode.md).

**Autoscaling of a grid of web and compute nodes on Azure**. A script template will be offered to easily spin up a set of R Server VMs in Azure, configure them as a grid for operationalizing analytics and remote execution. This grid can be scaled up or down based on CPU usage.


Read about the [differences between DeployR and R Server 9.x Operationalization](https://blogs.msdn.microsoft.com/rserver/2017/05/11/1885/).

## Executing remotely 
Asynchronous remote execution is now supported using the `mrsdeploy` R package.  To continue working in your development environment during the remote script execution, execute your R script asynchronously using the `async` parameter. This is particularly useful when you are running scripts that have long execution times. Learn more about [asynchronous remote execution](./operationalize/remote-execution.md#async).

## R Server deployment and administration in Cloudera Manager

R Server for Hadoop installation is improved for Cloudera distribution including Apache Hadoop (CDH) on RedHat Linux (RHEL) 7.x. On this installation configuration, you can easily deploy, activate, deactivate, or rollback a distribution of R Server using Cloudera Manager. For details, see [Install R Server on CDH](rserver-install-cloudera.md).

## SQL Server R Services and Machine Learning Services

In SQL Server 2016, Microsoft introduced SQL Server R Services, a feature that supports enterprise-scale data science by integrating the R language with SQL Server database engine.

In SQL Server 2017, machine learning becomes even more powerful, with addition of support for the popular Python language. To reflect the support for multiple languages, as of CTP 2.0, SQL Server R Services has also been renamed as Machine Learning Services (In-Database). To read up on the latest changes in CTP 2.0 release of SQL Server 2017, see [What's new for R in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r-services/what-s-new-in-sql-server-r-services) in the SQL Server product documentation. 

<a name="rclient333-package-updates"></a>

## New and updated packages

The following packages have been updated in Microsoft R Server and Microsoft R Client:

+ The **RevoScaleR** package has been updated to version 9.1.0 
+ The **mrsdeploy** package has been updated to version 1.1.0
+ The **curl** package has been updated to version 2.3
+ The **jsonlite** package has been updated to version 1.3 

**RevoScaleR 9.1.0 Function Updates**

| Function | Status | Changes |
|----------|--------|---------|
| [`rxExecBy`](r-reference/revoscaler/rxexecby.md) | New | Enables parallel processing of partitioned data in Spark and SQL Server compute contexts. |
| [`rxExecByPartition`](r-reference/revoscaler/rxexecbypartition.md) | New | Run analytics computation in parallel on individual data partitions split from an input data source based on the specified variables.|
| [`rxDataStep`](r-reference/revoscaler/rxdatastep.md) | Enhanced | Multithreaded support. |
| [`rxGetPartitions`](r-reference/revoscaler/rxgetpartitions.md)  | New | Gets the partitions of a previously partitioned Xdf data source. |
| [`rxGetSparklyrConnection`](r-reference/revoscaler/rxgetsparklyrconnection.md)  | New | Get a Spark compute context with sparklyr interop.  |
| [`rxImport`](r-reference/revoscaler/rximport.md)  | Enhanced | Multithreaded support. |
| `rxMerge` | Enhanced | Merging data frames in Spark compute context. |
| [`RxOrcData`](scaler/packagehelp/RxSparkData.md) | New | Create data sets based on data stored in Optimized Row Columnar (ORC) format.|
| [`rxSerializeModel`](r-reference/revoscaler/rxserializemodel.md)  | New | Serializes a RevoScaleR model so that it can be saved to disk or loaded into a SQL Server database table. Serialized models are requred for real-time scoring. |
| [`rxSparkCacheData`](r-reference/revoscaler/rxsparkcachedata.md) | New | Set the Cache flag in a Spark compute context.|
| [`rxSyncPackages`](scaler/packagehelp/rxSyncPackages.md)  | New | Copies packages from a user table in a SQL Server database to a location on the file system so that R scripts can call functions in those packages. |

**mrsdeploy 1.1.0 Function Updates**

| Function | Status | Changes |
|----------|--------|---------|
|[`getValidatedServiceType`](r-reference/mrsdeploy/getvalidatedservicetype.md) | New | Test to see if supplied service type is supported.|
|[`print.serviceDetails`](r-reference/mrsdeploy/print-servicedetails.md) | New| Defines the R print generic for `serviceDetails` during a  `listServices()`. |
|[`print.snapshotDetails`](r-reference/mrsdeploy/print-snapshotdetails.md) | New | Defines the R print generic for `snapshotDetails` during a  `listSnapshots()`. |
|[`remoteScript`](r-reference/mrsdeploy/remotescript.md) | Enhanced | Includes a new `async` parameter to support asynchronous remote execution. |
|[`serviceTypes`](r-reference/mrsdeploy/servicetypes.md)  | New | Defines the enumerated list of the supported service types.|
|[`summary.serviceDetails`](r-reference/mrsdeploy/summary-servicedetails.md)  | New | Defines the R summary generic for `serviceDetails` during a  `listServices()`. |
|[`summary.snapshotDetails`](r-reference/mrsdeploy/summary-snapshotdetails.md)  | New | Defines the R summary generic for `snapshotDetails` during a  `listSnapshots()`. |

## Previous releases

If you haven't upgraded recently, you can review the feature announcments from the last several releases of Microsoft R Server to learn about cumulative updates.

### 9.0.1 Announcements

This release of R Server, built on open source R 3.3.2, includes new and updated packages, plus new operationalization features in the core engine. Key features in this release include the following:

+ [Overview of MicrosoftML algorithms](r-reference/microsoftml/microsoftml-package.md)
+ [Remote execution](operationalize/remote-execution.md)
+ [Web service deployment](operationalize/data-scientist-manage-services.md)

**Related Documents**

+ [Release notes for R Server](notes/r-server-notes.md) contains information about known issues, bug fixes, and behavior changes in existing features.
+ [What's new in R Client](notes/r-client-notes.md) contains information about new features for R Client.
+ [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx) contains information about new features for SQL Server R Services (version 9.0).

#### New and updated packages in 9.0.1

**Microsoft Machine Learning algorithms (MicrosoftML package)** is a collection of functions for incorporating machine learning into R code or script that executes on R Server and R Client. It's available in the following Microsoft R products: R Server for Windows, R Client for Windows, and [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx). Availability for Linux, Hadoop, and [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/) is projected for the first quarter of 2017.

To learn more, see [Overview of MicrosoftML algorithms](r-reference/microsoftml/microsoftml-package.md).

**mrsdeploy package** is new in this release and available on all [platforms supporting operationalization](rserver-install-supported-platforms.md). Functions provide remote execution on a R Server 9.0.1 instance, and the ability to publish, and subsequently manage, an R code block as a web service.

To learn more, see [mrsdeploy Function Reference](r-reference/mrsdeploy/mrsdeploy-package.md).

**olapR package** lets you run MDX queries and connect directly to OLAP cubes on SQL Server 2016 Analysis Services from your R solution. You can manually create and then paste in an MDX query, or use an R-style API to choose a cube, axes, and slicers. This package is available on R Server for Windows, SQL Server R Services, and an R Server (Standalone) installation through SQL Server.

To learn more, see [Using Data from OLAP Cubes in R](https://msdn.microsoft.com/library/mt790485.aspx).

**RevoScaleR Package** is updated to include [support for **Spark 2.0**](#bkmk_Spark). For a list of all functions, see [RevoScaleR Function Reference](r-reference/revoscaler/revoscaler.md).

|Function | Description |
|--|--|
|`RxHiveData`|Generate a Hive Data Source object.|
|`RxParquetData `|Generate a Parquet Data Source object.|
|`rxSparkConnect` | Create a persistent Spark compute context. |
|`rxSparkDisconnect `| Disconnect a Spark session and return to a local compute context. |
|`rxSparkListData` | List cached `RxParquetData` or `RxHiveData` data source objects.|
|`rxSparkRemoveData`|Remove cached `RxParquetData` or `RxHiveData` data source objects.|

> [!NOTE]
> Although ScaleR jobs only execute on Spark 2.0 if you have [R Server 9.0.1 for Hadoop](rserver-install-hadoop.md), you can create solutions containing Hive, Parquet, and Spark-related functions in R Client.

#### General updates in 9.0.1

<a name="operationalize"></a>
##### Operationalization features

Formerly known as DeployR, the operationalization feature is now fully integrated into R Server, with a new ASP .NET core bringing improved support from Microsoft. After installing R Server on select platforms, you'll have everything you need to enable operationalization and [configure](operationalize/configuration-initial.md) R Server to host R analytics web services and remote R sessions.  For details on which platforms, see [Supported platforms](rserver-install-supported-platforms.md).

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization. The operationalization feature in Microsoft R Server provides the tools to deploy R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. R Server turns your R scripts into analytics web services, so R code can be easily executed by applications running on a secure server.

An operationalized R server offers the ability to host and bundle R analytics into web services with minimal code changes. R Server accepts interactive commands through [mrsdeploy functions](r-reference/mrsdeploy/mrsdeploy-package.md) for remote execution and web service deployment. Data scientists can use `mrsdeploy` functions  on the command line. Application developers can write code to instrument equivalent operations and integrate web services into their applications using [easy-to-consume Swagger-based APIs](operationalize/api.md) in any programming language.

The operationalization feature can be configured [on a single machine](operationalize/configuration-initial.md#onebox). It can also be [scaled](operationalize/configure-enterprise.md) for business-critical applications with multiple web and compute nodes on clustered servers for load balancing. This gives you the ability to pipeline data streams that are subsequently transformed, analyzed, and visualized into an R analytics web service.

In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. Operationalization with R Server also supports production-grade workloads and seamless integration with popular [enterprise security solutions](operationalize/admin-get-started.md#security).

For feature information and next steps, see [Operationalization with R Server](operationalize/about.md) and [Configure operationalization on R Server](operationalize/configuration-initial.md).

> [!NOTE]
> In the context of operationalization, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure. Feature support is limited to a subset of the [supported R Server platforms](rserver-install-supported-platforms.md) has the list.

<a name="8vs9"></a>
The following table presents some of the main differences between Microsoft R Server 9.x configured to operationalize analytics and the add-on DeployR 8.0.5 which was available in R Server 8.0.5.

>[!Important]
>R Server configured to operationalize analytics is **not backwards compatible** with DeployR 8.x. There is no migration path as the APIs are completely new and the data stored in the database is structured differently. 

Read about the [differences between DeployR and R Server 9.x Operationalization](https://blogs.msdn.microsoft.com/rserver/2017/05/11/1885/).

##### R Server 9.0.1 for Linux

This release now supports Ubuntu 14.04 and 16.04 on premises. For installation instructions, see [Install R Server for Linux](rserver-install-linux-server.md).

<a name="bkmk_Spark"></a>
#### R Server for Hadoop (MapReduce and Spark)

+ Support for Spark 1.6 and 2.0.
+ Support for Spark DataFrames through `RxHiveData` and `RxParquetData` in ScaleR when using an `RxSpark` compute context in ScaleR:

  ~~~~
    hiveData <- RxHiveData("select * from hivesampletable", ...)
    pqData <- RxParquetData('/share/claimsParquet', ...)
  ~~~~

Additional new ScaleR functions for Spark 2.0:

+ Manage Spark persistent sessions: `rxSparkConnect`, `rxSparkDisconnect`
+ Manage data in Spark DataFrames : `rxSparkListData`, `rxSparkRemoveData`

For installation instructions, see [Install R Server for Hadoop](rserver-install-hadoop.md). For ScaleR help, see [ScaleR Function Reference](r-reference/revoscaler/revoscaler.md).

##### R Server 9.0.1 for Windows

As noted, installation of R Server or R Client on Windows delivers the new [Overview of MicrosoftML algorithms](r-reference/microsoftml/microsoftml-package.md) for machine learning.

Additionally, this release adds a simplified setup program for an  Server installation on Windows. This setup is in addition to SQL Server Setup, which continues to be a viable option for installation.

Features in the 9.0.1 release are currently only available through simplified setup. In contrast, SQL Server 2016 Setup installs the 8.0.3 version of R Server for Windows, and SQL Server 2017 installs the 9.0.0 version. The 9.0.0 version offers the new MicrosoftML and olapR packages, but not operationalization. For a description of the features in 9.0.0, see [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx).

For installation and upgrade instructions, see [Install R Server for Windows](rserver-install-windows.md).

> [!NOTE]
> Although the installation experience is changing, licensing is not. R Server for Windows remains a SQL Server enterprise feature, even when installed outside of SQL Server Setup. A SQL Server enterprise license is required for the enterprise edition of R Server for Windows.

**New service and support options for R Server for Windows**

R Server for Windows can be serviced under the [Modern Lifecycle policy](https://support.microsoft.com/en-us/help/447912) or under [SQL Server's support policy (search for "SQL Server 2016" on this page)](https://support.microsoft.com/en-us/lifecycle). For support information for R Server in general, see [Support for Microsoft R Server Versions](rserver-servicing-support.md).

+ Modern lifecycle policy is designed for rapid release cycles. Individual versions age out sooner, but newer features roll out more frequently. The Modern lifecycle policy is in effect when you use simplified setup to install R Server for Windows.

+ SQL Server support policy supports released versions over a longer time frame, but updates are less frequent. This support policy is in effect when you use SQL Server Setup to install R Server for Windows.

+ In the upcoming SQL Server 2017 CTP 1.1 release, you will be able to unbind your SQL Server R Services instance and replace it with a 9.0.1 version that offers operationalization features and the Modern Lifecycle Support policy. Check [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx) for the latest information on CTP 1.1 when it becomes available.


### 8.0.5 Announcements

+ On **Linux**:
  + Support for RedHat RHEL 7.x has been added.

  + Installers are now composed of RPM packages, which can be installed via a top-level install script or as individual RPM packages. This can be convenient for Enterprise IT departments managing extensive deployments.

+ On **Hadoop**:

  + Installation on Hadoop clusters has been simplified to eliminate manual steps.

  + New support Hadoop on SUSE 11 and Hadoop distributions (Cloudera CDH 5.5-5.7, Hortonworks HDP 2.4, MapR 5.0-5.1)

  + A new distributed compute context `RxSpark` is available, in which computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark. This provides up to a 7x performance boost compared to `RxHadoopMR`.

  + New Hadoop diagnostic tool to collect status of MRS and dependencies on all nodes (available through a CSS Support request)

  + Hadoop user directories in HDFS and Linux are now created automatically as needed.

  + New Hadoop administrator script to clean-up orphaned HDFS and Linux user directories.

+ On **Teradata**:

  + New option has been added to `rxPredict` to insert into an existing table.

  + New option has been added for use of LDAP authentication with a TPT connection.

+ **DeployR Enterprise** includes the following changes and improvements:

    + Deployr Enterprise is more secure than ever with improved Web security features for better protection against malicious attacks, improved installation security, and improved Security Policy Management.

    + DeployR Enterprise now relies on an H2 database by default and allows you to easily use a SQL Server or PostgreSQL database instead to fit your production environment.

    + DeployR Enterprise now has a simplified installer for a better customer experience.

    + The XML format for data exchange is deprecated, and will be removed from future versions of DeployR.

    + The API has been updated. [See the change history.](deployr-api-reference.md#805)

    + This release is of DeployR Enterprise only.

### 8.0.3 Announcements

+ R Server 8.0.3 is a Windows-only, SQL-Server-only release. It is installed using SQL Server 2016 setup. Version 8.0.3 is succeeded by version 9.0.1 in SQL Server. Features are cumulative so what shipped in 8.0.3 is still available in 9.0.1. For a description of features, see [What's new in SQL Server R Services](https://msdn.microsoft.com/library/mt604847.aspx).


### 8.0.0 Announcements

+ Revolution R Open is now Microsoft R Open, and Revolution R Enterprise is now generically known as Microsoft R Services, and specifically Microsoft R Server for Linux platforms.

+ Microsoft R Services installs on top of an enhanced version of R 3.2.2, Microsoft R Open for Revolution R Enterprise 8.0.0 (on Windows) or Microsoft R Open for Microsoft R Server (on Linux)

+ Installation of Microsoft R Services has been simplified from three installers to two: the new Microsoft R Open for Revolution R Enterprise/Microsoft R Server installer combines Microsoft R Open with the GPL/LGPL components needed to support Microsoft R Services, so there is no need for the previous “Revolution R Connector” install.

+ RevoScaleR includes:
    + New Fuzzy Matching Algorithms: The new rxGetFuzzyKeys and rxGetFuzzyDist functions provide access to fuzzy matching
algorithms for cleaning and analyzing text data.
    + Support for Writing in ODBC Data Sources. The RxOdbcData data source now supports writing
    + Bug Fixes:
        + When using rxDataStep, new variables created in a transformation no longer inherit the rxLowHigh attribute of the variable used to create them.
        + rxGetInfo was failing when an extended class of RxXdfData was used.
        + rxGetVarInfo now respects the `newName` element of colInfo for non-xdf data sources.
        + If `inData` for rxDataStep is a non-xdf data source that contains a colInfo specification using `newName`, the `newName` should now be used for `varsToKeep` and `varsToDrop`.
    + Deprecated and Defunct.
        + `NIEDERR` is no longer supported as a type of random number generator.
        + `scheduleOnce` is now defunct for rxPredict.rxDForest and rxPredict.rxBTrees.
        + The compute context `RxLsfCluster` is now defunct.
        + The compute context `RxHpcServer` is now deprecated

+ DevelopR - The R Productivity Environment (the IDE provided with Revolution R Enterprise on Windows) is not deprecated, but it will be removed from future versions of Microsoft R Services.

+ RevoMPM, a Multinode Package Manager, is now defunct, as it was deemed redundant. Numerous distributed shells are available, including pdsh, fabric, and PyDSH. More sophisticated provisioning tools such as Puppet, Chef, and Ansible are also available. Any of these can be used in place of RevoMPM.

## See Also

 [Introduction to R Server](rserver.md) 
 [Install R Server on Windows](rserver-install-windows.md)  
 [Install R Server on Linux](rserver-install-linux-server.md)  
 [Install R Server on Hadoop](rserver-install-hadoop.md)