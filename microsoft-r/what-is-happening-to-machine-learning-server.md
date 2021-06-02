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

### Microsoft SQL Server Big Data Clusters 

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

![SQL Server Big Data Clusters archivtecture](media/what-is-happening-to-machine-learning-server/sql-bdc-architecture-diagram-overview.png) 

For more information on SQL Server Big Data Clusters, see [What are SQL Server Big Data Clusters?](/sql/big-data-cluster/big-data-cluster-overview).






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
