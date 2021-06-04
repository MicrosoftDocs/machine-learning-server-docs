---
title: "What's happening to Machine Learning Server?"
description: "An explanation of the support plan for Machine Learning Server."
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 06/01/2021
ms.topic: "overview"
ms.prod: "mlserver"

---

# What's happening to Machine Learning Server?

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

Microsoft Machine Learning Server 9.4.7 is enterprise software for data science, providing R and Python interpreters, base distributions of R and Python, additional high-performance libraries from Microsoft, and an operationalization capability for advanced deployment scenarios. Machine Learning Server runs on-premises and in the cloud, on a variety of operating systems, and can run in a distributed mode. Web services are hosted on a server grid on-premises or in the cloud and can be integrated with line-of-business applications. Additionally, Machine Learning Server integrates with Active Directory and Azure Active Directory and includes role-based access control to satisfy security and compliance needs of your enterprise. The ability to deploy to an elastic grid lets you scale seamlessly with the needs of your business, both for batch and real-time scoring. 

The support for Machine Learning Server ends on June 30, 2022. This article explains options for replacing the functionality of Microsoft Machine Learning Server and migration strategies for code and data implemented on Microsoft Machine Learning Server. 

The first decision point for these options is the locations of compute and data storage for analysis.  

## On-premises deployment options

Microsoft Machine Learning Server environments are available completely independent of connecting to the Internet or Intranets. These are normally high-security environments where large-scale data analysis is critical, but the data is extremely sensitive. 

### SQL Server Big Data Clusters

Microsoft SQL Server Big Data Clusters allow you to deploy scalable clusters of SQL Server, Spark, and HDFS containers running on Kubernetes. These components are running side by side to enable you to read, write, and process big data from Transact-SQL or Spark, allowing you to easily combine and analyze your high-value relational data with high-volume big data.  

Microsoft SQL Server Big Data Clusters includes SQL Server Machine Learning Services, which runs Python, R and Java languages from inside SQL Server Stored Procedures. Other languages such as Go are also possible via an extensible framework. The Spark subsystem within the cluster is a standard Apache Spark distribution, and can run Scala, R, Python and other supported languages and constructs.  

SQL Server Big Data Clusters has Relational data stores (SQL Server Databases), HDFS data stores, and a Relational large-scale store within the architecture.  

Use SQL Server Big Data Clusters as a replacement for Microsoft Machine Learning Server when you need to: 

- Deploy scalable clusters of SQL Server, Spark, and HDFS containers running on Kubernetes, on-premises or in the cloud. 
- Read, write, and process big data from Transact-SQL or Spark. 
- Easily combine and analyze high-value relational data with high-volume big data, storing data in HDFS that is also accessible by SQL Server. 
- Query virtualized data from external SQL Server, Oracle, Teradata, MongoDB, and ODBC data sources with external tables using PolyBase. 
- Use data for AI, machine learning, and other analysis tasks. 
- Deploy and run applications in the cluster, accessible from anywhere you determine. 
- Provide high availability for the SQL Server master instance and all databases by using Always On availability group technology.  

The architecture of a SQL Server Big Data Cluster is as follows:

![SQL Server Big Data Clusters architecture](media/what-is-happening-to-machine-learning-server/sql-bdc-architecture-diagram-overview.png) 

For more information on SQL Server Big Data Clusters, see [What are SQL Server Big Data Clusters?](/sql/big-data-cluster/big-data-cluster-overview).

## In-cloud deployment options

Many workloads from Microsoft Machine Learning Server can also be uploaded to the Microsoft Azure platform. Data “born in the cloud” (originated in Cloud-based applications) are prime candidates for these technologies, and data movement services can migrate large-scale data securely and quickly. For more on data movement options, see [Choose an Azure solution for data transfer](/azure/storage/common/storage-choose-data-transfer-solution).

