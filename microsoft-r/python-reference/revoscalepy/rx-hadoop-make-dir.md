--- 
 
# required metadata 
title: "rx_hadoop_make_dir: Execute Hadoop make directory commands (revoscalepy)" 
description: "Wraps the Hadoop fs -mkdir -p command." 
keywords: "Hadoop Command" 
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

# rx_hadoop_make_dir


 


## Usage



```
revoscalepy.rx_hadoop_make_dir(path: typing.Union[list, str])
```





## Description

Wraps the Hadoop *fs -mkdir -p* command.


## Arguments


### path

Character string or list. A list of paths or A character string specifying location of one or more
directories.


## Example



```
from revoscalepy import rx_hadoop_make_dir
rx_hadoop_make_dir("/user/RevoShare/newUser")
```

