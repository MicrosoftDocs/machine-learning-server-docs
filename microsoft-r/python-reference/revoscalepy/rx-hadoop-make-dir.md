--- 
 
# required metadata 
title: "rx_hadoop_make_dir: Execute Hadoop Make Directory Commands" 
description: "rx_hadoop_make_dir wraps the Hadoop fs -mkdir -p command." 
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

# rx_hadoop_make_dir


 


## Usage



```
revoscalepy.rx_hadoop_make_dir(path: typing.Union[list, str])
```





## Description

rx_hadoop_make_dir wraps the Hadoop fs -mkdir -p command.


## Arguments


### path

str or list. A list of paths or A character string specifying location of one or more
directories.


## Example



```
from revoscalepy import rx_hadoop_make_dir
rx_hadoop_make_dir("/user/RevoShare/newUser")
```

