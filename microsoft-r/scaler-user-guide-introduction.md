---

# required metadata
title: "What is RevoScaleR (Microsoft R)"
description: "Learn about the benefits of RevoScaleR and how to use it in custom script and code."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "0605/2017"
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
ms.technology: "r-server"
ms.custom: ""

---

# What is RevoScaleR?

RevoScaleR is a collection of proprietary functions in Microsoft R used for practicing data science at scale. For data scientists, RevoScaleR gives you data-related functions for import, transformation and manipulation, summarization, visualization, and analysis. *At scale* refers to the core engine's ability to perform these tasks against very large datasets, in parallel and on distributed file systems, chunking and reconstituting data when it cannot fit in memory.

RevoScaleR functions are provided through the **RevoScaleR** package installed for free in [Microsoft R Client](r-client.md) or commercially in [Microsoft R Server](rserver.md) on supported platforms. RevoScaleR is also embedded in Azure HDInsight, Azure Data Science virtual machines, and will be coming soon to Azure Machine Learning.

The RevoScaleR functions run on a computational engine include in the aforementioned products. As such, the package cannot be downloaded or used independently of the products and services that provide it.

RevoScaleR is engineered to adapt to the computational power of the platform it runs on. On R Server for Hadoop, script using RevoScaleR functions that run in parallel will automatically use nodes in the cluster. Whereas on the free R Client, scale is provided at much lower levels (2 processors, data resides in-memory).

RevoScaleR provides enhanced capabilities to many elements of the open source R programming language. In fact, there are [RevoScaleR equivalents for many common base R functions](r-reference/revoscaler/revoscaler-compared-to-base-r.md), such as *rxSort* for *sort()*, *rxMerge* for *merge()*, and so forth. Because Microsoft R is compatible with the open source R language, solutions often use a combination of base R and RevoScaleR functions. RevoScaleR functions are denoted with an **rx** or **Rx** prefix to make them readily identifiable in your R script that uses the RevoScaleR package.

## What can you do with RevoScaleR?

Data scientists and developers can include RevoScaleR functions in custom script or solutions that run locally against R Client or remotely on R Server. Solutions leveraging RevoScaleR functions will run wherever the RevoScaleR engine is installed.

A common workflow is to write the initial code or script against a subset of data on a local computer, change the compute context to specify a large set of data on a big data platform, and then operationalize the solution by deploying it to the target environment, thus making it accessible to users.

At a high level, RevoScaleR functions are grouped as follows:

* Platform-specific utilities.
* Data-related functions are used for import, transformation, summarization, visualization, and analysis. These functions comprise the bulk of the RevoScaleR function library.

The data manipulation and analysis functions in RevoScaleR are appropriate for small and large datasets, but are particularly useful in three common situations:

1. Analyze data sets that are too big to fit in memory.
2. Perform computations distributed over several cores, processors, or nodes in a cluster.
3. Create scalable data analysis routines that can be developed locally with smaller data sets, then deployed to larger data and/or a cluster of computers.

RevoScaleR enables these scenarios because it operates on chunks of data and uses *updating algorithms*.

Data is stored in an efficient XDF file format designed for rapid reading of arbitrary rows and columns of data. Functions in RevoScaleR are used to import data into XDF before performing analysis, but you can also work directly with data stored in a text, SPSS, or SAS file or an ODBC connection, or extract a subset of a data file into a data frame in memory for further analysis.

To perform an analysis, you must provide the following information: where the computations should take place (the compute context), the data to use (the data source), and what analysis to perform (the analysis function). 

## Data management and analysis using RevoScaleR

RevoScaleR provides functions for scalable data management and analysis. These functions can be used with data sets in memory, and applied the same way to huge data sets stored on disk. It includes functionality for:

- Accessing external data sets (SAS, SPSS, ODBC, Teradata, and delimited and fixed format text) for analysis in R

- Efficiently storing and retrieving data in a high performance data file

- Cleaning, exploring, and manipulating data

- Fast, basic statistical analyses

RevoScaleR also includes an extensible framework for writing your own analyses for big data sets.

