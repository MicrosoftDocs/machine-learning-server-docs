--- 
 
# required metadata 
title: "RevoScaleR Data Sources: Class Generator" 
description: " This is the main generator for RxDataSource S4 classes. " 
keywords: "RevoScaleR, rxNewDataSource, rxNewDataSource,character-method, file, connection" 
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
 
 
 
 #`rxNewDataSource`: RevoScaleR Data Sources: Class Generator

 Applies to version 9.1.0 of package RevoScaleR.
 
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
[RxXdfData](rxxdfdata.md) as a generator. Therefore either `RxXdfData(...)`
or `rxNewDataSource("RxXdfData", ...)` will create an RxXdfData instance.
 
 
 ##Value
 
RxDataSource data source object. This object may be used to open and close the
actual RevoScaleR data sources.
 
 ## Side Effects 

 
This function creates the data source instance in the Microsoft R Services Compute Engine, but does not
actually open the data. The methods [rxOpen](rxopen-methods.md) and
[rxClose](rxopen-methods.md) will open and close the data.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxXdfData](rxxdfdata.md),
[RxTextData](rxtextdata.md),
[RxSasData](rxsasdata.md),
[RxSpssData](rxspssdata.md),
[RxOdbcData](rxodbcdata.md),
[RxTeradata](rxteradata.md),
[rxOpen](rxopen-methods.md),
[rxReadNext](rxopen-methods.md).
   
 ##Examples

 ```
   
  fileName <- file.path(rxGetOption("sampleDataDir"), "claims.xdf")
  ds <- rxNewDataSource("RxXdfData", fileName)
  rxOpen(ds)
  myData <- rxReadNext(ds)
  rxClose(ds)
 
```
 
 
 
