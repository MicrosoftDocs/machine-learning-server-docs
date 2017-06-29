---

# required metadata
title: "Microsoft R Glossary"
description: "DeployR and R Server Glossary Terms FAQ"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "06/16/2017"
ms.topic: "get-started-article"
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
  - deployr
#ms.custom: ""

---

# Microsoft R Glossary

---
**Quick Links to Terms:** &nbsp;&nbsp;&nbsp;&nbsp;  A &nbsp;B &nbsp;[C](#C) &nbsp;[D](#D) &nbsp;E &nbsp;F &nbsp;G &nbsp;[H](#H) &nbsp;I &nbsp;J &nbsp;K &nbsp;L &nbsp;[M](#M) &nbsp;N &nbsp;[O](#O) &nbsp;[P](#P) &nbsp;Q &nbsp;[R](#R) &nbsp;[S](#S) &nbsp;T &nbsp;[U](#U) &nbsp;V &nbsp;[W](#W) &nbsp;[X](#X) &nbsp;Y &nbsp;Z</big>

---

>Didn't find what you were looking for?  Suggest a term below.
>
>For general R language terminology, check out CRAN's [R Language Definition](https://cran.r-project.org/doc/manuals/r-release/R-lang.pdf).
>
>Also, check out our [More Resources](resources-more.md) for other publications and resources.


Use this glossary to find the definitions to common terms in the Microsoft R documentation.


<a name="C"></a>

---

## C

**Compute Context**

A feature in [RevoScaleR](r-reference/revoscaler/revoscaler.md) that lets you define an environment, either local or remote, and then transfer R computations to that environment, typically to get better performance or to minimize data transfer. Local is the default. Remote compute context is available for these platforms: SQL Server, HDInsight, Teradata, Hadoop MR and Spark, and Microsoft R Server (Linux and Windows). [Learn more…](r/how-to-revoscaler-distributed-computing.md)



<br>

<a name="D"></a>

---

## D 

**Data chunking**

Using ScaleR on R Server, this is the ability to partition data into multiple parts for processing, reassembling it later for analysis. [Learn more…](r/tutorial-revoscaler-data-import-transform.md#chunking)

<br>

**DeployR**

See [<i>Operationalizing Analytics</i>](#o16n)

<br>

**Distributed computing**

The breakdown of a complicated computation into pieces that can be performed independently, while maintaining a framework that allows for the results of those independent computations to be put together to create the final result. Distributed comput is similar to parallel computing, but in Microsoft R, it specifically refers to workload distribution across multiple physical servers. [Learn more…](r/how-to-revoscaler-distributed-computing.md)


<a name="H"></a>

---

## H 

<a name="hpa"></a>**High-performance analytics (HPA)**

HPA is paradigm describing the distribution of data across multiple cores by means of efficient disk I/O, threading, and data management in memory. Instead of passing large amounts of data from node to node, the computations are distributed to the data. <p/> ScaleR, which is designed to process large data one chunk at a time, is also designed to process each chunk of data independently and in parallel. Each computing resource needs access only to that portion of the total data source required for its particular computation. <p/> In ScaleR, HPA is evident in functions such as rxLinMod and other RevoScaleR analytics functions that focus on efficiently feeding data to available cores by means of efficient disk I/O, threading, and data management in memory. [Learn more…](r/how-to-developer-manage-threads.md)

<br>

<a name="hpc"></a>**High-performance computing (HPC)**

HPC is a paradigm for sharing tasks among multiple computing resources. HPC mechanisms are CPU-centric, involving tremendous amounts of processing on relatively small amounts of data. Common tasks tackled with HPC mechanisms include the family of [embarrassingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel) problems, a term used to describe workloads that are naturally modular, self-contained, and independent. Examples include element-by-element computations on arrays, or computation of membership in the Mandelbrot set. This family of problems also includes many types of simulation, where each individual run is independent. <p/> In ScaleR, HPC refers to functions such as rxExec, foreach, and rmpi that are CPU-centric, involving tremendous amounts of processing on relatively small amounts of data. HPC functions are optimized to share tasks across available computing resources, but can be slowed if large amounts of data need to be transferred.


<a name="M"></a>

---

## M 

<a name="mrc"></a>**Microsoft R Client**

Microsoft R Client is a free, community-supported, data science tool for high performance analytics. R Client is built on top of Microsoft R Open and includes RevoScaleR so you can benefit from parallelization and remote computing. [Learn more…](r-client/what-is-microsoft-r-client.md)

<br>

<a name="mro"></a>**Microsoft R Open**

Microsoft R Open is the enhanced distribution of open source R from Microsoft that is fully compatibility with all R packages, scripts and applications. Enhancements include multi-core processing, a fixed CRAN repository date, and reproducible R with the checkpoint package. Learn more at https://mran.microsoft.com/open/. 

<br>

<a name="mrs"></a>**Microsoft R Server**

Use R—the powerful, statistical programming language—in an enterprise-class, big data analytics platform. R Server, built on ScaleR technology, is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers (Linux and Windows) and clusters (Hadoop and Apache Spark). [Learn more…](what-is-microsoft-r-server.md)

<br>

<a name="mkl"></a>**MKL**

The Intel® Math Kernel Library (Intel® MKL) is a library of optimized math routines. This library makes it possible for so many common R operations, such as matrix multiply/inverse, matrix decomposition, and some higher-level matrix operations, to compute in parallel and use all of the processing power available to reduce computation times. [Learn more…](https://mran.microsoft.com/documents/rro/multithread/)

<a name="O"></a>

---

## O

<a name="o16n"></a>**Operationalizing Analytics**

Configure R Server to act as a deployment engine to publish your analytics and enable them to be easily consumed in a production environment. [Learn more…](what-is-operationalization.md)


<a name="P"></a>

---

## P

<a name="parallel"></a>**Parallel computing**

A process that breaks a computing task into segments that can be executed independently on separate threads, cores, or computers. Multiple architectures support parallel computing: SMP means parallel processing on a single computer with multiple processors, each running a different set of commands; MPP means processing a task across multiple computers. In both SMP and MPP, the results from all processes are combined at the end to give a single result. ScaleR performs parallel computing on any computer with multiple computing cores. [Learn more…](r/how-to-revoscaler-distributed-computing.md)
 


<a name="R"></a>

---

## R

**R Client**

See <a href="#mrc"><i>Microsoft R Client</i></a>

<br>

**R Open**

See <a href="#mro"><i>Microsoft R Open</i></a>

<br>

**R Server**

See <a href="#mrs"><i>Microsoft R Server</i></a>

<br>

**RevoScaleR**

A proprietary R package by Microsoft that provides scalable, fast (multicore), and extensible data analysis  for large datasets with R.  It offers parallel external memory algorithms that help R break through memory and performance limitations. This package is available with Microsoft R Server, SQL Server Machine Learning Services, and Microsoft R Client. [Learn more…](r-reference/revoscaler/revoscaler.md)

<br>

<a name="S"></a>

---

## S

**ScaleR**

See <i>RevoScaleR</i>

<br>

**SQL Server Machine Learning Services**

A feature included in SQL Server 2016 as SQL Server R Services and then renamed in SQL Server 2017 when support for Python was added to the already supported R language in T-SQL stored procedures. This product integrates an R run-time with optimizations in SQL Server, including resource management and data security.


<a name="U"></a>

---

## U

<a name="updatingalgorithm"></a>**Updating algorithm**

An algorithm that takes a given set of values and a chunk of data, and then outputs a revised set of values cumulative for all chunks. The simplest example is an updating sum: sum is computed for the first chunk, followed by a second chunk, which each successive chunk contributing to a revised value until reaching the cumulative sum. Updating algorithms are used in ScaleR.


<a name="W"></a>

---

## W

**Write Once, Deploy Anywhere (WODA)**

Refers to the ability to create script locally, with the option of running it remotely on any supported platform with minimal changes. In practice, some distributed platforms have specialized data handling requirements. You may have to specify a context-specific *data source* along with the *compute context* to enable execution on a different platform. In most cases, the bulk of your analysis scripts can proceed with no further changes.

<a name="X"></a>

---

## X

**XDF File Format**

The External Data Frame (XDF) file format is a high-performance, binary file format for storing big data sets for use with RevoScaleR. This file format has an R interface and optimizes rows and columns for faster processing and analysis.  [Learn more…](https://msdn.microsoft.com/en-us/microsoft-r/scaler-user-guide-introduction)
