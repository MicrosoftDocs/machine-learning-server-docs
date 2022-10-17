--- 

# required metadata 
title: "RxXdfData-class class (revoAnalytics) | Microsoft Docs" 
description: " Xdf data source connection class. " 
keywords: "(revoAnalytics), RxXdfData-class, colnames,RxXdfData-method, dim,RxXdfData-method, dimnames,RxXdfData-method, formula,RxXdfData-method, length,RxXdfData-method, names,RxXdfData-method, names<-,RxXdfData-method, row.names,RxXdfData-method, show,RxXdfData-method, str,RxXdfData-method, classes" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "sql-non-specified"
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
ms.technology: machine-learning-server

--- 














 # RxXdfData-class: Class RxXdfData 
 ## Description

Xdf data source connection class.


 ## Generators 


The targeted generator [RxXdfData](RxXdfData.md) as well as the general generator
[rxNewDataSource](rxNew.md).

 ## Extends 


Class RxFileData, directly.
Class RxDataSource, by class RxFileData.


 ## Methods 




### `colnames`
`signature(x = "RxXdfData")`: ... 


### `dim`
`signature(x = "RxXdfData")`: ... 


### `dimnames`
`signature(x = "RxXdfData")`: ... 


### `formula`
`signature(x = "RxXdfData")`: ... 


### `length`
`signature(x = "RxXdfData")`: ... 


### `names`
`signature(x = "RxXdfData")`: ... 


### `names<-`
`signature(x = "RxXdfData")`: ... 


### `row.names`
`signature(x = "RxXdfData")`: ... 


### `show`
`signature(object = "RxXdfData")`: ... 


### `str`
`signature(object = "RxXdfData")`: ... 





 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[RxDataSource-class](RxDataSource-class.md),
[RxXdfData](RxXdfData.md),
[rxNewDataSource](rxNew.md)


 ## Examples

 ```

  DS <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "fourthgraders.xdf"))
  head(DS)
  tail(DS)
  names(DS)
  dim(DS)
  dimnames(DS)
  nrow(DS)
  ncol(DS)
  str(DS)

  # formula examples
  formula(DS)
  formula(DS, varsToDrop = "male")
  formula(DS, depVar = "height")
  formula(DS, depVar = "height", inter = list(c("male", "eyecolor")))

  # summarize variables in data source
  summary(DS)

  # renaming variables in .xdf file via replacement method
  XDF <- file.path(tempdir(), "iris.xdf")
  rxDataStep(iris, XDF, overwrite = TRUE)
  irisDS <- RxXdfData(XDF)
  names(irisDS)
  names(irisDS) <- c("cow","horse","chicken","goat","squirrel")
  names(irisDS)
  if (file.exists(XDF)) file.remove(XDF)
```



