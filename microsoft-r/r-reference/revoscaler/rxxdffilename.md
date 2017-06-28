--- 
 
# required metadata 
title: "Retrieve .xdf file name" 
description: " Get the .xdf file path from a character string or RxXdfData object. " 
keywords: "RevoScaleR, rxXdfFileName-methods, rxXdfFileName, rxXdfFileName,RxXdfData-method, rxXdfFileName,character-method, rxXdfFileName,ANY-method, methods, file, connection" 
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
 
 
 
 
 
 
 
 #`rxXdfFileName-methods`: Retrieve .xdf file name

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Get the .xdf file path from a character string or RxXdfData object.
 
 
 ##Usage

```   
  rxXdfFileName( x )
 
```
 
 ##Arguments

   
    
 ### `x`
 character string containing the file name or an RxXdfData object. 
  
 
 
 ##Value
 
a character string containing the path and name of the .xdf file
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxXdfData](rxxdfdata.md)
   
 ##Examples

 ```
   
  # Create an RxXdfData object
  ds <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "claims.xdf"))
  # Retrieve the file name with path
  fileName <- rxXdfFileName(ds)
  fileName
  
 
```
 
 
 
 
