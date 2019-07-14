--- 
 
# required metadata 
title: "rx_hadoop_file_exists: Execute Hadoop files exists commands (revoscalepy)" 
description: "Wraps the Hadoop fs -test -e command." 
keywords: "Hadoop Command" 
author: "HeidiSteen" 
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

# rx_hadoop_file_exists


 


## Usage



```
revoscalepy.rx_hadoop_file_exists(path: typing.Union[list, str])
```





## Description

Wraps the Hadoop *fs -test -e* command.


## Arguments


### path

Character string or list. A list of paths or A character string specifying location of one or more files
or directories.


## Returns

A bool value specifying whether file exists.


## Example



```
from revoscalepy import rx_hadoop_file_exists
rx_hadoop_file_exists("/user/RevoShare/newUser/foo.txt")
```

