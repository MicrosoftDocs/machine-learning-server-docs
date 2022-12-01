--- 
 
# required metadata 
title: "rx_spark_disconnect: Disconnect from remote Spark applications (revoscalepy)" 
description: "Shuts down the remote Spark application and switches to a local compute context. All rx* function calls after this will run in a local compute context. In pyspark-interop mode, if Spark application is started by pyspark APIs, rx_spark_disconnect will not shut down the remote Spark application but disassociate from it. Run ‘help(revoscalepy.rx_spark_connect)’ for more information about interop." 
keywords: "" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "sql-non-specified"
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
ms.technology: machine-learning-server
 
---

# rx_spark_disconnect


 


## Usage



```
revoscalepy.rx_spark_disconnect(compute_context=None)
```





## Description

Shuts down the remote Spark application and switches to a local compute context.
All rx* function calls after this will run in a local compute context.
In pyspark-interop mode, if Spark application is started by pyspark APIs,
rx_spark_disconnect will not shut down the remote Spark application but disassociate from it.
Run ‘help(revoscalepy.rx_spark_connect)’ for more information about interop.


## Arguments


### compute_context

Spark compute context to be terminated by rx_spark_disconnect.
If input is None, then current compute context will be used.


## Example



```
from revoscalepy import rx_spark_connect, rx_spark_disconnect
cc = rx_spark_connect()
rx_spark_disconnect(cc)
```

