--- 
 
# required metadata 
title: "rx_hadoop_move: Execute Hadoop move commands (revoscalepy)" 
description: "Wraps the Hadoop fs -mv command." 
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

# rx_hadoop_move


 


## Usage



```
revoscalepy.rx_hadoop_move(source: str, dest: str)
```





## Description

Wraps the Hadoop *fs -mv* command.


## Arguments


### source

A character string specifying file(s) to be moved in HDFS


### dest

A character string specifying the destination of move in HDFS
If *source* includes more than one file, *dest* must be a directory.


## Example



```
from revoscalepy import rx_hadoop_move
rx_hadoop_move("/user/RevoShare/newUser/foo.txt","/user/RevoShare/newUser/bar.txt")
```

