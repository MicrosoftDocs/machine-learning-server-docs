--- 
 
# required metadata 
title: "Delete a snapshot from the R server." 
description: " Deletes the specified snapshot from the repository on the R Server. " 
keywords: "mrsdeploy, deleteSnapshot" 
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
 
 
 
 
 #`deleteSnapshot`: Delete a snapshot from the R server.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Deletes the specified snapshot from the repository on the R Server.
 
 
 ##Usage

```   
  deleteSnapshot(snapshot_id)
 
```
 
 ##Arguments

   
  
 ### `snapshot_id`
 Identifier of the snapshot to delete. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
`TRUE` if successful.
 
 ##See Also
 
[createSnapshot](createsnapshot.md)

[listSnapshots](../../mrsdeploy/packagehelp/listsnapshots.md)

[loadSnapshot](../../mrsdeploy/packagehelp/loadsnapshot.md)

[downloadSnapshot](down../../mrsdeploy/packagehelp/loadsnapshot.md)
   
 ##Examples

 ```
   
  ## Not run:
 
deleteSnapshot("8ce7eb47-3aeb-4c5a-b0a5-a2025f07d9cd")
 ## End(Not run) 
  
 
```
 
 
