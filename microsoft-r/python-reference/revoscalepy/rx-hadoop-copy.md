--- 
 
# required metadata 
title: "rx_hadoop_copy: Execute Hadoop copy commands (revoscalepy)" 
description: "Wraps the Hadoop fs -cp command." 
keywords: "Hadoop Command" 
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

# rx_hadoop_copy


 


## Usage



```
revoscalepy.rx_hadoop_copy(source: str, dest: str)
```





## Description

Wraps the Hadoop *fs -cp* command.


## Arguments


### source

A character string specifying file(s) to be copied in HDFS


### dest

A character string specifying the destination of a copy in HDFS
If *source* includes more than one file, *dest* must be a directory.


## Example



```
from revoscalepy import rx_hadoop_copy
rx_hadoop_copy("/user/RevoShare/newUser/foo.txt","/user/RevoShare/newUser/bar.txt")
```

