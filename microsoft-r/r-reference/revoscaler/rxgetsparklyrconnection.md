--- 

# required metadata 
title: "rxGetSparklyrConnection function (revoAnalytics) | Microsoft Docs" 
description: " Get a Spark compute context with sparklyr interop.  rxGetSparklyrConnection get sparklyr spark connection from created Spark compute context. " 
keywords: "(revoAnalytics), rxGetSparklyrConnection" 
author: "dphansen" 
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


 # rxGetSparklyrConnection: Get sparklyr connection from Spark compute context 
 ## Description
  Get a Spark compute context with sparklyr interop.
 `rxGetSparklyrConnection` get sparklyr spark connection from created Spark compute context.


 ## Usage

```   
  rxGetSparklyrConnection(
      computeContext = rxGetOption("computeContext"))

```


 ## Arguments



 ### `computeContext`
 Compute context get created by `rxSparkConnect`. 




 ## Value

object of sparklyr spark connection


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## Examples

 ```

  ## Not run:

library("sparklyr")
cc <- rxSparkConnect(interop = "sparklyr")
sc <- rxGetSparklyrConnection(cc)
iris_tbl <- copy_to(sc, iris)
 ## End(Not run) 
```

