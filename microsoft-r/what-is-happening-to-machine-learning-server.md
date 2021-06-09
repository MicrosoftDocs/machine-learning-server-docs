---
title: "What's happening to Machine Learning Server?"
description: "An explanation of the support plan for Machine Learning Server."
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 06/08/2021
ms.topic: "overview"
ms.prod: "mlserver"

---

# What's happening to Machine Learning Server?

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

[Microsoft Machine Learning Server 9.4.7](what-is-machine-learning-server.md) is the last version and the **support will end on June 30, 2020**. Machine Learning Server is enterprise software for data science, providing R and Python interpreters, base distributions of R and Python, additional high-performance libraries from Microsoft, and an operationalization capability for advanced deployment scenarios.

This article explains options for replacing the functionality of Machine Learning Server and migration strategies for code and data implemented on Machine Learning Server.

The first decision point for these options is the locations of compute and data storage for analysis, either on-premises or in the cloud.

## On-premises deployment options

Machine Learning Server environments are available completely independent of connecting to the Internet or intranets. These are normally high-security environments where large-scale data analysis is critical, but the data is extremely sensitive.

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

### Azure Databricks

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

For more information, see [What is Azure Databricks?](/azure/databricks/scenarios/what-is-azure-databricks)

### Azure Synapse Analytics

Azure Synapse Analytics is an enterprise analytics service that accelerates time to insight across data warehouses and big data systems, using distributed processing and data constructs. Azure Synapse brings together SQL technologies used in enterprise data warehousing, Spark technologies used for big data, Pipelines for data integration and ETL/ELT, and deep integration with other Azure services such as Power BI, CosmosDB, and AzureML. 

Use Microsoft Azure Synapse as a replacement for Microsoft Machine Learning Server when you need to: 

- Leverage both serverless and dedicated resource models. For predictable performance and cost, create dedicated SQL pools to reserve processing power for data stored in SQL tables.  
- Process unplanned or “burst” workloads, access an always-available, serverless SQL endpoint. 
- Use built-in streaming capabilities to land data from cloud data sources into SQL tables. 
- Integrate AI with SQL by using machine learning models to score data using the T-SQL PREDICT function. 
- Leverage ML models with SparkML algorithms and AzureML integration for Apache Spark 2.4 supported for Linux Foundation Delta Lake. 
- Use a simplified resource model that frees you from having to worry about managing clusters. 
- Process data that requires fast Spark start-up and aggressive autoscaling. 
- Process data using.NET for Spark allowing you to reuse your C# expertise and existing .NET code within a Spark application. 
- Work with tables defined on files in the data lake are seamlessly consumed by either Spark or Hive. 
- Use SQL with Spark to directly explore and analyze Parquet, CSV, TSV, and JSON files stored in a data lake. 
- Enable fast, scalable data loading between SQL and Spark databases. 
- Ingest data from 90+ data sources. 
- Enable “Code-Free” ETL with Data flow activities. 
- Orchestrate notebooks, Spark jobs, stored procedures, SQL scripts, and more. 
- Monitor resources, usage, and users across SQL and Spark. 
- Use Role-based access control to simplify access to analytics resources. 
- Write SQL or Spark code and integrate with enterprise CI/CD processes. 

The architecture of Azure Synapse Analytics is as follows: 

![Azure Synapse Analytics architecture](media/what-is-happening-to-machine-learning-server/synapse-architecture.png) 

For more information, see [What is Azure Synapse Analytics?](/azure/synapse-analytics/overview-what-is)

### Azure Machine Learning

Azure Machine Learning is a cloud-based service that can be used for any kind of machine learning, from classical ML to deep learning, supervised, and unsupervised learning. Whether you prefer to write Python or R code with the SDK or work with no-code/low-code options in the studio, you can build, train, and track machine learning and deep-learning models in an Azure Machine Learning Workspace. With Azure Machine Learning you can start training on your local machine and then scale out to the cloud. The service also interoperates with popular deep learning and reinforcement open-source tools such as PyTorch, TensorFlow, scikit-learn, and Ray RLlib. 

Use Microsoft Azure Machine Learning as a replacement for Microsoft Machine Learning Server when you need: 

