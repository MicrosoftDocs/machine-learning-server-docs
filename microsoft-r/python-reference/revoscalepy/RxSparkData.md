--- 
 
# required metadata 
title: "RxSparkData: Base class generator for Spark data (revoscalepy)" 
description: "Base class for all revoscalepy Spark data sources, including RxHiveData, RxOrcData, RxParquetData and RxSparkDataFrame. It is intended to be called from those subclasses instead of directly." 
keywords: "" 
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
