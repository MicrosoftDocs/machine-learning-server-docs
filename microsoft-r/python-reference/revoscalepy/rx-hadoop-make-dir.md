--- 
 
# required metadata 
title: "rx_hadoop_make_dir: Execute Hadoop make directory commands (revoscalepy)" 
description: "Wraps the Hadoop fs -mkdir -p command." 
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

