--- 
 
# required metadata 
title: "rx_hadoop_list_files: Execute Hadoop List Files Commands" 
description: "Wraps the Hadoop fs -ls or -lsr command." 
keywords: "Hadoop Command" 
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

