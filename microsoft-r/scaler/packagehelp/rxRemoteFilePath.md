--- 
 
# required metadata 
title: "Merge path using the remote platform's separator." 
description: "This is a utility function to automatically build a path according to file path rules of the remote compute context platform." 
keywords: "RevoScaleR, rxRemoteFilePath, IO" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 #`rxRemoteFilePath`: Merge path using the remote platform's separator.

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 This is a utility function to automatically build a path
according to file path rules of the remote compute context platform. 
 
 ##Usage

```   rxRemoteFilePath(  ...  , computeContext) 
```
 
 ##Arguments

   
    
 ### ` ...`
 character strings containing parts of the path to be merged. 
  
    
 ### `computeContext`
 An [RxComputeContext](RxComputeContext.md) context object specifying the platform information. 
  
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxComputeContext](RxComputeContext.md),
[RxHadoopMR](RxHadoopMR.md),
[RxSpark](RxSpark.md).
   
 ##Examples

 ```
   
  ## Not run:
 
  # myHadoopCC is an RxHadoopMR compute context
   myFilePath <- rxRemoteFilePath("/var/RevoShare", "testDir", myHadoopCC)
 ## End(Not run) 
  
 
```
 
 
 
