--- 
 
# required metadata 
title: "RevoScaleR Data Source: Class Generator" 
description: " A generator for RxDataSource S4 classes. " 
keywords: "RevoScaleR, RxDataSource, file, connection" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #`RxDataSource`: RevoScaleR Data Source: Class Generator

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
A generator for RxDataSource S4 classes.
 
 
 ##Usage

```   
  	RxDataSource( class = NULL, dataSource = NULL,  ...)
 
```
 
 ##Arguments

   
    
 ### `class`
 an optional character string specifying class name of the data source to be created, such as [RxTextData](RxTextData.md), [RxSpssData](RxSpssData.md), [RxSasData](RxSasData.md), [RxOdbcData](RxOdbcData.md), or [RxTeradata](RxTeradata.md). If the class of the input dataSource differs from `class`, contents of overlapping slots will be copied from the input data source to the  newly constructed data source. 
  
    
 ### `dataSource`
 an optional data source object to be cloned. 
  
    
 ### ` ...`
 any other arguments to be applied to the newly constructed data source. 
  
 
 
 ##Details
 
If `class` is specified, the returned object will be of that class.
If not, the returned object will have the class of `dataSource`.
If both `class` and `dataSource` are specified, the contents of
matching slots will be copied from the input `dataSource` to the 
output object. Additional arguments will then be applied.
 
 
 ##Value
 
A type of RxDataSource data source object.
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxDataSource-class](RxDataSource-class.md),
[RxTextData](RxTextData.md),
[RxSqlServerData](RxSqlServerData.md),
[RxSpssData](RxSpssData.md),
[RxSasData](RxSasData.md),
[RxOdbcData](RxOdbcData.md),
[RxTeradata](RxTeradata.md),
[RxXdfData](RxXdfData.md).
   
 ##Examples

 ```
   
  
  	claimsSpssName <- file.path(rxGetOption("sampleDataDir"), "claims.sav")
  	claimsTextName <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
  	claimsColInfo <- list( 
  		age = list(type = "factor", 
  			levels = c("17-20", "21-24", "25-29", "30-34", "35-39", "40-49", "50-59", "60+")), 
  			car.age = list(type = "factor", levels = c("0-3", "4-7", "8-9", "10+")), 
  			type = list(type = "factor", levels = c("A", "B", "C", "D")) 
  		) 
  	textDS <- RxTextData(file = claimsTextName, colInfo = claimsColInfo)
  	
  	# Create an SPSS data source from the text data source
  	# The 'colInfo' will be carried over to the SPSS data source
  	# and the file name will be replaced
  	spssDS <- RxDataSource(class = "RxSpssData", dataSource = textDS, file = claimsSpssName)
  
 
```
 
 
 
