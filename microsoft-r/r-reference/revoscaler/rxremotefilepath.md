--- 
 
# required metadata 
title: "rxRemoteFilePath function (RevoScaleR) " 
description: "This is a utility function to automatically build a path according to file path rules of the remote compute context platform." 
keywords: "(RevoScaleR), rxRemoteFilePath, IO" 
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
 
 
 #rxRemoteFilePath: Merge path using the remote platform's separator. 
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
 
 
 
