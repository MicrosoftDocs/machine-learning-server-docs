---

# required metadata
title: "Introduction to Microsoft R Server and R Client"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/10/2017"
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

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and visualizations. As an open source technology, R is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Although R is widely used, deploying R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or requirements for scale and support in production environments emerge.

Microsoft R fills the functional void in enterprise deployments by providing a collection of servers and tools that extend the capabilities of R. The Microsoft R product family builds on top of open source R, offering free and commercial products in the form of Microsoft R Server, Microsoft R Client, and Microsoft R Open. In addition to the over 10,000 standard R packages available to all R users, Microsoft R Server and R Client include additional R packages and connectivity tools that enable remote compute context and remote execution, web service deployment, machine learning integration, and scalable solutions through clusters or parallelized workloads on platforms that support it.

<a name="compare-prods"></a>

## Compare products

The following table broadly compares members of the Microsoft R product family. All Microsoft R products are built on Microsoft R Open and install the package automatically.

|Component  |Role |Price | Support | Intended use |
|-----------|-----|------|---------|--------------|
|[Microsoft R Server](rserver.md) | Enterprise class server software | Commercial software | [Fully supported by Microsoft](rserver-servicing-support.md) | Adds custom functionality provided in proprietary Microsoft R packages, intended for local, remote, or distributed execution of larger datasets at scale.|
|[Microsoft R Client](r-client.md) | Workstation version of Microsoft R | Free | Community forums <sup>1</sup>| Adds custom functionality provided in proprietary Microsoft R packages, intended for development and local execution of in-memory datasets.|
|[Microsoft R Open](r-open.md) | Microsoft's distribution of open source R | Free | Community forums <sup>1</sup>| Use Microsoft R Open as you would any other distribution of R. Script written against Microsoft R Open is straight R, composed of basic functions provided in publically available R packages.|

<sup>1</sup> Microsoft does not offer technical support for issues encountered in either Microsoft R Open or Microsoft R Client, but you can get peer support in [MSDN forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ropen) and [StackOverflow](https://stackoverflow.com/questions/tagged/microsoft-r), to name a few.

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

<sup>2</sup> Because the Intel Math Kernel Library (MKL) is included in Microsoft R Open, the performance of a generic R solution is generally better. MKL replaces the standard R implementations of Basic Linear Algebra Subroutines (BLAS) and the LAPACK library with multithreaded versions. As a result, calls to those low-level routines tend to execute faster on Microsoft R than on a conventional installation of R.

## Interoperability with R language and across Microsoft R

R Server is built on open source R 3.3.2 and is 100% compatible with the R language. You can run any pure open source R solution on a Microsoft R Open, Microsoft R Client, or Microsoft R Server deployment.

Value-added packages like RevoScaleR, MicrosoftML, and mrsdeploy are available in both Microsoft R Client and Microsoft R Server. Although packages are equally available, the infrastructure backed by each product is substantially different. R Client is limited to in-memory data storage and can use a maximum of two processors.

R Server is the flagship product of the [Microsoft R product family](microsoft-r-getting-started.md) and supports much larger workloads. Data scientists typically switch to R Server when data and computational requirements cannot be accommodated on R Client.

Existing solutions developed with R Client can be deployed to R Server with minimal or no changes, but most developers make use of the additional functions, such as parallel and distributed computing that become available when you upgrade to R Server.

The [mrsdeploy package](mrsdeploy/mrsdeploy.md) gives you the ability to toggle between remote and local sessions in an R console application. As you change the compute context and make other adjustments to increase data size, you can set up a remote session and issue commands to validate your changes incrementally.



## Microsoft R Server

Microsoft R Server is "R for the Enterprise" and solves the problem of deploying and consuming of R code. It offers big-data capable R distributions for servers, Hadoop clusters, and data warehouses. It supports a variety of big data statistics, predictive modeling and machine learning capabilities, and provides you with analytics that are fully compatible with the R language, the de facto standard for modern analytics users.

Although generic R scripts tend to run faster on MRO via Intel MKL, the major benefit in terms of scale and performance comes from using ScaleR functions. ScaleR is available in both R Client and R Server, but only R Server adds support for remote execution, remote compute contexts, data chunking, additional threads for multithreaded processing, parallel processing, and streaming.

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools for doing just that; it is a full-featured web services software development kit for R that allows programmers to use in any language to integrate the R analysis output with a third party package. [Learn more about operationalizing](operationalize/about.md)

<a href="https://www.microsoft.com/en-us/cloud-platform/r-server" target="_blank"><b>Watch this 2-minute video introduction to Microsoft R Server.</b></a> 

> [!Note]
> Performance can be affected by external factors outside the R code, including competing demands on server resources, the type of query plan that is created, schema changes, the need to update statistics or create a new query plan, fragmentation and so on. It is possible that a stored procedure containing R code might run in seconds under one workload, but take minutes when there are other services running. We recommend that you monitor multiple aspects of server performance, including networking for remote compute contexts, when quantifying R job performance.

<a name="mrc"></a>
## Microsoft R Client

Microsoft R Client is a free, [community-supported](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MicrosoftR), data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R package to build your analytics. Additionally, R Client introduces the [powerful ScaleR technology](scaler-getting-started.md) and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as [SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility](r-client-compatibility.md). 

You can also run your code remotely on R Server using [remoteLogin() or remoteLoginAAD()](operationalize/remote-execution.md) from the `mrsdeploy` package to offload heavy processing on server or to test your analytics during their development. 

Learn how to [install and get started with Microsoft R Client](r-client-get-started.md).


## Microsoft R Open

Microsoft R Open is the enhanced distribution of R from Microsoft Corporation. It is a complete open source platform for statistical analysis and data science. Being based on the open source R engine makes Microsoft R Open fully compatibility with all R packages, scripts and applications that work with that version of R. Microsoft R Open delivers [performance boosts](https://mran.microsoft.com/documents/rro/multithread/#mt-bench), in comparison to the standard R distribution, since R Open leverages high-performance, multi-threaded math libraries. Like open source R from CRAN, Microsoft R Open is open source and free to download, use, and share. Microsoft R Open provides limited performance and scalability in comparison to Microsoft R Server and Microsoft R Client Editions. Specifically, none of the proprietary R packages installed with Microsoft R Server and Microsoft R Client are available in standalone Microsoft R Open. Also, data that can be processed is limited to the data that can fit in server memory.

Visit the [MRAN Website](https://mran.microsoft.com/) to learn more about Microsoft R Open and download it.

## Next Steps

If you are new to Microsoft R, we recommend starting with R Client and an integrated development environment like R Tools for Visual Studio (RTVS). This configuration is free of charge. It gives you Microsoft R Open with full support of all base R functions so that you can write R-only solutions, but also includes Microsoft R proprietary packages that run locally on your development computer.

Since the R engine lies underneath, you can use the R Core Team manuals that are part of every R distribution to learn how to code in R. Built-in manuals include *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and more. Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

However, because you have R Client, your script can also include functions shipped only with Microsoft R products, including the MicrosoftML, olapR, mrsdeploy, and RevoScaleR packages. All of these packages are available in both R Client and R Server, but at different levels of capacity.

Tutorials in Microsoft R product documentation will help you learn how to use the functions in the proprietary packages:  

+ [Get started with ScaleR](scaler-getting-started.md)

+ [Explore R and ScaleR in 25 functions](microsoft-r-getting-started-tutorial.md)

+ [Introduction to MicrosoftML](microsoftml-introduction.md)

## See Also

[What's new in R Server](rserver-whats-new.md)
