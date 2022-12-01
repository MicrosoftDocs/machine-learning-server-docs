--- 

# required metadata 
title: "rxXdfFileName-methods methods (revoAnalytics) | Microsoft Docs" 
description: " Get the .xdf file path from a character string or RxXdfData object. " 
keywords: "(revoAnalytics), rxXdfFileName-methods, rxXdfFileName, rxXdfFileName,RxXdfData-method, rxXdfFileName,character-method, rxXdfFileName,ANY-method, methods, file, connection" 
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







 # rxXdfFileName-methods: Retrieve .xdf file name 
 ## Description

Get the .xdf file path from a character string or RxXdfData object.


 ## Usage

```   
  rxXdfFileName( x )

```

 ## Arguments



 ### `x`
 character string containing the file name or an RxXdfData object. 



 ## Value

a character string containing the path and name of the .xdf file

 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[RxXdfData](RxXdfData.md)

 ## Examples

 ```

  # Create an RxXdfData object
  ds <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "claims.xdf"))
  # Retrieve the file name with path
  fileName <- rxXdfFileName(ds)
  fileName
```




