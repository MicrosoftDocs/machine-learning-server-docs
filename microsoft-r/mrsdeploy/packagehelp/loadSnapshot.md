--- 
 
# required metadata 
title: "Load a snapshot from the server into the remote session (workspace and working directory)." 
description: " Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)  " 
keywords: "mrsdeploy, loadSnapshot" 
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
 
 
 
 
 #`loadSnapshot`: Load a snapshot from the server into the remote session (workspace and working directory).

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Usage

```   
  loadSnapshot(snapshot_id)
 
```
 
 ##Arguments

   
  
 ### `snapshot_id`
 Identifier of the snapshot to load. 
  
 
 
 ##Details
 
Loads the specified snapshot from the server into the remote session.The workpace
is updated with the objects saved in the snapshot, and saved files are restored to the working directory.
 
 
 ##Value
 
`TRUE` if successful.
 
 ##See Also
 
[createSnapshot](createSnapshot.md)

[deleteSnapshot](deleteSnapshot.md)

[listSnapshots](listSnapshots.md)

[downloadSnapshot](downloadSnapshot.md)
   
 ##Examples

 ```
   
  ## Not run:
 
loadSnapshot("8ce7eb47-3aeb-4c5a-b0a5-a2025f07d9cd")
 ## End(Not run) 
  
 
```
 
 