Microsoft Azure has systems and certifications allowing secure data and data processing in a variety of tools. More information on these certifications is located at the [Microsoft Trust Center resource](https://www.microsoft.com/en-us/trust-center).

### Azure HDInsight

Azure HDInsight is a cloud distribution of Hadoop components. Azure HDInsight is a managed, full-spectrum, open-source analytics service in the cloud for enterprises. HDInsight allows you to use open-source frameworks such as Hadoop, Apache Spark, Apache Hive, LLAP, Apache Kafka, Apache Storm, R, and more. 

HDInsight includes specific cluster types and cluster customization capabilities, such as the capability to add components, utilities, and languages. HDInsight offers the following cluster types:

| Cluster Type | Description |
|-|-|
| Apache Hadoop | A framework that uses HDFS, YARN resource management, and a simple MapReduce programming model to process and analyze batch data in parallel. |
| Apache Spark | An open-source, parallel-processing framework that supports in-memory processing to boost the performance of big-data analysis applications. |
| Apache HBase | A NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semi-structured data--potentially billions of rows times millions of columns. |
| ML Services | A server for hosting and managing parallel, distributed R processes. It provides data scientists, statisticians, and R programmers with on-demand access to scalable, distributed methods of analytics on HDInsight. |
| Apache Storm | A distributed, real-time computation system for processing large streams of data fast. Storm is offered as a managed cluster in HDInsight.  |
| Apache Interactive Query | In-memory caching for interactive and faster Hive queries. |
| Apache Kafka | An open-source platform that's used for building streaming data pipelines and applications. Kafka also provides message-queue functionality that allows you to publish and subscribe to data streams. |

Use Azure HDInsight as a replacement for Machine Learning Server when you need to: 

- Create optimized clusters for Hadoop, Spark, Interactive query (LLAP), Kafka, Storm, HBase, and ML Services on Azure.  
- Use an end-to-end SLA on all your production workloads. 
- Scale workloads up or down.  
- Build data pipelines to operationalize your jobs.  
- Decouple compute and storage for better performance and flexibility. 
- Protect your enterprise data assets with Azure Virtual Network, encryption, and integration with Azure Active Directory. HDInsight also meets the most popular industry and government compliance standards. 
- Integrate with Azure Monitor logs to provide a single interface.  
- Leverage more regions than any other big data analytics offering. Azure HDInsight is also available in Azure Government, China, and Germany, which allows you to meet your enterprise needs in key sovereign areas. 
- Use rich productive tools for Hadoop and Spark with your preferred development environments. These development environments include Visual Studio, VSCode, Eclipse, and IntelliJ for Scala, Python, R, Java, and .NET support. Data scientists can also collaborate using popular notebooks such as Jupyter and Zeppelin. 
- Extend clusters with installed components (Hue, Presto, and so on) by using script actions, by adding edge nodes, or by integrating with other big data certified applications.  
- Create clusters with open-source frameworks such as Hadoop, Spark, Hive, LLAP, Kafka, Storm, HBase, and R. These clusters, by default, come with other open-source components that are included on the cluster such as Apache Ambari5, Avro5, Apache Hive3, HCatalog2, Apache Mahout2, Apache Hadoop MapReduce3, Apache Hadoop YARN2, Apache Phoenix3, Apache Pig3, Apache Sqoop3, Apache Tez3, Apache Oozie2, and Apache ZooKeeper5. 
- Use Default programming language support, such as Java, Python, .NET, Go, Java virtual machine (JVM) languages (The following JVM-based languages are supported on HDInsight clusters: Clojure, Jython, and Scala) 
- Use Hadoop-specific languages such as Pig Latin for Pig jobs, HiveQL for Hive jobs and SparkSQL) 

The architecture of a Azure HDInsight deployment is as follows:

![HDInsight architecture](media/what-is-happening-to-machine-learning-server/hdinsight-architecture-data-warehouse.png) 

For more information, see [What is Azure HDInsight?](/azure/hdinsight/hdinsight-overview).

## Azure Databricks

Azure Databricks is a data analytics platform optimized for the Microsoft Azure cloud services platform. Azure Databricks offers two environments for developing data intensive applications: Azure Databricks SQL Analytics and Azure Databricks Workspace. 

Azure Databricks SQL Analytics provides an easy-to-use platform for analysts who want to run SQL queries on their data lake, create multiple visualization types to explore query results from different perspectives, and build and share dashboards. 

Azure Databricks Workspace provides an interactive workspace that enables collaboration between data engineers, data scientists, and machine learning engineers. For a big data pipeline, the data (raw or structured) is ingested into Azure through Azure Data Factory in batches, or streamed near real-time using Apache Kafka, Event Hub, or IoT Hub. This data lands in a data lake for long term persisted storage, in Azure Blob Storage or Azure Data Lake Storage. As part of your analytics workflow, use Azure Databricks to read data from multiple data sources and turn it into breakthrough insights using Spark.  

Use Microsoft Azure Databricks as a replacement for Microsoft Machine Learning Server when you need: 

