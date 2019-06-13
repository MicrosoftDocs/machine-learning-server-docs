---

# required metadata
title: "Product comparison for Machine Learning Server and related components "
description: "Understand the relationship and interaction between Machine Learning Server, R components, and Python components."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "07/15/2019"
ms.topic: "conceptual"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Compare Machine Learning Server and related tools

The following table broadly compares members of the Machine Learning Server and client apps. All products with R support are built on Microsoft R Open and install the package automatically. All products with Python support are built on Anaconda.

There is no Python client or workstation version for Machine Learning Server that is equilvalent to R Client. You can [install the Python client libraries](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter), or a developer edition or an evaluation edition of Machine Learning Server if you require a free-of-charge copy for study and development.

SQL Server provides an embedded machine learning feature that provides identical packages (RevoScaleR, revoscalepy, and so forth). The packages and run time environment run within the security and operational context of SQL Server, which fundamentally changes how you develop and deploy R and Python code. Despite having the same components, the installation and servicing is differnet. If you already use SQL Server, we strongly recommend that you look into the machine learning features in SQL Server. For more information, see [SQL Server Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/home-advanced-analytics-r-machine-learning-sql-server).

|Component  |Role |Price | Support | Intended use |
|-----------|-----|------|---------|--------------|
|[Machine Learning Server](what-is-machine-learning-server.md) | Enterprise class server software | Commercial software | [Fully supported by Microsoft](resources-servicing-support.md) | Adds custom functionality provided in proprietary Microsoft R and Python packages, intended for local, remote, or distributed execution of larger datasets at scale. This product was formerly known as [Microsoft R Server](rebranding-microsoft-r-server.md). |
|[Microsoft R Client](r-client/what-is-microsoft-r-client.md) | Workstation version | Free | Community forums <sup>1</sup>| Adds custom functionality provided in proprietary Microsoft R packages, intended for development and local execution of in-memory datasets.|
|[Microsoft R Open](https://mran.microsoft.com/open/) | Microsoft's distribution of open-source R | Free | Community forums <sup>1</sup>| Use Microsoft R Open as you would any other distribution of R. Script written against Microsoft R Open is straight R, composed of basic functions provided in publically available R packages.<br><br>**Warning!** Microsoft R Client and Machine Learning Server are built atop of specific versions of Microsoft R Open. If you are installing R Client or Machine Learning Server, always use the R Open version provided by the installer (or described in the installation guides) to avoid compatibility errors. You can also install and use Microsoft R Open on its own by downloading the [R Open latest version](https://mran.microsoft.com).|

<sup>1</sup> Microsoft does not offer technical support for issues encountered in either Microsoft R Open or Microsoft R Client, but you can get peer support in [MSDN forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ropen) and [StackOverflow](https://stackoverflow.com/questions/tagged/microsoft-r), to name a few.

## Compare features by product

Features provided by [Machine Learning Server](what-is-microsoft-r-server.md), [Microsoft R Client](r-client/what-is-microsoft-r-client.md), and [Microsoft R Open](https://mran.microsoft.com/open/) can be categorized as shown in this table. This table slices key features by components. Additional capability provided in R Client and Machine Learning Server is delivered via proprietary packages.


||Microsoft&nbsp;R&nbsp;Open|Microsoft&nbsp;R&nbsp;Client|Machine&nbsp;Learning&nbsp;Server|
|-----------|-----|------|---------|
|<b>Memory & Storage</b>|Memory bound<sup>1</sup>|Memory bound<sup>1</sup> &amp; operates on large volumes when connected to R Server.|Memory across multiple nodes as well as data chunking across multiple disks.<br />Operates on bigger volumes &amp; factors.|
|<b>Speed of Analysis</b>|Multithreaded via MKL<sup>2</sup> for non-RevoScaleR functions.|Multithreaded via MKL<sup>2</sup> for non-RevoScaleR functions, but only up to 2 threads for RevoScaleR functions with a local compute context.|Full parallel threading &amp; processing for revoscalepy and RevoScaleR functions as well as for non-proprietary functions (via MKL<sup>2</sup>) in both local and remote compute contexts.|
|<b>Analytic Breadth &amp; Depth</b>|Open-source&nbsp;packages only.|Open-source R packages plus proprietary packages.|Open-source R packages plus proprietary packages with support for parallelization and distributed workloads. Proprietary Python packages for solutions written in that language.|
|<b><a href="what-is-operationalization.md">Operationalizing</a> R Analytics</b>|Not available|Not available|Includes the instant deployment and easy consumption of R analytics, interactive remote code execution, speedy real-time scoring, scalability, and enterprise-grade security.|

<sup>1</sup> Memory bound because product can only process datasets that fit into the available memory.

<sup>2</sup> Because the Intel Math Kernel Library (MKL) is included in Microsoft R Open, the performance of a generic R solution is generally better. MKL replaces the standard R implementations of Basic Linear Algebra Subroutines (BLAS) and the LAPACK library with multithreaded versions. As a result, calls to those low-level routines tend to execute faster on Microsoft R than on a conventional installation of R.

## See Also

[What's new in Machine Learning Server](whats-new-in-machine-learning-server.md)
