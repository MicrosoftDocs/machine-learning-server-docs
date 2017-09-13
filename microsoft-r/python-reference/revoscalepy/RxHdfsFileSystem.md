--- 
 
# required metadata 
title: "RxHdfsFileSystem: revoscalepy HDFS File System Generator" 
description: "Main generator class for RxHdfsFileSystem" 
keywords: "filesystem hdfs" 
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

# RxHdfsFileSystem


 



```
revoscalepy.RxHdfsFileSystem(host_name=None, port=None)
```





## Description

Main generator class for RxHdfsFileSystem


## Returns

An RxHdfsFileSystem file system object.
This object may be used in [`RxOptions`](RxOptions.md),
[`RxTextData`](RxTextData.md),
[`RxXdfData`](RxXdfData.md),
[`RxParquetData`](RxParquetData.md),
or [`RxOrcData`](RxOrcData.md) to set the file system.


## See also

[`RxOptions`](RxOptions.md)
[`RxTextData`](RxTextData.md)
[`RxXdfData`](RxXdfData.md)
[`RxParquet`](RxParquetData.md)
[`RxOrcData`](RxOrcData.md)
[`RxFileSystem`](RxFileSystem.md)


## Example



```
from revoscalepy import RxHdfsFileSystem
fs = RxHdfsFileSystem()
print(fs.file_system_type())
from revoscalepy import RxXdfData
xdf_data = RxXdfData("/tmp/hdfs_file", file_system = fs)
```

