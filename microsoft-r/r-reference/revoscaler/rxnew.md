--- 
 
# required metadata 
title: "rxNewDataSource function (revoAnalytics) | Microsoft Docs" 
description: " This is the main generator for RxDataSource S4 classes. " 
keywords: "(revoAnalytics), rxNewDataSource, rxNewDataSource,character-method, file, connection" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 
 #rxNewDataSource: RevoScaleR Data Sources: Class Generator 
 ##Description
 
This is the main generator for RxDataSource S4 classes.
 
 
 ##Usage

```   
 ## S4 method for class `character':
rxNewDataSource  (name, ...)
 
```
 
 ##Arguments

   
    
 ### `name`
 character name of the specific class to instantiate. 
  
    
 ### ` ...`
 any other arguments are passed to the class generator `name`. 
  
 
 
 ##Details
 
This is a wrapper to specific class generator functions for the
RCE data source classes. For example, the RxXdfData class uses function
[RxXdfData](RxXdfData.md) as a generator. Therefore either `RxXdfData(...)`
or `rxNewDataSource("RxXdfData", ...)` will create an RxXdfData instance.
 
 
 ##Value
 
RxDataSource data source object. This object may be used to open and close the
actual RevoScaleR data sources.
 
 ## Side Effects 

 
This function creates the data source instance in the Microsoft R Services Compute Engine, but does not
actually open the data. The methods [rxOpen](rxOpen-methods.md) and
[rxClose](rxOpen-methods.md) will open and close the data.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxXdfData](RxXdfData.md),
[RxTextData](RxTextData.md),
[RxSasData](RxSasData.md),
[RxSpssData](RxSpssData.md),
[RxOdbcData](RxOdbcData.md),
[RxTeradata](RxTeradata.md),
[rxOpen](rxOpen-methods.md),
[rxReadNext](rxOpen-methods.md).
   
 ##Examples

 ```
   
  fileName <- file.path(rxGetOption("sampleDataDir"), "claims.xdf")
  ds <- rxNewDataSource("RxXdfData", fileName)
  rxOpen(ds)
  myData <- rxReadNext(ds)
  rxClose(ds)
 
```
 
 
 
