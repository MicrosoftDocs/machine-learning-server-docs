---

# required metadata
title: "R function library reference (Microsoft R) | Microsoft Docs"
description: "Function reference for R packages in Microsoft R, including MicrosoftML, mrsdeploy, RevoScaleR, RevoPemaR, and others."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "08/17/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
  - r-client
  - r-server
#ms.custom: ""

---

# R function library reference

This section contains the function reference documentation for proprietary *revo* packages installed with Microsoft R Client and R Server, or available in a service (SQL Server 2016 or later, or Azure). Each package offers functions used for specific categories of operations. 

You can use these libraries and functions in combination with other open source or third-party packages, but to use the *revo* packages, your R code must run against a service or on a computer that has a Microsoft R installation.

The function libraries are built on **R 3.3.0**.

|Package | Version | Description |
|--------|---------|-------------|
|[MicrosoftML](microsoftml/microsoftml-package.md) | 1.3.0  | A collection of functions in Microsoft R used for machine learning at scale.|
|[mrsdeploy](mrsdeploy/mrsdeploy-package.md) | 1.1.0 | Deployment functions for interactive remote execution at the command line, plus web service functions for bundling up R code blocks as discrete web services that can be deployed and managed on an R Server instance. Formally known as DeployR. |
|[olapR](olapr/olapr.md) | 1.0.0 | A collection of functions for constructing MDX queries against an OLAP cube. Runs only on the Windows platform.|
|RevoIOQ | 8.0.6 | Installation and Operational Qualification test functions, used in conjunction with the RUnit package to run a set of unit tests. It has only one user-facing function, also called **RevoIOQ**. Reference documentation is online only (`*`). |
|RevoMods | 11.0.0 | Microsoft modifications and extensions to standard R functions. Reference documentation is online only (`*`).  |
|[RevoPemaR](revopemar/pemar.md) | 10.0.0 | Developer functions for coding custom parallel external memory algorithms. |
|[RevoScaleR](~/r-reference/revoscaler/revoscaler.md) | 9.1.0 | Data acquisition, manipulation and transformations, visualization, and analysis. RevoScaleR provides functions for the full range of statistical and analytical tasks. It's the backbone of [R Server](../what-is-microsoft-r-server.md) functionality. |
|RevoTreeView | 10.0.0 | Decision tree functions, including the **rxDTree** function. Reference documentation is online only (`*`). |
|[RevoUtils](revoutils/revoutils.md) | 10.0.3 | Utility functions useful when programming and developing R packages.|
|RevoUtilsMath | 10.0.0 | Microsoft's distribution of the Intel Math Kernal Library (MKL). Reference documentation is online only (`*`). |
|[sqlrutils](sqlrutils/sqlrutils.md) | 1.0.0 | A collection of functions for executing stored procedures against SQL Server.|

`*` Learn more about this package by typing ?"packagename" in the RGUI.exe console window or in the R Help page in your preferred R IDE, such as **R Tools for Visual Studio**.

## How to get packages

The packages documented in this section are found only on installations of the Microsoft products or Azure services that provide them. Setup programs or scripts install the propertietary R packages from Microsoft and any package dependencies. Unless noted otherwise, all of the packages listed in the preceding table are installed with the product or service.

By default, packages are installed in the \Program Files\Microsoft\R Server\R_SERVER\library folder on Windows, and in the /usr/lib64/microsoft-r folder on the Linux native file system.

## How to list packages and package versions

To get the version of an R package installed on your ocmputer, open an R console application and execute the following command: `installed.packages()`

## How to view built-in help pages

Most R packages come with built-in help pages that open in separate window.

To see the **RevoScaleR** functions that can be called from the R console:

1. Open an R console tool, such as Rgui.exe or another R IDE.
2. List the packages using `installed.packages()`
3. Open help for a specific package using: `library(help="<package-name>")`
4. Open help for a specific funtion using: `?<function_name>`

R is case-sensitive. Be sure to type the package name using the correct case: `library(help = "RevoScaleR")`.

## Deprecated & discontinued packages

The following packages exist for backward compatibility but are no longer under active development:

* RevoRpeConnector
* RevoRsrConnector
* revolpe

For a list of deprecated or discontinued functions within an existing package, see [Release notes for R Server](../resources-deprecated-features.md).

## Next steps

**RevoScaleR** is the most commonly used package in Microsoft R. To try it out, see [Explore R and RevoScaleR in 25 functions](../r/tutorial-r-to-revoscaler.md).

If you do not have the packages, [install R Client](../r-client/what-is-microsoft-r-client.md) or [R Server](../what-is-microsoft-r-server.md) to add the packages to your computer.

## See also

 [Diving into data analysis in Microsoft R](../r/how-to-introduction.md)  
 [Microsoft R Getting Started Guide](../microsoft-r-getting-started.md) 
 [Additional learning resources and sample datasets](../resources-more.md)  
 
 
