--- 
 
# required metadata 
title: "RxSparkData: Base class generator for Spark data (revoscalepy)" 
description: "Base class for all revoscalepy Spark data sources, including RxHiveData, RxOrcData, RxParquetData and RxSparkDataFrame. It is intended to be called from those subclasses instead of directly." 
keywords: "" 
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

# RxSparkData


 



```
revoscalepy.RxSparkData(column_info: dict = None,
    df_source: str = None, df_type: str = None,
    write_factors_as_indexes: bool = False, **kwargs)
```





## Description

Base class for all revoscalepy Spark data sources,
including RxHiveData, RxOrcData, RxParquetData and RxSparkDataFrame.
It is intended to be called from those subclasses
instead of directly.
