--- 
 
# required metadata 
title: "rx_hadoop_remove: Execute Hadoop Remove Commands" 
description: "Wraps the Hadoop fs -rm or fs -rm -skipTrash command." 
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

# rx_hadoop_remove


 


## Usage



```
revoscalepy.rx_hadoop_remove(path: typing.Union[list, str], skip_trash=False)
```





## Description

Wraps the Hadoop *fs -rm* or *fs -rm -skipTrash* command.


## Arguments


### path

Character string or list. A list of paths or A character string specifying location of one or more
files.

:param skip_trash. If True, removal bypasses the trash folder, if one has been set up.


## Example



```
from revoscalepy import rx_hadoop_remove
rx_hadoop_remove("/user/RevoShare/newUser/foo.txt", skip_trash = True)
```