- A designer-based web environment for Machine Learning: drag-n-drop modules to build your experiments and then deploy pipelines in a low-code environment. 
- Jupyter notebooks: use our example notebooks or create your own notebooks to leverage our SDK for Python samples for your machine learning. 
- R scripts or notebooks in which you use the SDK for R to write your own code or use the R modules in the designer. 
- The “Many Models Solution Accelerator” which builds on Azure Machine Learning and enables you to train, operate, and manage hundreds or even thousands of machine learning models. 
- Machine learning extensions for Visual Studio Code (preview) provides you with a full-featured development environment for building and managing your machine learning projects. 
- A Machine learning Command-Line Interface (CLI), Azure Machine Learning includes an Azure CLI extension that provides commands for managing with Azure Machine Learning resources from the command line. 
- Integration with open-source frameworks such as PyTorch, TensorFlow, and scikit-learn and many more for training, deploying, and managing the end-to-end machine learning process. 
- Reinforcement learning with Ray RLlib 
- MLflow to track metrics and deploy models or Kubeflow to build end-to-end workflow pipelines. 

The architecture of a Azure Machine Learning deployment is as follows: 

![Azure Machine Learning architecture](media/what-is-happening-to-machine-learning-server/azureml-architecture.svg) 

For more information, see [What is Azure Machine Learning?](/azure/machine-learning/overview-what-is-azure-ml)

### Azure Cosmos DB

Azure Cosmos DB is a fully managed NoSQL database for modern app development. Single-digit millisecond response times, and automatic and instant scalability, guarantee speed at any scale. Business continuity is assured with SLA-backed availability and enterprise-grade security. App development is faster and more productive thanks to turnkey multi region data distribution anywhere in the world, open-source APIs and SDKs for popular languages.  

As a fully managed service, Azure Cosmos DB handles database administration with automatic management, updates and patching. It also handles capacity management with serverless and automatic scaling options that respond to application needs to match capacity with demand.  

Use Microsoft Azure CosmosDB as a replacement for Microsoft Machine Learning Server when you need to: 

- Create web, mobile, gaming, and IoT applications that need to handle massive amounts of data, reads, and writes at a global scale with near-real response times for a variety of data types, leveraging guaranteed high availability, high throughput, low latency, and tunable consistency. 
- Deeply integrate with key Azure services used in modern (cloud-native) app development including Azure Functions, IoT Hub, AKS (Azure Kubernetes Service), App Service, and more. 
- Choose from multiple database APIs including the native Core (SQL) API, API for MongoDB, Cassandra API, Gremlin API, and Table API. 
- Build apps on Core (SQL) API using the languages of your choice with SDKs for .NET, Java, Node.js and Python, or your choice of drivers for any of the other database APIs. 
- Run no-ETL analytics over the near-real time operational data stored in Azure Cosmos DB with Azure Synapse Analytics. 
- Enable change feed to track and manage changes to database containers and create triggered events with Azure Functions. 
- Leverage a schema-less service which automatically indexes all your data, regardless of the data model, to deliver faster queries. 
- Access a comprehensive suite of SLAs including industry-leading availability worldwide. 
- Easily distribute data to any Azure region with automatic data replication with zero downtime with multi-region writes or RPO 0 when using Strong consistency. 
- Implement enterprise-grade encryption-at-rest with self-managed keys. 
- Use Microsoft Azure role-based access control keeps to keep data safe  with fine-tuned control. 
- Implement a fully-managed database service with no touch maintenance, patching, and updates. 
- Leverage a serverless model for workloads and an automatic and responsive service to manage traffic bursts on demand. 
- Leverage auto-scale systems and provisioned throughput automatically and instantly scale capacity for unpredictable workloads, while maintaining SLAs. 

The architecture of a Microsoft Azure CosmosDB deployment is as follows: 

![Azure Cosmos DB and Azure Synapse Analytics architecture](media/what-is-happening-to-machine-learning-server/synapse-analytics-cosmos-db-architecture.png) 


For more information on using Notebooks with Python or C# in CosmosDB Deployments, see [Built-in Jupyter Notebooks support in Azure Cosmos DB](/azure/cosmos-db/cosmosdb-jupyter-notebooks).

For more information, see the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction).

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
