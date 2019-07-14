--- 
 
# required metadata 
title: "rx_hadoop_remove_dir: Execute Hadoop remove directory commands (revoscalepy)" 
description: "Wraps the Hadoop fs -rm -r  or fs -rm -r -skipTrash command." 
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

# rx_hadoop_remove_dir


 


## Usage



```
revoscalepy.rx_hadoop_remove_dir(path: typing.Union[list, str],
    skip_trash=False)
```





## Description

Wraps the Hadoop *fs -rm -r*  or *fs -rm -r -skipTrash* command.


## Arguments


### path

Character string or list. A list of paths or A character string specifying location of one or more
directories.

:param skip_trash. If True, removal bypasses the trash folder, if one has been set up.


## Example



```
from revoscalepy import rx_hadoop_remove_dir
rx_hadoop_remove_dir("/user/RevoShare/newUser", skip_trash = True)
```

