---

# required metadata
title: "Microsoft R Glossary"
description: "DeployR and R Server Glossary Terms FAQ"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "11/30/2016"
ms.topic: "article"
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
  - deployr
ms.custom: ""

---

# Microsoft R Glossary

<hr>


<big>**Quick Links:** &nbsp;&nbsp;&nbsp;&nbsp;  A &nbsp;B &nbsp;[C](#C) &nbsp;D &nbsp;E &nbsp;F &nbsp;G &nbsp;[H](#H) &nbsp;I &nbsp;J &nbsp;K &nbsp;L &nbsp;[M](#M) &nbsp;N &nbsp;O &nbsp;[P](#P) &nbsp;Q &nbsp;[R](#R) &nbsp;[S](#S) &nbsp;T &nbsp;[U](#U) &nbsp;V &nbsp;[X](#X) &nbsp;Y &nbsp;Z</big>

<hr>

>Didn't find what you were looking for?  Suggest a term using the **"Is this page helpful?"** link below.
>
>For general R language terminology, check out CRAN's [R Language Definition](https://cran.r-project.org/doc/manuals/r-release/R-lang.pdf).
>
>Also, check out our [More Resources](microsoft-r-more-resources.md) for other publications and resources.

Use this glossary to find the definitions to common terms in the Microsoft R documentation.

<!--
<br>

<a name="A"></a>
## A

<br>

<a name="B"></a>
## B

-->

<a name="C"></a>
<hr>
<big><b> C </b></big>

**Compute Context**
<div style="margin:15px; margin-bottom:25px;">A feature in [RevoScaleR](scaler/scaler.md) that lets you define an environment, either local or remote, and then transfer R computations to that environment, typically to get better performance or to minimize data transfer. Local is the default. Remote compute context is available for these platforms: SQL Server, HDInsight, Teradata, Hadoop MR and Spark, and Microsoft R Server (Linux and Windows). <a href="scaler-distributed-computing.md">Learn more…</a></div>



<br>

<a name="D"></a>
<hr>
<big><b> D </b></big>

**Data chunking**
<div style="margin:15px; margin-bottom:25px;">Using ScaleR on R Server, this is the ability to partition data into multiple parts for processing, reassembling it later for analysis. <a href="scaler-getting-started.md#data-chunking-and-scaler">Learn more…</a></div>

**DeployR**
<div style="margin:15px; margin-bottom:25px;">See <a href="#o16n"><i>Operationalization</i></a></div>

**Distributed computing**
<div style="margin:15px; margin-bottom:25px;">Similar to parallel computing, but in Microsoft R, it specifically refers to workload distribution across multiple servers. <a href="scaler-distributed-computing.md">Learn more…</a></div>

<!--
<br>

<a name="E"></a>
<hr>
<big><b> E </b></big>

**Term**

<br>

<a name="F"></a>
<hr>
<big><b> F </b></big>

**Term**

<br>

<a name="G"></a>
<hr>
<big><b> G </b></big>

**Term**

-->
<br>

<a name="H"></a>
<hr>
<big><b> H </b></big>

<a name="hpa"></a>**High-performance analytics (HPA)**
<div style="margin:15px; margin-bottom:25px;">In ScaleR, HPA refers to functions such as rxLinMod and other RevoScaleR analytics functions that focus on efficiently feeding data to available cores by means of efficient disk I/O, threading, and data management in memory. <a href="scaler-user-guide-manage-threads.md">Learn more…</a></div>

<a name="hpc"></a>**High-performance computing (HPC)**
<div style="margin:15px; margin-bottom:25px;">In ScaleR, HPC refers to functions such as rxExec, foreach, and rmpi that are CPU-centric, involving tremendous amounts of processing on relatively small amounts of data. HPC functions are optimized to share tasks across available computing resources, but can be slowed if large amounts of data need to be transferred.</div>

<!--
<br>

<a name="I"></a>
<hr>
<big><b> I </b></big>

**Term**

<br>

<a name="J"></a>
<hr>
<big><b> J </b></big>

**Term**

<br>

<a name="K"></a>
<hr>
<big><b> K </b></big>

**Term**



<br>

<a name="L"></a>
<hr>
<big><b> L </b></big>

**Term**


-->

<br>

<a name="M"></a>
<hr>
<big><b> M </b></big>

<a name="mrc"></a>**Microsoft R Client**
<div style="margin:15px; margin-bottom:25px;">Microsoft R Client is a free, community-supported, data science tool for high performance analytics. R Client is built on top of Microsoft R Open and includes RevoScaleR so you can benefit from parallelization and remote computing. <a href="r-client.md">Learn more…</a></div>


<a name="mro"></a>**Microsoft R Open**
<div style="margin:15px; margin-bottom:25px;">Microsoft R Open is the enhanced distribution of open source R from Microsoft that is fully compatibility with all R packages, scripts and applications. Enhancements include multi-core processing, a fixed CRAN repository date, and reproducible R with the checkpoint package.  <a href="https://mran.microsoft.com/open/" target=_blank>Learn more…</a></div>


<a name="mrs"></a>**Microsoft R Server**
<div style="margin:15px; margin-bottom:25px;">Use R—the powerful, statistical programming language—in an enterprise-class, big data analytics platform. R Server, built on ScaleR technology, is an enterprise class server for hosting and managing parallel and distributed workloads of R processes on servers (Linux and Windows) and clusters (Hadoop and Apache Spark). <a href="rserver">Learn more…</a></div>


<a name="mkl"></a>**MKL**
<div style="margin:15px; margin-bottom:25px;">The Intel® Math Kernel Library (Intel® MKL) is a library of optimized math routines. This library makes it possible for so many common R operations, such as matrix multiply/inverse, matrix decomposition, and some higher-level matrix operations, to compute in parallel and use all of the processing power available to reduce computation times. <a href="https://mran.microsoft.com/documents/rro/multithread/">Learn more…</a></div>

<!--<br>

<a name="N"></a>
<hr>
<big><b> N</b></big>

**Term**

-->

<br>

<a name="O"></a>
<hr>
<big><b> O </b></big>

<a name="o16n"></a>**Operationalization**
<div style="margin:15px; margin-bottom:25px;">Configure the operationalization feature for Microsoft R Server to act as a deployment server and host analytic web services. <a href="operationalize/about.md">Learn more…</a></div>


<a name="P"></a>
<hr>
<big><b> P </b></big>

<a name="parallel"></a>**Parallel computing**
<div style="margin:15px; margin-bottom:25px;">A process that breaks a computing task into segments that can be executed independently on separate threads, cores, or computers. Multiple architectures support parallel computing: SMP means parallel processing on a single computer with multiple processors, each running a different set of commands; MPP means processing a task across multiple computers. In both SMP and MPP, the results from all processes are combined at the end to give a single result. <a href="scaler-distributed-computing.md">Learn more…</a></div>

<!--
<br>

<a name="Q"></a>
<hr>
<big><b> Q </b></big>

**Term**

-->

<br>

<a name="R"></a>
<hr>
<big><b>R</b></big>

**R Client**
<div style="margin:15px; margin-bottom:25px;">See <a href="#mrc"><i>Microsoft R Client</i></a></div>


**R Open**
<div style="margin:15px; margin-bottom:25px;">See <a href="#mro"><i>Microsoft R Open</i></a></div>

**R Server**
<div style="margin:15px; margin-bottom:25px;">See <a href="#mrs"><i>Microsoft R Server</i></a></div>

**RevoScaleR**
<div style="margin:15px; margin-bottom:25px;">A proprietary R package by Microsoft that provides scalable, fast (multicore), and extensible data analysis  for large datasets with R.  It offers parallel external memory algorithms that help R break through memory and performance limitations. This package is available with Microsoft R Server, SQL Server R Services, and Microsoft R Client.   <a href="scaler/scaler.md">Learn more…</a></div>

<br>

<a name="S"></a>
<hr>
<big><b> S </b></big>

**ScaleR**
<div style="margin:15px; margin-bottom:25px;">See <i>RevoScaleR</i></div>

**SQL Server R Services**
<div style="margin:15px; margin-bottom:25px;">A feature included in SQL Server 2016 and after that supports execution of R scripts in T-SQL stored procedures. R Services integrates an R run-time with optimizations in SQL Server, including resource management and data security.</div>

<!--
<br>

<a name="T"></a>
<hr>
<big><b> T </b></big>

**Term**

-->
<br>

<a name="U"></a>
<hr>
<big><b> U </b></big>

<a name="updatingalgorithm"></a>**Updating algorithm**
<div style="margin:15px; margin-bottom:25px;">An algorithm that takes a given set of values and a chunk of data, and then outputs a revised set of values cumulative for all chunks. The simplest example is an updating sum: sum is computed for the first chunk, followed by a second chunk, which each successive chunk contributing to a revised value until reaching the cumulative sum. Updating algorithms are used in ScaleR.</div>

<!--
<br>

<a name="V"></a>
<hr>
<big><b> V </b></big>

**Term**


<br>

<a name="W"></a>
<hr>
<big><b> W </b></big>

**Term**


-->
<br>

<a name="X"></a>
<hr>
<big><b> X </b></big>

**XDF File Format**
<div style="margin:15px; margin-bottom:25px;">The External Data Frame (XDF) file format is a high-performance, binary file format for storing big data sets for use with RevoScaleR. This file format has an R interface and optimizes rows and columns for faster processing and analysis.  <a href="https://msdn.microsoft.com/en-us/microsoft-r/scaler-user-guide-introduction">Learn more…</a></div>

<!--
<br>

<a name="Y"></a>
<hr>
<big><b>Y</b></big>

**Term**


<br>

<a name="Z"></a>
<hr>
<big><b> Z </b></big>

**Term**

-->


<br>
<br>

## Suggest terms with <br>"Is this page helpful?"
