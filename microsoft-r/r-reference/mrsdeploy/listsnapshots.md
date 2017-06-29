--- 
 
# required metadata 
title: "Get a list of snapshots for the current user." 
description: " Get a list of all the snapshots on the R server that are available to the current user. " 
keywords: "mrsdeploy, listSnapshots" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 
 
 #listSnapshots: Get a list of snapshots for the current user.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Get a list of all the snapshots on the R server that are available to the current user.
 
 
 ##Usage

```   
  listSnapshots()
 
```
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
A character vector containing the snapshot ids.
 
 ##See Also
 
[createSnapshot](createsnapshot.md)

[deleteSnapshot](deletesnapshot.md)

[loadSnapshot](loadsnapshot.md)

[downloadSnapshot](downloadsnapshot.md)
   
 ##Examples

 ```
   
  ## Not run:
 
snapshots<-listSnapshots()
 ## End(Not run) 
  
 
```
 
 
