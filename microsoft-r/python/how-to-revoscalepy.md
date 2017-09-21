---

# required metadata
title: "How to use revoscalepy with Apache Spark - Machine Learning Server  | Microsoft Docs "
description: "Learn what you can do with revoscalepy in a Spark compute context in Machine learning Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/19/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: ""
#ms.custom: ""

---

# How to use revoscalepy in a Spark compute context

This article introduces Python functions in a **revoscalepy** package with Apache Spark (Spark) running on a Hadoop cluster. 

+ Hadoop provides a distributed file system with HDFS.
+ Yarn provides the job scheduling infrastructure.
+ Spark provides the processing framework. 
+ revoscalepy provides scalable and high-performance data management, analysis, and visualization Python functions. 

When you set the [compute context](../r/concept-what-is-compute-context.md) to [RxSpark](../python-reference/revoscalepy/rxSpark.md), revoscalepy functions automatically distribute the workload across all the data nodes. There is no requirement for managing jobs or the queue, or tracking the physical location of data in HDFS; Spark does both tasks for you.

> [!Note]
> For installation instructions, see [Install Machine Learning Server for Hadoop](../install/machine-learning-server-hadoop-install.md).

## Set a Spark compute context and manage connections

When you use an IDE to connect to Machine Learning Server in a Hadoop cluster, the connection should be to the server (**mlserver-python** program) running on the edge node. 

**Local compute context on Spark**

By default, the local compute context is the computing environment of the edge node. All mlserver-python code runs here until you specify a remote compute context.

**Remote compute context on Spark**

From the edge node, you can push computations to the data layer by creating a remote Spark compute context. In this context, execution is on all data nodes. In Machine Learning Server, revoscalepy includes support for remote compute context on Spark (2.0-2.4).

The following example shows how to set a remote compute context to clustered data nodes, execute functions in the Spark comute context, switch back to a local compute context, and disconnect from the server.

```Python
# rxSparkConnect, Disconnect, RxSetComputeContext
from revoscalepy import RxOrcData, rx_spark_connect, rx_spark_list_data, rx_lin_mod, rx_spark_cache_data
cc = rx_spark_connect()
col_info = {"DayOfWeek": {"type": "factor"}}
df = RxOrcData(file = "/share/sample_data/AirlineDemoSmallOrc", column_info = col_info)
df = rx_spark_cache_data(df, True)

# After the first run, a Spark data object is added into the list
rx_lin_mod("ArrDelay ~ DayOfWeek", data = df)
rx_spark_list_data(True)

# Disconnect, switching back to a local compute context on edge node
rx_spark_disconnect(cc)
rx_get_compute_context()
```

## Specify a data source and location

As part of execution in Spark, your data source must be a format that Spark understands, such as text, Hive, Orc, and Parquet. You can also create and consume [.xdf files](../r/concept-what-is-xdf.md), a data file format native to Machine Learning Server, accessible in Python and R script.

Data source objects provided by revoscalepy in a Spark compute context include [RxTextData](../python-reference/revoscalepy/rxtextdata.md) [RxXdfData](../python-reference/revoscalepy/rxxdfdata.md), and the [RxSparkData](../python-reference/revoscalepy/rxSparkdata.md) with derivatives for RxHiveData, RxOrcData, RxParquetData and RxSparkDataFrame.

### Create a data source

The following example illustrates an Xdf data source object that pulls data from a local sample directory created when you install Machine Learning Server. The "sampleDataDir" argument is a reference to the sampleDataDir folder, known to revoscalepy.

```python
import os
import revoscalepy

sample_data_path = revoscalepy.RxOptions.get_option("sampleDataDir")
d_s = revoscalepy.RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
```

### Import data into a data frame

Data is automatically loaded into a data frame, but you can load it explicity using the rx_import and following syntax. In mlserver-python, you can use head and tail functions, similar to R, to return the first or last part of the data set.

```python
airlinedata = rx_import(input_data = d_s)
airlinedata.head()
```

## Get information from revoscalepy

+ Get the edition, which determines capacity/capability available to you.
+ List data sources
+ Read objects

## Summarize data

To quickly understand fundamental characteristics of your data, use the **rx_summary** function to return basic statistical descriptors. Mean, standard deviation, and min-max values. A count of total observations, missing observations, and valid observations is included.

A minimum specification of the [rx_summmary](../python-reference/revoscalepy/rx-summary.md) function consists of a valid data source object and a formula giving the fields to summarize.

The formula is symbolic and typically does not contain a response variable. It should be of the form of ~ terms. 

+ Get the term list from a data source: `revoscalepy.rx_get_var_names(data_source)`

```python
airlinedata = rx_import(input_data = d_s)
airlinedata.head()
```

## Define and train models, score data

+ Logistic regression -- link
+ Linear regression
+ Predictions

```python
airlinedata = rx_import(input_data = d_s)
airlinedata.head()
```

## See Also

+ [Python function reference](../python-reference/introducing-python-package-reference.md)
+ [microsoftml function reference](../python-reference/microsoftml/microsoftml-package.md)
+ [revoscalepy function reference](../python-reference/revoscalepy/revoscalepy-package.md)