- Fully managed Spark clusters with Spark SQL and DataFrames. 
- Streaming for real-time data processing and analysis for analytical and interactive applications, Integrating with HDFS, Flume, and Kafka. 
- Access to the MLlib library consisting of common learning algorithms and utilities, including classification, regression, clustering, collaborative filtering, dimensionality reduction, as well as underlying optimization primitives. 
- Documentation of your progress in notebooks in R, Python, Scala, or SQL. 
- Visualization of data in a few clicks, and use familiar tools like Matplotlib, ggplot, or d3. 
- Interactive dashboards to create dynamic reports. 
- GraphX, for Graphs and graph computation for a broad scope of use cases from cognitive analytics to data exploration. 
- Cluster creation in seconds, with dynamic autoscaling clusters, sharing them across teams. 
- Programmatic cluster access using REST APIs. 
- Instant access to the latest Apache Spark features with each release. 
- A Spark Core API: Includes support for R, SQL, Python, Scala, and Java. 
- An interactive workspace for exploration and visualization 
- Fully managed SQL endpoints in the cloud 
- SQL queries that run on fully managed SQL endpoints sized according to query latency and number of concurrent users. 
- Integration with Azure Active Directory. 
- Role-based access for  fine-grained user permissions for notebooks, clusters, jobs, and data. 
- Enterprise-grade SLAs. 
- Dashboards for sharing insights, combining visualizations and text to share insights drawn from your queries. 
- Alerts help you monitor and integrate, and notification when a field returned by a query meets a threshold. Use alerts to monitor your business or integrate them with tools to start workflows such as user onboarding or support tickets. 
- Enterprise security, including Azure Active Directory integration, role-based controls, and SLAs that protect your data and your business. 
- Integration with Azure services and Azure databases and stores including Synapse Analytics, Cosmos DB, Data Lake Store, and Blob storage. 
- Integration with Power BI and other BI tools, such as Tableau Software. 

The architecture of a Microsoft Azure Databricks deployment is as follows:

![Azure Databricks architecture](media/what-is-happening-to-machine-learning-server/azure-databricks-overview.png) 













## Example retirement banners from other products

The following are sample banners currently being used in other products. All of these are for Azure services.

### Azure Data Lake Storage Gen1

> [!NOTE]
> On **Feb 29, 2024** Azure Data Lake Storage Gen1 will be retired. For more information, see the [official announcement](https://azure.microsoft.com/updates/action-required-switch-to-azure-data-lake-storage-gen2-by-29-february-2024/). If you use Azure Data Lake Storage Gen1, make sure to migrate to Azure Data Lake Storage Gen2 prior to that date. To learn how, see [Migrate Azure Data Lake Storage from Gen1 to Gen2](#) 

### Azure Dev Spaces

> [!IMPORTANT]
> Azure Dev Spaces is being retired and will stop working on October 31, 2023. Consider migrating to [Bridge to Kubernetes](#).

### Azure Notebooks public preview site

> [!IMPORTANT]
> On January 15, 2021 the Azure Notebooks public preview site retired and has been replaced with integrated services from Visual Studio, Azure, and GitHub. To create new or execute notebook content, [learn more](https://aka.ms/aznb-notebooks-at-msft/) about our other notebooks experiences from Microsoft.

### Classic VMs

> [!IMPORTANT]
> Classic VMs will be retired on March 1, 2023.
>
> If you use IaaS resources from ASM, please complete your migration by March 1, 2023. We encourage you to make the switch sooner to take advantage of the many feature enhancements in Azure Resource Manager.
>
> For more information, see [Migrate your IaaS resources to Azure Resource Manager by March 1, 2023](#).

### Azure Container Service

> [!WARNING]
> **The Azure Container Service (ACS) is being deprecated. No new features or functionality are being added to ACS. All of the APIs, portal experience, CLI commands and documentation are marked as deprecated.**
>
> For more information, see the [Azure Container Service deprecation announcement on Azure.com](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/).
>
> We recommend that you deploy one of the following Azure Marketplace solutions:
>
> * Mesosphere DC/OS
>   * [Enterprise](https://azuremarketplace.microsoft.com/marketplace/apps/mesosphere.enterprise-dcos?tab=Overview)
>   * [Open Source edition](https://azuremarketplace.microsoft.com/marketplace/apps/mesosphere.dcos?tab=overview)
>
> If you want to use Kubernetes, see [Azure Kubernetes Service](#).

### Developer portal

> [!NOTE]
> This documentation content is about the deprecated developer portal. You can continue to use it, as per usual, until its retirement in October 2023, when it will be removed from all API Management services. The deprecated portal will only receive critical security updates. Refer to the following articles for more details:
> 
> * [Learn how to migrate to the new developer portal](#)
> * [Azure API Management new developer portal overview](#)
> * [Access and customize the new developer portal](#)

## See Also

[Support Timeline for Microsoft R Server & Machine Learning Server](resources-servicing-support.md)
