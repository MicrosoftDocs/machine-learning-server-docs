--- 
 
# required metadata 
title: "RxNativeFileSystem: Class generator for the native file system (revoscalepy)" 
description: "Main generator class for objects repreresenting the native file system." 
keywords: "filesystem native" 
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

# RxNativeFileSystem


 



```
revoscalepy.RxNativeFileSystem
```





## Description

Main generator class for objects representing the native file system.


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

