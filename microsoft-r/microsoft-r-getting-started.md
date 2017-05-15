<meta http-equiv="refresh" content="0; url=https://msdn.microsoft.com/en-us/microsoft-r/index" />

# required metadata
title: "Introduction to Microsoft R Server and R Client"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "01/05/2017"
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

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and graphics. As an open source technology, R is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Although R is widely used, deploying R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or requirements for scale and support in production environments emerge.

Microsoft R fills the functional void in enterprise deployments by providing a collection of servers and tools that extend the capabilities of R. The Microsoft R product family builds on top of open source R, offering free and commercial products in the form of Microsoft R Server, Microsoft R Client, and Microsoft R Open. In addition to the over 8000 standard R packages available to all R users, Microsoft R Server and R Client include additional R packages and connectivity tools that enable remote compute context and remote execution, web service deployment, machine learning integration, and scalable solutions through clusters or parallelized workloads on platforms that support it.

<a name="compare-prods"></a>
## Compare products

The following table broadly compares members of the Microsoft R product family. All Microsoft R products are built on Microsoft R Open (MRO) and install the package automatically.

|Component  |Role |Price | Support | Intended use |
|-----------|-----|------|---------|--------------|
|[Microsoft R Open (MRO)](r-open.md) | Microsoft's distribution of open source R | Free | Community forums <sup>1</sup>| Use MRO as you would any other distribution of R. Script written against MRO is straight R, composed of basic functions provided in publically available R packages.|
|[Microsoft R Client (MRC)](r-client.md) | Workstation version of Microsoft R | Free | Community forums <sup>1</sup>| Adds custom functionality provided in proprietary Microsoft R packages, intended for development and local execution of in-memory datasets.|
|[Microsoft R Server (MRS)](rserver.md) | Enterprise class server software | Commercial software | [Fully supported by Microsoft](rserver-servicing-support.md) | Adds custom functionality provided in proprietary Microsoft R packages, intended for local, remote, or distributed execution of larger datasets at scale.|

