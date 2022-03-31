--- 
 
# required metadata 
title: "rx_hadoop_copy_from_local: Execute Hadoop copyFromLocal commands (revoscalepy)" 
description: "Wraps the Hadoop fs -copyFromLocal command." 
keywords: "Hadoop Command" 
author: "DaniBunny"
ms.author: "dacoelho" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
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
 
---

# rx_hadoop_copy_from_local


 


## Usage



```
revoscalepy.rx_hadoop_copy_from_local(source: str, dest: str)
```





## Description

Wraps the Hadoop *fs -copyFromLocal* command.


## Arguments


### source

A character string specifying file(s) in Local File System


### dest

A character string specifying the destination of a copy in HDFS
If *source* includes more than one file, *dest* must be a directory.


## Example



```
from revoscalepy import rx_hadoop_copy_from_local
rx_hadoop_copy_from_local("/tmp/foo.txt", "/user/RevoShare/newUser")
```

