--- 
 
# required metadata 
title: "Class RxDataSource" 
description: "   Base class for all Microsoft R Services Compute Engine data sources.   " 
keywords: "RevoScaleR, RxDataSource-class, names,RxDataSource-method, show,RxDataSource-method, classes" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 
 
 
 #RxDataSource-class: Class RxDataSource

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Base class for all Microsoft R Services Compute Engine data sources.  
 
 
 ## Objects from the Class 

 
A virtual class: No objects may be created from it.
 
 ## Generator 

 
The generator for classes that extend RxDataSource is
[rxNewDataSource](rxnew.md).  
 
 ## Methods 

 
The following methods are defined for classes that extend
RxDataSource:



###names
`signature(x = "RxDataSource")`: ... 


###[rxOpen](rxopen-methods.md)
`signature(src = "RxDataSource")`: ... 


###[rxClose](rxopen-methods.md)
`signature(src = "RxDataSource")`: ... 


###[rxReadNext](rxopen-methods.md)
`signature(src = "RxDataSource")`: ... 


###[rxWriteNext](rxopen-methods.md)
`signature(from = "data.frame", to = "RxDataSource", verbose = 0)`: ... 



 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxXdfData-class](rxxdfdata-class.md),
[RxXdfData](rxxdfdata.md),
[RxTextData](rxtextdata.md),
[RxSasData](rxsasdata.md),
[RxSpssData](rxspssdata.md),
[RxOdbcData](rxodbcdata.md),
[RxTeradata](rxteradata.md),
[rxNewDataSource](rxnew.md),
[rxOpen](rxopen-methods.md),
[rxReadNext](rxopen-methods.md),
[rxWriteNext](rxopen-methods.md).
   
 ##Examples

 ```
   
  fileName <- file.path(rxGetOption("sampleDataDir"), "claims.xdf")
  ds <- rxNewDataSource("RxXdfData", fileName)
  is(ds, "RxXdfData")
  # [1] TRUE
  is(ds, "RxDataSource")
  # [1] TRUE
 
```
 
 
