--- 
 
# required metadata 
title: "RxDataSource function (RevoScaleR) " 
description: " A generator for RxDataSource S4 classes. " 
keywords: "(RevoScaleR), RxDataSource, file, connection" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 #RxDataSource: RevoScaleR Data Source: Class Generator 
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
 
 
 
