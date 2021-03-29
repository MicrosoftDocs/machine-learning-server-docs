--- 

# required metadata 
title: "summary.snapshotDetails function (mrsdeploy) | Microsoft Docs" 
description: " Defines the R summary generic for snapshotDetails during a  listSnapshots(). " 
keywords: "(mrsdeploy), summary.snapshotDetails" 
author: "dphansen"
ms.author: "davidph" 
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
ms.technology: "" 
ms.custom: "" 

--- 




 # summary.snapshotDetails: The summary generic for `snapshotDetails`. 
 ## Description

Defines the R summary generic for `snapshotDetails` during a 
`listSnapshots()`.


 ## Usage

```   
 ## S3 method for class `snapshotDetails':
summary  (o)

```

 ## Arguments



 ### `o`
 The `snapshotDetails` list of S3 object. 



 ## See Also

Other snapshot methods: [print.snapshotDetails](print.snapshotDetails.md)

 ## Examples

 ```

  ## Not run:

# --- print all ---
summary(listSnapshots())
 ## End(Not run) 
```

