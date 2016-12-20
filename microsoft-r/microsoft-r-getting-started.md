---

# required metadata
title: "Introduction to Microsoft R Server and R Client"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/20/2016"
ms.topic: "get-started-article"
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
ms.technology:
  - r-client
  - r-server
ms.custom: ""

---

# Microsoft R products and features

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and graphics. As an open source technology, R is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Although R is widely used, deploying R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or requirements for scale and support in production environments arise.

Microsoft R fills the functional void in enterprise deployments through a collection of servers and tools that extend the capabilities of R. The Microsoft R product family builds on top of open source R, providing both commercial and free offerings in the form of Microsoft R Server, Microsoft R Client, and Microsoft R Open. In addition to the over 8000 standard R packages available to all R users, Microsoft R Server and R Client provide additional R packages and connectivity tools to enable remote compute context, machine learning integration, and scalable solutions through clusters or parallelized workloads on platforms that support it.

<br>
<a name="compare-prods"></a>
### Microsoft R Product Comparison

The following table  offers a broad comparison of each member of the Microsoft R product family.

|Component  |Key characteristics|
|-----------|----------------|
|[Microsoft R Open (MRO)](r-open.md) | Microsoft's distribution of open-source R. <br />Free of charge and community supported in forums (you cannot contact Microsoft customer support for MRO issues). <br />You can use it as a standalone component the same way you would use any other distribution of R. Both R Client and R Server automatically include the MRO package for its delivery of the open source R language. Script or code blocks you write against MRO consists of base functions.|
|[Microsoft R Client (MRC)](r-client.md) | Adds proprietary packages from Microsoft (RevoScaleR and MicrosoftML) that are resource-bound versions of the higher-end versions in R Server. The purpose of this component is to provide the benefits of statistical, visual, and analytical functions, but at lower scale intended for development and local execution. <br />Free of charge and community supported in forums (no Microsoft support). <br />R Client can be used as a standalone component, but also overlaps with R Server in the form of common pacakges so that if and when you need the extra capability of R Server, the transition is easy and your code runs intact on R Server with minimal modifications.|
|[Microsoft R Server (MRS)](rserver.md) | Enterprise class server software, fully supported by Microsoft, scalable for big data scenarios. <br />Supports parallel and distributed workloads on standalone servers, clustered servers, database platforms like SQL Server and Teradata, and on distributed file systems like Hadoop. <br />Only R Server includes the operationalization features that let you run solutions and scripts on coordinated web and compute node configurations.|

Features provided by Microsoft R Server, Microsoft R Client, and Microsoft R Open can be categorized as shown in this table. This table slices the main features by components.

|Features   |Microsoft R Open|Microsoft R Client|Microsoft R Server|
|-----------|----------------|------------------|-----------|
|Big Data   |In-memory bound<br>Can only process datasets that fit into the available memory|In-memory bound<br>Can process datasets that fit into the available memory<br>Operates on large volumes when connected to R Server|Disk scalability<br>Operates on bigger volumes & factors|  
|Speed of<br>Analysis    |Multi-threaded when MKL is installed for non-ScaleR functions|Multi-threaded with MKL for non-ScaleR functions<br>Up to 2 threads for ScaleR functions with a local compute context|Full parallel threading & processing|
|Enterprise<br>Readiness   |Community support|Community support|Commercial support|
|Analytic<br>Breadth <br>& Depth     |8000+ open source packages|Leverage & optimize open source R packages plus 'Big Data'-ready ScaleR packages|Leverage & optimize open source R packages plus 'Big Data'-ready + Multithreaded ready ScaleR packages|
|Commercial<br>Viability   |Risk of deployment to open source|Free for everyone|Commercial licenses|
|[Operationalization](operationalize/about.md)  |Not available|Not available|Included|

<br>


### Microsoft R Server


[!include[Microsoft R Server](./includes/r-server/intro.md)]

<br>

<a name="mrc"></a>
### Microsoft R Client

[!include[Microsoft R Client](./includes/r-client/r-client-intro.md)]

Learn how to [install and get started with Microsoft R Client](r-client-get-started.md).

<br>

### Microsoft R Open

[!include[Microsoft R Open](./includes/r-open/mro-intro.md)]

<br>

