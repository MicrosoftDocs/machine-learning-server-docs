--- 
 
# required metadata 
title: "The summary generic for snapshotDetails." 
description: " Defines the R summary generic for snapshotDetails during a  listSnapshots(). " 
keywords: "mrsdeploy, summary.snapshotDetails" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 
 
 #`summary.snapshotDetails`: The summary generic for `snapshotDetails`.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Defines the R summary generic for `snapshotDetails` during a 
`listSnapshots()`.
 
 
 ##Usage

```   
 ## S3 method for class `snapshotDetails':
summary  (o)
 
```
 
 ##Arguments

   
  
 ### `o`
 The `snapshotDetails` list of S3 object. 
  
 
 
 ##See Also
 
Other snapshot methods: [print.snapshotDetails](print.snapshotDetails.md)
   
 ##Examples

 ```
   
  ## Not run:
 
# --- print all ---
summary(listSnapshots())
 ## End(Not run) 
  
 
```
 
 