With RevoScaleR, you can analyze data sets far larger than can be kept in memory. This is possible because RevoScaleR uses external memory algorithms that allow it to work on one chunk of data at a time (that is, a subset of the rows and perhaps the variables in the data set), update the results, and proceed through all available data.

### Accessing External Data Sets

Data can be stored in a wide-variety of formats. Typically, the first step in any RevoScaleR analysis is to make the data accessible. With RevoScaleR’s data import capability, you can access data from a SAS file, SPSS file, fixed format or delimited text file, an ODBC connection, SQL Server, or a Teradata database, bringing it into a data frame in memory, or storing it for fast access in chunks on disk.

<a name="compute-context"></a>
### Defining Compute Context

RevoScaleR has the concept of *compute context* that sets the location for data processing (subsets, aggregations, transformations, and so forth) and analysis. The compute context is either local or remote, where remote offloads processing and analysis of chunked data to one or more remote R Servers.

Local is the default, and it supports the full range of data source inputs. As its name suggests, a local compute context uses only the physical cores of the local computer. Local compute context is provided by RevoScaleR on both R Client and R Server instances.

Remote compute context requires the explicit creation of a compute context object, a single logical object defining location (a remote network resource that has R Server and local data) and modes of processing (such as wait versus no-wait jobs). Remote compute context is supported for RevoScaleR analytical functions that can be performed in a distributed fashion, and is available on these platforms in R Server only: HDInsight, Hadoop (both MapReduce and Spark), Teradata, SQL Server, and R Server (Windows and Linux). For more information, see [Compute Context](scaler-user-guide-introduction.md#compute-context).

### Efficiently Storing and Retrieving Data

A key component of RevoScaleR is a data file format (.xdf) that is extremely efficient for both reading and writing data. You can create .xdf files by importing data files or from R data frames, and add rows or variables to an existing .xdf file (appending rows is currently only supported in a local compute context). Once your data is in this file format you can use it directly with analysis functions provided with RevoScaleR, or quickly extract a subsample and read it into a data frame in memory for use in other R functions.

### Data Cleaning, Exploration, and Manipulation

When working with a new data set, the first step is to clean and explore. With RevoScaleR, you can quickly obtain information about your data set (e.g., how many rows and variables) and the variables in your data set (e.g., name, data type, value labels). With RevoScaleR’s summary statistics and cube functionality, you can examine summary information about your data and quickly plot histograms or relationships between variables.

RevoScaleR also provides all of the power of R to use in data transformations and manipulations. In RevoScaleR’s data step functionality, you can specify R expressions to transform specific variables and have them automatically applied to a single data frame or to each block of data as it is read in from an .xdf file. You can create new variables, recode variables, and set missing values with all the flexibility of the R language.

### Statistical Analysis

In addition to descriptive statistics and crosstabs, RevoScaleR provides functions for fitting linear and binary logistic regression models, generalized linear models, k-means models, and decision trees and forests, among others. These functions access .xdf files or other data sources directly or operate on data frames in memory. Because these functions are so efficient and do not require that all of the data be in memory at one time, you can analyze huge data sets without requiring huge computing power. In particular, you can relax assumptions previously required. For example, instead of assuming a linear or polynomial functional form in a model, you can break independent variables into many categories providing a completely flexible functional form. The many degrees of freedom provided by large data sets, combined with RevoScaleR’s efficiency, make this approach feasible.

### Writing Your Own Analyses for Large Data Sets

All of the main analysis functions in RevoScaleR use updating or external memory algorithms, that is, they analyze a chunk of data, update the results, then move on to the next chunk of data and repeat the process. When all the data has been processed (sometimes multiple times), final results can be calculated and the analysis is complete. You can write your own functions to process a chunk of data and update results, and use RevoScaleR functionality to provide you with access to your data file chunk by chunk. For more information, see [Write custom chunking algorithms in RevoScaleR](scaler-getting-started-4-write-chunking-algorithms.md).

## See Also

 [Introduction to Microsoft R](microsoft-r-getting-started.md)   
 [Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md) 
 [RevoScaleR Functions](scaler/scaler.md)   
