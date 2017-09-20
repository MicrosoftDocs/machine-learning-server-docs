--- 
 
# required metadata 
title: "rx_hadoop_copy: Execute Hadoop Copy Commands" 
description: "rx_hadoop_copy wraps the Hadoop fs -cp command." 
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

# rx_hadoop_copy


 


## Usage



```
revoscalepy.rx_hadoop_copy(source: str, dest: str)
```





## Description

rx_hadoop_copy wraps the Hadoop fs -cp command.


## Arguments


### source

str. A character string specifying file(s) to be copied in HDFS


### dest

str. A character string specifying the destination of a copy in HDFS
If *source* includes more than one file, *dest* must be a directory.


## Example



```
from revoscalepy import rx_hadoop_copy
rx_hadoop_copy("/user/RevoShare/newUser/foo.txt","/user/RevoShare/newUser/bar.txt")
```

