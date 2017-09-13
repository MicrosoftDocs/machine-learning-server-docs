--- 
 
# required metadata 
title: "rx_hadoop_file_exists: Execute Hadoop Files Exists Commands" 
description: "rx_hadoop_file_exists wraps the Hadoop fs -test -e command." 
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

# rx_hadoop_file_exists


 


## Usage



```
revoscalepy.rx_hadoop_file_exists(path: typing.Union[list, str])
```





## Description

rx_hadoop_file_exists wraps the Hadoop fs -test -e command.


## Arguments


### path

str or list. A list of paths or A character string specifying location of one or more files
or directories.


## Returns

a bool value specifying whether file exists.


## Example



```
from revoscalepy import rx_hadoop_file_exists
rx_hadoop_file_exists("/user/RevoShare/newUser/foo.txt")
```

