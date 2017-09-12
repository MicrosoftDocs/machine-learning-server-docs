--- 
 
# required metadata 
title: "rx_hadoop_copy_from_local: Execute Hadoop Copy From Local Commands" 
description: "rx_hadoop_copy_from_local wraps the Hadoop fs -copyFromLocal command." 
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

# rx_hadoop_copy_from_local


 


## Usage



```
revoscalepy.rx_hadoop_copy_from_local(source: str, dest: str)
```





## Description

rx_hadoop_copy_from_local wraps the Hadoop fs -copyFromLocal command.


## Arguments


### source

str. A character string specifying file(s) in Local File System


### dest

str. A character string specifying the destination of a copy in HDFS
If *source* includes more than one file, *dest* must be a directory.


## Example



```
from revoscalepy import rx_hadoop_copy_from_local
rx_hadoop_copy_from_local("/tmp/foo.txt", "/user/RevoShare/newUser")
```

