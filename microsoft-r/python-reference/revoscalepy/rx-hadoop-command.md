--- 
 
# required metadata 
title: "rx_hadoop_command: Execute Hadoop commands (revoscalepy)" 
description: "Executes arbitrary Hadoop commands and performs standard file operations in Hadoop." 
keywords: "Hadoop Command" 
author: "chuckheinzelman"
ms.author: "charlhe" 
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

# rx_hadoop_command


 


## Usage



```
revoscalepy.rx_hadoop_command(cmd: str) -> revoscalepy.functions.RxHadoopUtils.RxHadoopCommandResults
```





## Description

Executes arbitrary Hadoop commands and performs standard file operations in Hadoop.


## Arguments


### cmd

A character string containing a valid Hadoop command, that
is, the cmd portion of Hadoop Command. Embedded quotes are not permitted.


## Returns

Class RxHadoopCommandResults


## See also

[`rx_hadoop_make_dir`](rx-hadoop-make-dir.md)
[`rx_hadoop_copy_from_local`](rx-hadoop-copy-from-local.md)
[`rx_hadoop_remove_dir`](rx-hadoop-remove-dir.md)
[`rx_hadoop_file_exists`](rx-hadoop-file-exists.md)
[`rx_hadoop_list_files`](rx-hadoop-list-files.md)


## Example



```
from revoscalepy import rx_hadoop_command
rx_hadoop_command("version")
```

