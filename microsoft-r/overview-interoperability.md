---

# required metadata
title: "How R language, R Server, and R Client relate"
description: "Understand the relationship and interaction between R language and Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "heidisteen"
manager: "jhubbard"
ms.date: "06/22/2017"
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

# Interoperability with R language and Microsoft R products and features

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and visualizations. As an open source technology, R is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Although R is widely used, deploying R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or requirements for scale and support in production environments emerge.

Microsoft R fills the functional void in enterprise deployments by providing a collection of servers and tools that extend the capabilities of R. The Microsoft R product family builds on top of open source R, offering free and commercial products in the form of [Microsoft R Server](rserver.md), [Microsoft R Client](r-client.md), and [Microsoft R Open](https://mran.microsoft.com/open/). In addition to the over 10,000 standard R packages available to all R users, Microsoft R Server and R Client include additional R packages and connectivity tools that enable remote compute context and remote execution, web service deployment, machine learning integration, and scalable solutions through clusters or parallelized workloads on platforms that support it.

## Packages in Microsoft R

R Server is built on open source R and is 100% compatible with the R language. You can run any pure open source R solution on a [Microsoft R Open](https://mran.microsoft.com/open/), [Microsoft R Client](r-client.md), or [Microsoft R Server](rserver.md) deployment.

Value-added packages like RevoScaleR, MicrosoftML, and mrsdeploy are available in both Microsoft R Client and Microsoft R Server. Although packages are equally available, the infrastructure backed by each product is substantially different. R Client is limited to in-memory data storage and can use a maximum of two processors.

R Server is the flagship product of the [Microsoft R product family](microsoft-r-getting-started.md) and supports much larger workloads. Data scientists typically switch to R Server when data and computational requirements cannot be accommodated on R Client.

Existing solutions developed with R Client can be deployed to R Server with minimal or no changes, but most developers make use of the additional functions, such as parallel and distributed computing that become available when you upgrade to R Server.

The [mrsdeploy package](mrsdeploy/mrsdeploy.md) gives you the ability to toggle between remote and local sessions in an R console application. As you change the compute context and make other adjustments to increase data size, you can set up a remote session and issue commands to validate your changes incrementally.

<a name="compare-prods"></a>

## Compare products

The following table broadly compares members of the Microsoft R product family. All Microsoft R products are built on Microsoft R Open and install the package automatically.

|Component  |Role |Price | Support | Intended use |
|-----------|-----|------|---------|--------------|
|[Microsoft R Server](rserver.md) | Enterprise class server software | Commercial software | [Fully supported by Microsoft](rserver-servicing-support.md) | Adds custom functionality provided in proprietary Microsoft R packages, intended for local, remote, or distributed execution of larger datasets at scale.|
|[Microsoft R Client](r-client.md) | Workstation version of Microsoft R | Free | Community forums <sup>1</sup>| Adds custom functionality provided in proprietary Microsoft R packages, intended for development and local execution of in-memory datasets.|
|[Microsoft R Open](https://mran.microsoft.com/open/) | Microsoft's distribution of open source R | Free | Community forums <sup>1</sup>| Use Microsoft R Open as you would any other distribution of R. Script written against Microsoft R Open is straight R, composed of basic functions provided in publically available R packages.<br><br>**Warning!** Microsoft R Client and R Server are built atop of specific versions of Microsoft R Open. If you are installing R Client or R Server, always use the R Open version provided by the installer (or described in the installation guides) to avoid compatibility errors. You can also install and use Microsoft R Open on its own by downloading the [R Open latest version](https://mran.microsoft.com).|

<sup>1</sup> Microsoft does not offer technical support for issues encountered in either Microsoft R Open or Microsoft R Client, but you can get peer support in [MSDN forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ropen) and [StackOverflow](https://stackoverflow.com/questions/tagged/microsoft-r), to name a few.


>[!IMPORTANT]
>For information on SQL Server Machine Learning Services, please visit the corresponding documentation here: https://docs.microsoft.com/en-us/sql/advanced-analytics/r/sql-server-r-services.

## Compare features by product

Features provided by [Microsoft R Server](rserver.md), [Microsoft R Client](r-client.md), and [Microsoft R Open](https://mran.microsoft.com/open/) can be categorized as shown in this table. This table slices key features by components. Additional capability provided in R Client and R Server is delivered via propertietary packages in Microsoft R.

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
            <td><b>Memory & Storage</b></td>
            <td>Memory bound<sup>1</sup></td>
            <td>Memory bound<sup>1</sup> &amp; operates on large volumes when connected to R Server.</td>
            <td>Memory across multiple nodes as well as data chunking across multiple disks.<br />Operates on bigger volumes &amp; factors.</td>
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

## See Also

[What's new in R Server](rserver-whats-new.md)
