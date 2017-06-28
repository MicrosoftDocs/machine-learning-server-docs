--- 
 
# required metadata 
title: "Class RxDataSource" 
description: "   Base class for all Microsoft R Services Compute Engine data sources.   " 
keywords: "RevoScaleR, RxDataSource-class, names,RxDataSource-method, show,RxDataSource-method, classes" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 
 #`RxDataSource-class`: Class RxDataSource

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Base class for all Microsoft R Services Compute Engine data sources.  
 
 
 ## Objects from the Class 

 
A virtual class: No objects may be created from it.
 
 ## Generator 

 
The generator for classes that extend RxDataSource is
[rxNewDataSource](../../scaler/packagehelp/rxnew.md).  
 
 ## Methods 

 
The following methods are defined for classes that extend
RxDataSource:



###`names`
`signature(x = "RxDataSource")`: ... 


###`[rxOpen](../../scaler/packagehelp/rxopen-methods.md)`
`signature(src = "RxDataSource")`: ... 


###`[rxClose](../../scaler/packagehelp/rxopen-methods.md)`
`signature(src = "RxDataSource")`: ... 


###`[rxReadNext](../../scaler/packagehelp/rxopen-methods.md)`
`signature(src = "RxDataSource")`: ... 


###`[rxWriteNext](../../scaler/packagehelp/rxopen-methods.md)`
`signature(from = "data.frame", to = "RxDataSource", verbose = 0)`: ... 



 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxXdfData-class](../../scaler/packagehelp/rxxdfdata-class.md),
[RxXdfData](../../scaler/packagehelp/rxxdfdata.md),
[RxTextData](../../scaler/packagehelp/rxtextdata.md),
[RxSasData](../../scaler/packagehelp/rxsasdata.md),
[RxSpssData](../../scaler/packagehelp/rxspssdata.md),
[RxOdbcData](../../scaler/packagehelp/rxodbcdata.md),
[RxTeradata](../../scaler/packagehelp/rxteradata.md),
[rxNewDataSource](../../scaler/packagehelp/rxnew.md),
[rxOpen](../../scaler/packagehelp/rxopen-methods.md),
[rxReadNext](../../scaler/packagehelp/rxopen-methods.md),
[rxWriteNext](../../scaler/packagehelp/rxopen-methods.md).
   
 ##Examples

 ```
   
  fileName <- file.path(rxGetOption("sampleDataDir"), "claims.xdf")
  ds <- rxNewDataSource("RxXdfData", fileName)
  is(ds, "RxXdfData")
  # [1] TRUE
  is(ds, "RxDataSource")
  # [1] TRUE
 
```
 
 
