--- 
 
# required metadata 
title: "Class RxDataSource" 
description: "Base class for all RevoScalePy data sources." 
keywords: "datasource" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

## ``RxDataSource``


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
class revoscalepy.RxDataSource(column_info: dict = None)
```




### Description

Base class for all RevoScalePy data sources.


### Example



```
ds = RxDataSource(column_info={'DayOfWeek':{'type': 'factor', 'levels':["Monday","Tuesday","Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]}})
info = ds.extract_info()
```

