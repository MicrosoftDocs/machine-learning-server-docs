--- 
 
# required metadata 
title: "rx_hadoop_list_files: Execute Hadoop list files commands (revoscalepy)" 
description: "Wraps the Hadoop fs -ls or -lsr command." 
keywords: "Hadoop Command" 
author: "dphansen" 
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

# rx_hadoop_list_files


 


## Usage



```
revoscalepy.rx_hadoop_list_files(path: typing.Union[list, str],
    recursive=False)
```





## Description

Wraps the Hadoop *fs -ls* or *-lsr* command.


## Arguments


### path

Character string or list. A list of paths or A character string specifying location of one or more files
or directories.


### recursive

Bool value. If True, directory listings are recursive.


## Example



```
from revoscalepy import rx_hadoop_list_files
rx_hadoop_list_files("/user/RevoShare/newUser")
```

