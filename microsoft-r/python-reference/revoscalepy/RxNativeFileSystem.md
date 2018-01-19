--- 
 
# required metadata 
title: "RxNativeFileSystem: revoscalepy Native File System Generator" 
description: "Main generator class for RxNativeFileSystem" 
keywords: "filesystem native" 
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

# RxNativeFileSystem


 



```
revoscalepy.RxNativeFileSystem
```





## Description

Main generator class for RxNativeFileSystem


## Returns

An RxNativeFileSystem file system object. This object may be used in [`RxOptions`](RxOptions.md), [`RxTextData`](RxTextData.md), or [`RxXdfData`](RxXdfData.md) to set the file system.


## See also

[`RxOptions`](RxOptions.md)
[`RxTextData`](RxTextData.md)
[`RxXdfData`](RxXdfData.md)
[`RxFileSystem`](RxFileSystem.md)


## Example



```
from revoscalepy import RxNativeFileSystem
fs = RxNativeFileSystem()
print(fs.file_system_type())
```

