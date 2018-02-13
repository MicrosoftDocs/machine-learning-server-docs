--- 
 
# required metadata 
title: "rx_spark_connect: Connect to remote Spark applications (revoscalepy)" 
description: "Creates the compute context object with RxSpark and then immediately starts the remote Spark application." 
keywords: "" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# rx_spark_connect


 


## Usage



```
revoscalepy.rx_spark_connect(hdfs_share_dir: str = '/user/RevoShare\\766RR78ROCWFDMK$',
    share_dir: str = '/var/RevoShare\\766RR78ROCWFDMK$',
    user: str = '766RR78ROCWFDMK$', name_node: str = None,
    master: str = 'yarn', port: int = None,
    auto_cleanup: bool = True, console_output: bool = False,
    packages_to_load: list = None, idle_timeout: int = None,
    num_executors: int = None, executor_cores: int = None,
    executor_mem: str = None, driver_mem: str = None,
    executor_overhead_mem: str = None, extra_spark_config: str = '',
    spark_reduce_method: str = 'auto', tmp_fs_work_dir: str = None,
    interop: typing.Union[list, str] = None,
    reset: bool = False) -> revoscalepy.computecontext.RxSpark.RxSpark
```





## Description

Creates the compute context object with RxSpark and then immediately starts the remote Spark application.


## Arguments


### hdfs_share_dir

Character string specifying the file sharing location within HDFS.
You must have permissions to read and write to this location. When you are running in Spark local mode,
this parameter will be ignored and it will be forced to be equal to the parameter share_dir.


### share_dir

Character string specifying the directory on the master (perhaps edge) node that is
shared among all the nodes of the cluster and any client host. You must have permissions to read and write
in this directory.


### user

Character string specifying the username for submitting job to the
Spark cluster. Defaults to the username of the user running the python (that is, the value of getpass.getuser()).


### name_node

Character string specifying the Spark name node hostname or IP address.
Typically you can leave this at its default value.
If set to a value other than “default” or the empty string (see below),
this must be an address that can be resolved by the data nodes and used by them to contact the
name node. Depending on your cluster, it may need to be set to a private network address
such as “master.local”. If set to the empty string, “”, then the master process will set
this to the name of the node on which it is running, as returned by platform.node().
If you are running in Spark local mode, this parameter defaults to “[file:///](file:///.md)”; otherwise it
defaults to RxOptions.get_option(“hdfsHost”).


### master

Character string specifying the master URL passed to Spark.
The value of this parameter could be “yarn”, “standalone” and “local”.
The formats of Spark’s master URL (except for mesos) specified in this website [https://spark.apache.org/docs/latest/submitting-applications.html](https://spark.apache.org/docs/latest/submitting-applications.html) are also supported.


### port

Integer value specifying the port used by the name node for HDFS. Needs
to be able to be cast to an integer. Defaults to RxOptions.get_option(“hdfsPort”).


### auto_cleanup

Bool value, if True, the default behavior is to clean up the
temporary computational artifacts and delete the result objects upon retrieval.  If False,
then the computational results are not deleted. Leaving this
flag set to False can result in accumulation of compute artifacts which you may
eventually need to delete before they fill up your hard drive.


### console_output

Bool value, if True, causes the standard output
of the python processes to be printed to the user console.


### packages_to_load

Optional character vector specifying additional modules to be
loaded on the nodes when jobs are run in this compute context.


### idle_timeout

Integer value, specifying the number of seconds of being idle
(i.e., not running any Spark job) before system kills the Spark process.
Setting a value greater than 600 is recommended. Defaults to 3600.
Setting a negative value will not timeout. pyspark interoperation will have no timeout.


### num_executors

Integer value, specifying the number of executors in Spark, which is
equivalent to parameter –num-executors in spark-submit app. If not specified,
the default behavior is to launch as many executors as possible,
which may use up all resources and prevent other users from sharing the cluster.
Defaults to RxOptions.get_option(“spark.numExecutors”).


### executor_cores

Integer value, specifying the number of cores per executor, which is
equivalent to parameter –executor-cores in spark-submit app.
Defaults to RxOptions.get_option(“spark.executorCores”).


### executor_mem

Character string specifying memory per executor (e.g., 1000M, 2G), which is
equivalent to parameter –executor-memory in spark-submit app.
Defaults to RxOptions.get_option(“spark.executorMem”).


### driver_mem

Character string specifying memory for driver (e.g., 1000M, 2G), which is
equivalent to parameter –driver-memory in spark-submit app.
Defaults to RxOptions.get_option(“spark.driverMem”).


### executor_overhead_mem

Character string specifying memory overhead per executor (e.g. 1000M, 2G), which is
equivalent to setting spark.yarn.executor.memoryOverhead in YARN.
Increasing this value will allocate more memory for the python process and the ScaleR engine process
in the YARN executors, so it may help resolve job failure or executor lost issues.
Defaults to RxOptions.get_option(“spark.executorOverheadMem”).


### extra_spark_config

Character string specifying extra arbitrary Spark properties
(e.g., “–conf spark.speculation=true –conf spark.yarn.queue=aQueue”),
which is equivalent to additional parameters passed into spark-submit app.


### spark_reduce_method

Spark job collects all parallel tasks’ results to reduce as final result.
This parameter decides reduce strategy: oneStage/twoStage/auto.
oneStage: reduce parallel tasks to one result with one reduce function;
twoStage: reduce parallel tasks to square root size with the first reduce function,
then reduce to final result with the second reduce function;
auto: spark will smartly select oneStage or twoStage.


### tmp_fs_work_dir

Character string specifying the temporary working directory in worker nodes.
It defaults to /dev/shm to utilize in-memory temporary file system for performance gain,
when the size of /dev/shm is less than 1G, the default value would switch to /tmp for guarantee sufficiency.
You can specify a different location if the size of /dev/shm is insufficient.
Please make sure that YARN run-as user has permission to read and write to this location


### interop

None or string or list of strings. Current supported interoperation values are,
‘pyspark’: active revoscalepy Spark compute context in existing pyspark application to support the usage of both

    pyspark and revoscalepy functionalities. For jobs running in AzureML, interop value ‘pyspark’ must be used.


### reset

Bool value, if True, all cached Spark Data Frames will be freed and all existing Spark applications
that belong to the current user will be shut down.


## Example



```
#############################################################################
from revoscalepy import rx_spark_connect, rx_spark_disconnect
cc = rx_spark_connect()
rx_spark_disconnect(cc)

#############################################################################
####### pyspark interoperation example ####################
from pyspark.sql import SparkSession
from pyspark import SparkContext
from revoscalepy import rx_spark_connect, rx_get_pyspark_connection, RxSparkDataFrame, rx_summary, rx_data_step
spark = SparkSession.builder.master("yarn").getOrCreate()
cc = rx_spark_connect(interop = "pyspark")

# sc is equivalent to result of pyspark API call SparkContext.getOrCreate()
sc = rx_get_pyspark_connection(cc)

####### algorithms with input from spark data frame #######
df = spark.createDataFrame([('Monday',3012),('Friday',1354),('Sunday',5452)], ["DayOfWeek", "Sale"])
input_df = RxSparkDataFrame(df)
rx_summary("~.", input_df)

####### ETL with output to spark data frame ###############
output_df = RxSparkDataFrame()
rx_data_step(input_df, output_df, vars_to_drop = ['DayOfWeek'])
odf = output_df.spark_data_frame
odf.take(2)
# [Row(Sale=3012), Row(Sale=1354)]
```

