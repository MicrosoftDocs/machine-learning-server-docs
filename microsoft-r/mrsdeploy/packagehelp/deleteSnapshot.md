--- 
 
# required metadata 
title: "Delete a snapshot from the server." 
description: " Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)  " 
keywords: "mrsdeploy, deleteSnapshot" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 
 #`deleteSnapshot`: Delete a snapshot from the server.

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Usage

```   
  deleteSnapshot(snapshot_id)
 
```
 
 ##Arguments

   
  
 ### `snapshot_id`
 Identifier of the snapshot to delete. 
  
 
 
 ##Details
 
Deletes the specified snapshot from the repository on the R Server.
 
 
 ##Value
 
`TRUE` if sucessful.
 
 ##See Also
 
[createSnapshot](createSnapshot.md)

[listSnapshots](listSnapshots.md)

[loadSnapshot](loadSnapshot.md)

[downloadSnapshot](downloadSnapshot.md)
   
 ##Examples

 ```
   
  ## Not run:
 
deleteSnapshot("8ce7eb47-3aeb-4c5a-b0a5-a2025f07d9cd")
 ## End(Not run) 
  
 
```
 
 
