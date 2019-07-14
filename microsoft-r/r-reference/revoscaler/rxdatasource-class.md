--- 

# required metadata 
title: "RxDataSource-class class (revoAnalytics) | Microsoft Docs" 
description: "   Base class for all Microsoft R Services Compute Engine data sources.   " 
keywords: "(revoAnalytics), RxDataSource-class, names,RxDataSource-method, show,RxDataSource-method, classes" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 





 # RxDataSource-class: Class RxDataSource 
 ## Description

Base class for all Microsoft R Services Compute Engine data sources.  


 ## Objects from the Class 


A virtual class: No objects may be created from it.

 ## Generator 


The generator for classes that extend RxDataSource is
[rxNewDataSource](rxNew.md).  

 ## Methods 


The following methods are defined for classes that extend
RxDataSource:



### `names`
`signature(x = "RxDataSource")`: ... 


### `[rxOpen](rxOpen-methods.md)`
`signature(src = "RxDataSource")`: ... 


### `[rxClose](rxOpen-methods.md)`
`signature(src = "RxDataSource")`: ... 


### `[rxReadNext](rxOpen-methods.md)`
`signature(src = "RxDataSource")`: ... 


### `[rxWriteNext](rxOpen-methods.md)`
`signature(from = "data.frame", to = "RxDataSource", verbose = 0)`: ... 




 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[RxXdfData-class](RxXdfData-class.md),
[RxXdfData](RxXdfData.md),
[RxTextData](RxTextData.md),
[RxSasData](RxSasData.md),
[RxSpssData](RxSpssData.md),
[RxOdbcData](RxOdbcData.md),
[RxTeradata](RxTeradata.md),
[rxNewDataSource](rxNew.md),
[rxOpen](rxOpen-methods.md),
[rxReadNext](rxOpen-methods.md),
[rxWriteNext](rxOpen-methods.md).

 ## Examples

 ```

  fileName <- file.path(rxGetOption("sampleDataDir"), "claims.xdf")
  ds <- rxNewDataSource("RxXdfData", fileName)
  is(ds, "RxXdfData")
  # [1] TRUE
  is(ds, "RxDataSource")
  # [1] TRUE
```