<sup>1</sup> Microsoft does not offer technical support for issues encountered in either MRO or MRC, but you can get peer support in [MSDN forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ropen) and [StackOverflow](https://stackoverflow.com/questions/tagged/microsoft-r), to name a few.

## Compare features by product

Features provided by Microsoft R Server, Microsoft R Client, and Microsoft R Open can be categorized as shown in this table. This table slices key features by components. Additional capability provided in R Client and R Server is delivered via propertietary packages in Microsoft R.


<table>
    <thead>
        <tr>
            <th width=10%></th>
            <th width=23%>Microsoft R Open</th>
            <th width=32%>Microsoft R Client</th>
            <th width=35%>Microsoft R Server</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Storage</b></td>
            <td>Memory bound<sup>1</sup></td>
            <td>Memory bound<sup>1</sup> &amp; operates on large volumes when connected to R Server.</td>
            <td>Data chunking across multiple disks.<br />Operates on bigger volumes &amp; factors.</td>
        </tr>
        <tr>
            <td><b>Speed of Analysis</b></td>
            <td>Multithreaded via MKL<sup>2</sup> for non-RevoScaleR functions.</td>
            <td>Multithreaded via MKL<sup>2</sup> for non-RevoScaleR functions, but only up to 2 threads for ScaleR functions with a local compute context.</td>
            <td>Full parallel threading &amp; processing for RevoScaleR functions as well as for non-RevoScaleR functions (via MKL<sup>2</sup>) in both local and remote compute contexts.</td>
        </tr>
        <tr>
            <td><b>Analytic Breadth &amp; Depth</b></td>
            <td>Open source packages only.</td>
            <td>Open source R packages plus proprietary packages.</td>
            <td>Open source R packages plus proprietary packages with support for parallelization and distributed workloads.</td>
        </tr>
        <tr>
            <td><b><a href="operationalize/about.md">Operationalizing</a> R Analytics</b></td>
            <td>Not available</td>
            <td>Not available</td>
            <td>Includes the instant deployment and easy consumption of R analytics, interactive remote code execution, speedy realtime scoring, scalability, and enterprise-grade security.</td>
        </tr>
    </tbody>
</table>
<sup>1</sup> Memory bound because product can only process datasets that fit into the available memory.

<sup>2</sup> Because the Intel Math Kernel Library (MKL) is included in MRO, the performance of a generic R solution is generally better. MKL replaces the standard R implementations of Basic Linear Algebra Subroutines (BLAS) and the LAPACK library with multithreaded versions. As a result, calls to those low-level routines tend to execute faster on Microsoft R than on a conventional installation of R.

## Microsoft R Open

[!include[Microsoft R Open](./includes/r-open/mro-intro.md)]

<a name="mrc"></a>
## Microsoft R Client

[!include[Microsoft R Client](./includes/r-client/r-client-intro.md)]

Learn how to [install and get started with Microsoft R Client](r-client-get-started.md).

## Microsoft R Server

Microsoft R Server is "R for the Enterprise" and solves the problem of deploying and consuming of R code. It offers big-data capable R distributions for servers, Hadoop clusters, and data warehouses. It supports a variety of big data statistics, predictive modeling and machine learning capabilities, and provides you with analytics that are fully compatible with the R language, the de facto standard for modern analytics users.

Although generic R scripts tend to run faster on MRO via Intel MKL, the major benefit in terms of scale and performance comes from using ScaleR functions. ScaleR is available in both R Client and R Server, but only R Server adds support for remote execution, remote compute contexts, data chunking, additional threads for multithreaded processing, parallel processing, and streaming.

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools for doing just that; it is a full-featured web services software development kit for R that allows programmers to use in any language to integrate the R analysis output with a third party package. [Learn more about operationalizing](operationalize/about.md)

<a href="https://www.microsoft.com/en-us/cloud-platform/r-server" target="_blank"><b>Watch this 2-minute video introduction to Microsoft R Server.</b></a> 

> [!Note]
> Performance can be affected by external factors outside the R code, including competing demands on server resources, the type of query plan that is created, schema changes, the need to update statistics or create a new query plan, fragmentation and so on. It is possible that a stored procedure containing R code might run in seconds under one workload, but take minutes when there are other services running. We recommend that you monitor multiple aspects of server performance, including networking for remote compute contexts, when quantifying R job performance.

## Why choose R Server over R Client

R Server and R Client offer virtually identical packages, but each one targets different scenarios. R Client is intended for data scientists who create solutions that run locally. R Server is commercial software that runs on a range of platforms, at much greater scale, with infrastructure for handling major workloads, on client-server topologies that support remote access over authenticated connections. You can learn and develop on R Client, and then migrate your work to R Server when you need the scale, support, and infrastructure of an operationalized server.

### Scale

On R Server, the ScaleR technology in the RevoScaleR package offers almost unbounded scale in running R workloads in parallel and distributed configurations. Although you can call ScaleR functions on a system having just R Client, ScaleR is throttled on R Client: datasets must fit in memory, and processing is capped at a maximum of two processors on the local system. Only R Server gives you ScaleR at full capacity, with support for remote compute context, data chunking, parallelization, and distributed workloads.

Given a platform that supports it, functions in ScaleR provide high performance, parallelized, and distributable analytics functions that scale from small data sets in memory to huge data sets stored on disk on a cluster of computers. The analytics functions provided include summary statistics, cubes and crosstabs, linear models, logistic regression, generalized linear models, kmeans clustering, decision trees, and decision forests. These algorithms are parallelized and distributed automatically, and process data in chunks so that all of your data does not need to be in memory at one time; you can use the same analysis code for your giant data set as you do for a small data set in memory.

ScaleR also provides traditional high performance computing (tools if you prefer to construct your own distributed computations. In addition, in many environments, there are full-featured tools for data cleaning and manipulation.

Functions in ScaleR are prefixed with ‘rx’ for analysis and data manipulation. Additionally, use ‘rxExec’ for high performance computing. If you are computing decision trees, also check out the included RevoTreeView package that allows you to interactively visualize your decision trees.


### Operationalizing Analytics

Being able to operationalize your analytics is a central capability in R Server, and one of the key reasons you would choose R Server over R Client. Formerly known as DeployR, this capability for operationalizing your code is now fully integrated into R Server. After installing R Server on select platforms (availability on all platforms is still pending), you'll have everything you need to [configure R Server to host R analytics web services and remote R sessions](operationalize/configuration-initial.md).  For details on which platforms, see [Supported platforms](rserver-install-supported-platforms.md).

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools to deploy R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. R Server turns your R scripts into analytics web services, so R code can be easily executed by applications running on a secure server.

An operationalized R Server offers the ability to host and bundle R analytics into web services with minimal code changes. R Server accepts interactive commands through [mrsdeploy functions](mrsdeploy/mrsdeploy.md) for remote execution and web service deployment. Data scientists can use `mrsdeploy` functions  on the command line. Application developers can write code to instrument equivalent operations and integrate web services into their applications using [easy-to-consume Swagger-based APIs](operationalize/api.md) in any programming language.

You can configure R Server to operationalize analytics [on a single machine](operationalize/configuration-initial.md#onebox). It can also be [scaled](operationalize/configure-enterprise.md) for business-critical applications with multiple web and compute nodes on clustered servers for load balancing. This gives you the ability to pipeline data streams that are subsequently transformed, analyzed, and visualized into an R analytics web service.

In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. R Server also supports production-grade workloads and seamless integration with popular [enterprise security solutions](operationalize/security.md).

For more information and next steps, see the articles on [the introduction to operationalizing analytics](operationalize/about.md) and [configuring R Server to operationalize](operationalize/configuration-initial.md).

> [!NOTE]
> In this context, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure. The ability to use R Server to operationalize analytics is available in many, but not all, of the supported platforms. For the most up-to-date list, see [supported R Server platforms](rserver-install-supported-platforms.md).

### R Server Platforms

|Microsoft R Server platforms|Description|Install|Get Started|
|----------------------------|-----------|:-----:|:----------------:|
|R Server for Hadoop        |Scale your analysis transparently by distributing work across nodes without complex programming|[Doc](rserver-install-hadoop.md)|[Doc](scaler-hadoop-getting-started.md)|
|R Server for Teradata DB   |Run advanced analytics in-database for seamless data analysis on Teradata|[Doc](rserver-install-teradata-server.md)|[Doc](/scaler-teradata-getting-started.md)|
|R Server for Linux         |Bring predictive and prescriptive analytics power to your Linux environments|[Doc](rserver-install-linux-server.md)|[Doc](scaler-getting-started.md)|
|R Server for Windows|Bring predictive and prescriptive analytics power to your Windows environments|[Doc](rserver-install-windows.md)|[Doc](scaler-getting-started.md)|
|SQL Server Machine Learning Services  |Run advanced analytics in-database for seamless data analysis on SQL Server|[Doc](https://msdn.microsoft.com/library/mt696069.aspx)|[Doc](https://msdn.microsoft.com/library/mt604885.aspx)|

<br />
For a list of supported operating systems, see [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md).


## Next Steps

If you are new to Microsoft R, we recommend starting with R Client and an integrated development environment like **R Tools for Visual Studio (RTVS)**. This configuration is free of charge. It gives you Microsoft R Open with full support of all base R functions so that you can write R-only solutions, but also includes the Microsoft R proprietary packages that run locally on your development computer.

Using just MRO and RTVS, you can use the R Core Team manuals that are part of every R distribution to learn how to code in R. Built-in manuals include *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and more. Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

However, because you have R Client, your script can also include functions from Microsoft R packages, including MicrosoftML, olapR, and RevoScaleR. All of these packages are available in both R Client and R Server, but at different levels of capacity.

Tutorials in Microsoft R product documentation will help you learn how to use the functions in the proprietary packages:  

+ [Get started with ScaleR](scaler-getting-started.md)

+ [Explore R and ScaleR in 25 functions](microsoft-r-tutorial-R2RevoScaleR.md)

+ [Introduction to MicrosoftML](microsoftml-introduction.md)

## See Also

[What's new in R Server](rserver-whats-new.md)

