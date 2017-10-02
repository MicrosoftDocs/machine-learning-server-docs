---

# required metadata
title: "Feature announcements in Microsoft R Server releases | Microsoft Docs"
description: "Feature announcements, improvements, and changes in past releases of Microsoft R Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/05/2017"
ms.topic: "article"
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

# Feature annoucements from previous releases

Microsoft R Server is subsumed by Machine Learning Server 9.2.1, which is built on features from earlier releases. If you have R Server 9.1 or earlier, this article enumerates features introduced in each version.

## R Server 9.1

Release announcement blog: https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/



| Feature                | Description |
|------------------------|----------------------|
| MicrosoftML package | R Function library in R Server and on Apache Spark on a HDInsight cluster. <br/><br/>**Create text classification models** for problems such as sentiment analysis and support ticket classification. <br/><br/>**Train deep neural nets** with GPU acceleration in order to solve complex problems such as retail image classification and handwriting analysis. <br/><br/>**Work with high-dimensional categorical data** for scenarios like online advertising click-through prediction.<br/><br/>**Solve common machine learning tasks** such as churn prediction, loan risk analysis, and demand forecasting using state-of-the-art, fast and accurate algorithms.<br/><br/>**Train models 2x faster** than logistic regression with the Fast Linear Algorithm (SDCA). <br/><br/>**Train multilayer custom nets** on GPUs up to 8x faster with GPU acceleration for Neural Nets. <br/><br/>**Reduce training time up** to 10x while still retaining model accuracy using feature selection. |
| Pretrained models | Deep neural network models for sentiment analysis and image featurization |
| Ensemble methods | Use a combination of learning algorithms to provide better predictive performance than the algorithms could individually. The approach is used primarily in the Hadoop/Spark environment for training across a multi-node cluster. But it can also be used in a single-node/local context. |
| MicrosoftML and T-SQL integration | Real-time scoring in SQL Server. Execute R scripts from T-SQL without having to call an R interpreter. Scoring a model in this way reduces the overhead of multiple process interactions and provides faster predictions. |
| sparklyr interoperability | Within the same R script, you can mix and match functions from RevoScaleR and Microsoft ML packages with popular open source packages like sparklyr and through it, H2O. To learn more, see [Use R Server with sparklyr (step-by-step examples)](r/tutorial-sparklyr-revoscaler.md). |
| RevoScaleR new functions | [`rxExecBy`](r-reference/revoscaler/rxexecby.md) enables parallel processing of partitioned data in Spark and SQL Server compute contexts. Leverage the new `rxExecBy` function against unordered data, have it sorted and grouped into partitions (one partition per entity), and then processed in parallel using whatever function or operation you want to run. For example, to project the health outcomes of individuals in a fitness study, you could run a prediction model over data collected about each person. Supported compute context includes `RxSpark` and `RxInSQLServer`.<br/><br/>[`rxExecByPartition`](r-reference/revoscaler/rxexecbypartition.md) for running analytics computation in parallel on individual data partitions split from an input data source based on the specified variables.<br/><br/>[`rxGetPartitions`](r-reference/revoscaler/rxgetpartitions.md) gets the partitions of a previously partitioned Xdf data source. <br/><br/>[`rxGetSparklyrConnection`](r-reference/revoscaler/rxgetsparklyrconnection.md) gets a Spark compute context with sparklyr interop.  <br/><br/>[`RxOrcData`](r-reference/revoscaler/rxsparkdata.md) creates data sets based on data stored in Optimized Row Columnar (ORC) format.<br/><br/>[`rxSerializeModel`](r-reference/revoscaler/rxserializemodel.md) serializes a RevoScaleR model so that it can be saved to disk or loaded into a SQL Server database table. Serialized models are requred for real-time scoring. <br/><br/>[`rxSparkCacheData`](r-reference/revoscaler/rxsparkcachedata.md) sets the Cache flag in a Spark compute context.<br/><br/>[`rxSyncPackages`](r-reference/revoscaler/rxsyncpackages.md) copies packages from a user table in a SQL Server database to a location on the file system so that R scripts can call functions in those packages. |
| RevoScaleR enhanced functions | [`rxDataStep`](r-reference/revoscaler/rxdatastep.md) adds multithreaded support. <br/><br/>[`rxImport`](r-reference/revoscaler/rximport.md) adds multithreaded support. <br/><br/>`rxMerge` for Merging data frames in Spark compute context. <br/><br/>
| Cloudera installation improvements | R Server for Hadoop installation is improved for Cloudera distribution including Apache Hadoop (CDH) on RedHat Linux (RHEL) 7.x. On this installation configuration, you can easily deploy, activate, deactivate, or rollback a distribution of R Server using Cloudera Manager.|
| Remote execution | Asynchronous remote execution is now supported using the mrsdeploy R package.  To continue working in your development environment during the remote script execution, execute your R script asynchronously using the `async` parameter. This is particularly useful when you are running scripts that have long execution times. Learn more about [asynchronous remote execution](r/how-to-execute-code-remotely.md#async). |
| Operationalizing analytics | **Role-based access control** to analytical web services: Administrators can define authorization roles to give web service permissions to groups of users with authorization roles.  These roles determine who can publish, update, and delete their own web services, those who can also update and delete the web services published by other users, and who can only list and consume web services. Users are assigned to roles using the security groups defined in your organization's Active Directory /LDAP or Azure Active Directory server.  Learn more about [roles](operationalize/configure-roles.md).<br/><br/>**Scoring perform boosts with real time scoring**: Web services that are published with a supported R model object on Windows platforms can now benefit from an extra realtime performance boost and lower latency. Simply use a supported model object and set the  `serviceType = Realtime` argument at publish time. Expanded platform support in future releases. Learn more about [`Realtime` web services](operationalize/how-to-deploy-web-service-publish-manage-in-r.md#realtime).<br/><br/>**Asynchronously batch processing for large input data**: Web services can now be consumed asynchronously via batch execution. The Asynchronous Batch approach involves the execution of code without manual intervention using multiple asynchronous API calls on a specific web service sent as a single request to R Server. Previously, web services could only be consumed using [the Request-Response method](operationalize/how-to-consume-web-service-interact-in-r.md#consume-service). Learn more about [asynchronous batch consumption](operationalize/how-to-consume-web-service-asynchronously-batch.md).<br/><br/>**Autoscaling of a grid of web and compute nodes on Azure**. A script template will be offered to easily spin up a set of R Server VMs in Azure, configure them as a grid for operationalizing analytics and remote execution. This grid can be scaled up or down based on CPU usage. <br/><br/>Read about the [differences between DeployR and R Server 9.x Operationalization](https://blogs.msdn.microsoft.com/rserver/2017/05/11/1885/). |

For more information about this release, see this [blog announcement for 9.1](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/).

## R Server 9.0.1

Release announcement blog: https://blogs.technet.microsoft.com/machinelearning/2016/12/07/introducing-microsoft-r-server-9-0/
 
This release of R Server, built on open source R 3.3.2, included new and updated packages, plus new operationalization features in the core engine. 

| Feature                | Description |
|------------------------|----------------------|
| R Server 9.0.1 for Linux | Supports Ubuntu 14.04 and 16.04 on premises. |
| R Server for Hadoop (MapReduce and Spark) | Support for Spark 1.6 and 2.0. Support for Spark DataFrames through `RxHiveData` and `RxParquetData` in RevoScaleR when using an `RxSpark` compute context in ScaleR: `hiveData <- RxHiveData("select * from hivesampletable", ...)` and `pqData <- RxParquetData('/share/claimsParquet', ...)` |
| R Server 9.0.1 for Windows | Includes MicrosoftML and olapR support. Adds a simplified setup program, in addition to SQL Server Setup, which continues to be a viable option for installation. Features in the 9.0.1 release are currently only available through simplified setup. |
| MicrosoftML | A collection of functions for incorporating machine learning into R code or script that executes on R Server and R Client. Available in R Server for Windows, R Client for Windows, and SQL Server 2016 R Services. |
| Remote execution via mrsdeploy package | Remote execution on a R Server 9.0.1 instance.|
| Web service deployment via mrsdeploy package | Publish, and subsequently manage, an R code block as a web service.|
| olapR package | Run MDX queries and connect directly to OLAP cubes on SQL Server 2016 Analysis Services from your R solution. Manually create and then paste in an MDX query, or use an R-style API to choose a cube, axes, and slicers. Available on R Server for Windows, SQL Server 2016 R Services, and an R Server (Standalone) installation through SQL Server. |
| RevoScaleR Package | Updated to include support for Spark 2.0. <br/><br/>`RxHiveData` generates a Hive Data Source object.<br/><br/>`RxParquetData` generates a Parquet Data Source object.<br/><br/>`rxSparkConnect` creates a persistent Spark compute context.<br/><br/>`rxSparkDisconnect` disconnects a Spark session and return to a local compute context. <br/><br/>`rxSparkListData` lists cached `RxParquetData` or `RxHiveData` data source objects.<br/><br/>`rxSparkRemoveData`removes cached `RxParquetData` or `RxHiveData` data source objects. Although RevoScaleR jobs only execute on Spark 2.0 if you have R Server 9.0.1 for Hadoop, you can create solutions containing Hive, Parquet, and Spark-related functions in R Client.|
| Operationalization features | Formerly known as DeployR, the operationalization feature is now fully integrated into R Server, with a new ASP .NET core bringing improved support from Microsoft. After installing R Server on select platforms, you'll have everything you need to enable operationalization and [configure](operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) R Server to host R analytics web services and remote R sessions. <br/><br/>In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization. The operationalization feature in Microsoft R Server provides the tools to deploy R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. R Server turns your R scripts into analytics web services, so R code can be easily executed by applications running on a secure server.<br/><br/>An operationalized R server offers the ability to host and bundle R analytics into web services with minimal code changes. R Server accepts interactive commands through [mrsdeploy functions](r-reference/mrsdeploy/mrsdeploy-package.md) for remote execution and web service deployment. Data scientists can use `mrsdeploy` functions  on the command line. Application developers can write code to instrument equivalent operations and integrate web services into their applications using [easy-to-consume Swagger-based APIs](operationalize/concept-api.md) in any programming language.<br/><br/>The operationalization feature can be configured [on a single machine](install/operationalize-r-server-one-box-config.md#onebox). It can also be [scaled](install/operationalize-r-server-enterprise-config.md) for business-critical applications with multiple web and compute nodes on clustered servers for load balancing. This gives you the ability to pipeline data streams that are subsequently transformed, analyzed, and visualized into an R analytics web service.<br/><br/>In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. Operationalization with R Server also supports production-grade workloads and seamless integration with popular [enterprise security solutions](operationalize/configure-start-for-administrators.md#security).<br/><br/>In the context of operationalization, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure. Feature support is limited to a subset of the [supported R Server platforms](install/r-server-install-supported-platforms.md) has the list.|

<a name="8vs9"></a>
The following blog post presents some of the main differences between Microsoft R Server 9.x configured to operationalize analytics and the add-on DeployR 8.0.5 which was available in R Server 8.0.5. Read about the [differences between DeployR and R Server 9.x Operationalization](https://blogs.msdn.microsoft.com/rserver/2017/05/11/1885/).

>[!Important]
>R Server configured to operationalize analytics is **not backwards compatible** with DeployR 8.x. There is no migration path as the APIs are completely new and the data stored in the database is structured differently. 

## R Server 8.0.5
  
Release announcement blog: https://blogs.technet.microsoft.com/machinelearning/2016/01/12/making-r-the-enterprise-standard-for-cross-platform-analytics-both-on-premises-and-in-the-cloud/

| Feature                | Description |
|------------------------|----------------------|
| R Server for Linux | Support for RedHat RHEL 7.x has been added. |nstallers are now composed of RPM packages, which can be installed via a top-level install script or as individual RPM packages. This can be convenient for Enterprise IT departments managing extensive deployments.|
| R Server for Hadoop | Installation on Hadoop clusters has been simplified to eliminate manual steps. <br/><br/>Support Hadoop on SUSE 11 and Hadoop distributions (Cloudera CDH 5.5-5.7, Hortonworks HDP 2.4, MapR 5.0-5.1)<br/><br/>Distributed compute context `RxSpark` is available, in which computations are parallelized and distributed across the nodes of a Hadoop cluster via Apache Spark. This provides up to a 7x performance boost compared to `RxHadoopMR`.<br/><br/>Hadoop diagnostic tool to collect status of MRS and dependencies on all nodes (available through a CSS Support request).<br/><br/> Hadoop user directories in HDFS and Linux are now created automatically as needed.<br/><br/>Hadoop administrator script to clean-up orphaned HDFS and Linux user directories. |
| R Server for Teradata | Option added to `rxPredict` to insert into an existing table.<br/><br/>Option  added for use of LDAP authentication with a TPT connection. |
| DeployR Enterprise | Improved Web security features for better protection against malicious attacks, improved installation security, and improved Security Policy Management.<br/><br/>Relies on an H2 database by default and allows you to easily use a SQL Server or PostgreSQL database instead to fit your production environment.<br/><br/> Simplified installer for a better customer experience.<br/><br/>XML format for data exchange is deprecated, and will be removed from future versions of DeployR.<br/><br/> The API has been updated. [See the change history.](deployr/deployr-api-reference.md#805)<br/><br/> This release is of DeployR Enterprise only. |

## R Server 8.0.3

R Server 8.0.3 is a Windows-only, SQL-Server-only release. It is installed using SQL Server 2016 setup. Version 8.0.3 was succeeded by version 9.0.1 in SQL Server. Features are cumulative so what shipped in 8.0.3 was available in 9.0.1.


## R Server 8.0.0

+ Revolution R Open is now Microsoft R Open, and Revolution R Enterprise is now generically known as Microsoft R Services, and specifically Microsoft R Server for Linux platforms.

+ Microsoft R Services installs on top of an enhanced version of R 3.2.2, Microsoft R Open for Revolution R Enterprise 8.0.0 (on Windows) or Microsoft R Open for Microsoft R Server (on Linux).

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

 [What is Machine Learning Server](what-is-machine-learning-server.md) 
 [What is R Server](what-is-microsoft-r-server.md) 