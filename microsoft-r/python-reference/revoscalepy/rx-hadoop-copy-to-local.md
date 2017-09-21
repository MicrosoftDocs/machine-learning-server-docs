--- 
 
# required metadata 
title: "rx_hadoop_copy_to_local: Execute Hadoop Copy To Local Commands" 
description: "rx_hadoop_copy_to_local wraps the Hadoop fs -copyToLocal command." 
keywords: "Hadoop Command" 
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

# rx_hadoop_copy_to_local


 


## Usage



```
revoscalepy.rx_hadoop_copy_to_local(source: str, dest: str)
```





## Description

rx_hadoop_copy_to_local wraps the Hadoop fs -copyToLocal command.


## Arguments


### source

str. A character string specifying file(s) to be copied in HDFS


### dest

str. A character string specifying the destination of a copy in Local File System
If *source* includes more than one file, *dest* must be a directory.


## Example



```
from revoscalepy import rx_hadoop_copy_to_local
rx_hadoop_copy_to_local("/user/RevoShare/foo.txt", "/tmp/foo.txt")
```

