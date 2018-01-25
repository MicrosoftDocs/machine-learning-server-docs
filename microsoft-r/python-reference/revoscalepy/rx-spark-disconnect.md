--- 
 
# required metadata 
title: "rx_spark_disconnect: Disconnect revoscalepy from remote Spark application." 
description: "Shuts down the remote Spark application and switches to a local compute context. All rx* function calls after this will run in a local compute context. In pyspark-interop mode, if Spark application is started by pyspark APIs, rx_spark_disconnect will not shut down the remote Spark application but disassociate from it. Run ‘help(revoscalepy.rx_spark_connect)’ for more information about interop." 
keywords: "" 
author: "heidist" 
manager: "cgronlun" 
ms.date: "01/19/2018" 
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

