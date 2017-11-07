---

# required metadata
title: "How to use revoscalepy with Apache Spark - Machine Learning Server   "
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

This article introduces Python functions in a [revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md) package with Apache Spark (Spark) running on a Hadoop cluster. Within a Spark cluster, Machine Learning Server leverages these components:

+ Hadoop distributed file system for finding and accessing data.
+ Yarn for job scheduling and management.
+ Spark as the processing framework (versions 2.0-2.1).

The revoscalepy library provides cluster-aware Python functions for data management, predictive analytics, and visualization. When you set the [compute context](../r/concept-what-is-compute-context.md) to [rx-spark-connect](../python-reference/revoscalepy/rx-spark-connect.md), revoscalepy f automatically distributes the workload across all the data nodes. There is no overhead in managing jobs or the queue, or tracking the physical location of data in HDFS; Spark does both for you.

> [!Note]
> For installation instructions, see [Install Machine Learning Server for Hadoop](../install/machine-learning-server-hadoop-install.md).

## Start Python

On your cluster's edge node, start a session by typing **mlserver-python** at the command line. 

**Local compute context on Spark**

By default, the local compute context is the implicit computing environment. All mlserver-python code runs here until you specify a remote compute context.

**Remote compute context on Spark**

From the edge node, you can push computations to the data layer by creating a remote Spark compute context. In this context, execution is on all data nodes. 

The following example shows how to set a remote compute context to clustered data nodes, execute functions in the Spark compute context, switch back to a local compute context, and disconnect from the server.

```Python
# Load the functions
from revoscalepy import RxOrcData, rx_spark_connect, rx_spark_list_data, rx_lin_mod, rx_spark_cache_data

# Create a remote compute contenxt 
cc = rx_spark_connect()

# Create a col_info object specfiying the factors
col_info = {"DayOfWeek": {"type": "factor"}}

# Load data, factored and cached.
df = RxOrcData(file = "/share/sample_data/AirlineDemoSmallOrc", column_info = col_info)
df = rx_spark_cache_data(df, True)

# After the first run, a Spark data object is added into the list
rx_lin_mod("ArrDelay ~ DayOfWeek", data = df)
rx_spark_list_data(True)

# Disconnect. Switches back to a local compute context.
rx_spark_disconnect(cc)
rx_get_compute_context()
```

## Specify a data source and location

As part of execution in Spark, your data source must be a file format that Spark understands, such as text, Hive, Orc, and Parquet. You can also create and consume [.xdf files](../r/concept-what-is-xdf.md), a data file format native to Machine Learning Server that you can read or write to from both Python and R script.

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

Data is automatically loaded into a data frame even without rx_import, but you can load it explicitly using the rx_import, which is useful if you want to include parameters. 

In mlserver-python, you can use head and tail functions, similar to R, to return the first or last part of the data set.

```python
airlinedata = rx_import(input_data = d_s, outFile="/tmp/airExample.xdf")
airlinedata.head()
```

## Summarize data

To quickly understand fundamental characteristics of your data, use the **rx_summary** function to return basic statistical descriptors. Mean, standard deviation, and min-max values. A count of total observations, missing observations, and valid observations is included.

A minimum specification of the [rx_summmary](../python-reference/revoscalepy/rx-summary.md) function consists of a valid data source object and a formula giving the fields to summarize. The formula is symbolic, providing variables used in the model. and typically does not contain a response variable. It should be of the form of ~ terms. 

> [!Tip]
> Get the term list from a data source to see what is available: `revoscalepy.rx_get_var_names(data_source)`

```python
import os
from revoscalepy import rx_summary, RxOptions, RxXdfData
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
summary = rx_summary("ArrDelay+DayOfWeek", ds)
print(summary)
```

## Create models

The following example produces a linear regression, followed by predicted values for the linear regression model.

```python
# Linear regression
import os
import tempfile
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod

sample_data_path = RxOptions.get_option("sampleDataDir")
in_mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", in_mort_ds)
print(lin_mod)
```

```python
# Add predicted values
import os
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod, rx_predict, rx_data_step

sample_data_path = RxOptions.get_option("sampleDataDir")
mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))
mort_df = rx_data_step(mort_ds)

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", mort_df)
pred = rx_predict(lin_mod, data = mort_df)
print(pred.head())
```

## See Also

+ [Python function reference](../python-reference/introducing-python-package-reference.md)
+ [microsoftml function reference](../python-reference/microsoftml/microsoftml-package.md)
+ [revoscalepy function reference](../python-reference/revoscalepy/revoscalepy-package.md)
