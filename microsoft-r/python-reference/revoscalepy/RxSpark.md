--- 
 
# required metadata 
title: "RxSpark: Generate Spark Compute Context" 
description: "Creates the compute context for running revoscalepy analysis on Spark. Note that the use of rx_spark_connect() is recommended over RxSpark() as rx_spark_connect() supports persistent mode with in-memory caching. Run help(revoscalepy.rx_spark_connect) for more information." 
keywords: "" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/11/2017" 
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

# RxSpark


 



```
revoscalepy.RxSpark(hdfs_share_dir: str = '/user/RevoShare\70M2QL21OXX8HRP$',
    share_dir: str = '/var/RevoShare\70M2QL21OXX8HRP$',
    user: str = '70M2QL21OXX8HRP$', name_node: str = None,
    port: int = None, auto_cleanup: bool = True,
    console_output: bool = False, packages_to_load: list = None,
    idle_timeout: int = 3600, num_executors: int = None,
    executor_cores: int = None, executor_mem: str = None,
    driver_mem: str = None, executor_overhead_mem: str = None,
    extra_spark_config: str = '', spark_reduce_method: str = 'auto',
    tmp_fs_work_dir: str = '/dev/shm', persistent_run: bool = False,
    wait: bool = True, **kwargs)
```





## Description

Creates the compute context for running revoscalepy analysis on Spark.
Note that the use of rx_spark_connect() is recommended over RxSpark()
as rx_spark_connect() supports persistent mode with in-memory caching.
Run help(revoscalepy.rx_spark_connect) for more information.