## Why choose R Server over R Client

For a high-level, side-by-side comparison of R features in Microsoft R Server, R Client, and R Open, [see here](#compare-prods). For an explanation of benefts exclusive to R Server, read on.

### ScaleR

ScaleR technology delivered through the RevoScaleR package offers almost unbounded scale in running R workloads in parallel and distributed configurations. Although you can call ScaleR functions using R Client, only R Server provides support for data chunking and parallelization. 

Given a platform that supports it, functions in ScaleR provide high performance, parallelized, and distributable analytics functions that scale from small data sets in memory to huge data sets stored on disk on a cluster of computers. The analytics functions provided include summary statistics, cubes and crosstabs, linear models, logistic regression, generalized linear models, kmeans clustering, decision trees, and decision forests. These algorithms are parallelized and distributed automatically, and process data in chunks so that all of your data does not need to be in memory at one time; you can use the same analysis code for your giant data set as you do for a small data set in memory.

RevoScaleR also provides traditional high performance computing (tools if you prefer to construct your own distributed computations. In addition, in many environments, there are full-featured tools for data cleaning and manipulation.

Functions in RevoScaleR are prefixed with ‘rx’ for analysis and data manipulation. Additionally, use ‘rxExec’ for high performance computing. If you are computing decision trees, also check out the included RevoTreeView package that allows you to interactively visualize your decision trees.


### Operationalization

Operationalization is central to R Server capability, and one of the key reasons you would choose R Server over R Client. Formerly known as DeployR, the operationalization feature is now fully integrated into R Server. After installing R Server on select platforms (availability on all platforms is still pending), you'll have everything you need to enable operationalization and [configure](operationalize/configuration-initial.md) R Server to host R analytics web services and remote R sessions.  For details on which platforms, see [Supported platforms](rserver-install-supported-platforms.md).

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization. The operationalization feature in Microsoft R Server provides the tools to deploy R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. R Server turns your R scripts into analytics web services, so R code can be easily executed by applications running on a secure server.

An operationalized R server offers the ability to host and bundle R analytics into web services with minimal code changes. R Server accepts interactive commands through [mrsdeploy functions](mrsdeploy/mrsdeploy.md) for remote execution and web service deployment. Data scientists can use `mrsdeploy` functions  on the command line. Application developers can write code to instrument equivalent operations and integrate web services into their applications using [easy-to-consume Swagger-based APIs](operationalize/api.md) in any programming language.

The operationalization feature can be configured [on a single machine](operationalize/configuration-initial.md#onebox). It can also be [scaled](operationalize/configuration-initial.md#enterprise) for business-critical applications with multiple web and compute nodes on clustered servers for load balancing. This gives you the ability to pipeline data streams that are subsequently transformed, analyzed, and visualized into an R analytics web service.

In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. Operationalization with R Server also supports production-grade workloads and seamless integration with popular [enterprise security solutions](operationalize/security.md).

For feature information and next steps, see [Operationalization with R Server](operationalize/about.md) and [Configure operationalization on R Server](operationalize/configuration-initial.md).

> [!NOTE]
> In the context of operationalization, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure. Operationalization is available in many, but not all, of the supported platforms. For the most up-to-date list, see [supported R Server platforms](rserver-install-supported-platforms.md).

## Next Steps

If you are new to Microsoft R, we recommend that you start with R Client and an integrated development environment like R Tools for Visual Studio (RTVS). This configuration is free of charge. It gets you MRO with full support of all base R functions so that you can write R-only solutions, yet also provides several Microsoft R proprietary packages that run locally on your development computer.

Using just MRO and RTVS, you can use the R Core Team manuals that are part of every R distribution to learn how to code in R.

Manuals include *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and more. Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

Because you have R Client, your script can also include functions from MicrosoftML, olapR, and RevoScaleR. All of these packages are available in both R Client and R Server, but at different levels of capacity.

Tutorials in Microsoft R product documentation will help you learn how to use the functions in these packages:  

+ [Create your first R solution with Microsoft R](microsoft-r-getting-started-tutorial.md)

+ [Get started with ScaleR](scaler-getting-started.md)

+ [Introduction to MicrosoftML](microsoftml-introduction.md)

## See also

[What's new in R Server](rserver-whats-new.md)
